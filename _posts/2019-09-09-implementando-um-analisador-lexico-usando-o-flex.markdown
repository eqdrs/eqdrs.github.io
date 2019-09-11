---
layout: post
title:  "Implementando um analisador léxico usando o Flex"
date:   2019-09-08 12:00:00
categories: ['Compilers']
---

A análise léxica é a primeira fase do processo de compilação de um programa, responsável por fazer a leitura do código fonte, caractere a caractere, especificando os tokens e lexemas da linguagem. Neste post, iremos implementar e testar um analisador léxico para uma linguagem inspirada em C. E faremos isso usando o Flex!

O [Flex][flex] (Fast Lexical Analyzer Generator) é uma ferramenta gratuita e open-source para geração de analisadores léxicos. Ela foi escrita em C, por volta de 1987, pelo Professor [Vern Paxson][vern].

##### Instalando o Flex
<p />

Para instalar o Flex no Ubuntu, basta abrir o seu terminal (Ctrl + Alt + T) e executar os comandos abaixo:

<pre>
sudo apt-get update
sudo apt-get install flex
</pre>

Pronto! Já podemos utilizar os recursos do Flex!

##### Definindo uma convenção léxica para a linguagem
<p />

Nossa linguagem será simples e procedimental, um subconjunto inspirado na linguagem C, que inclui somente variáveis inteiras, array de inteiros, funções, condição, e repetição. A linguagem seguirá as convenções definidas a seguir.

- Na implementação, iremos considerar as seguintes classes de tokens na linguagem:

<pre>
  <b>ID</b>      Identificador
  <b>NUM</b>     Literal decimal (inteiro)
  <b>KEY</b>     Palavra-chave
  <b>SYM</b>     Símbolos léxicos
  <b>ERROR</b>   Lexema do primeiro erro encontrado
</pre>

- As palavras revervadas (keywords) serão as seguintes:

<pre>
  <b>else   if   int   return   void   while</b>
</pre>

Elas **não** poderão ser usadas como nomes de variáveis/funções, e devem ser escritas em minúsculo.

- Os símbolos especiais serão:

<pre>
  <b>!  &&  ||   +  -  *  /  <  <=  >  >=  ==  !=  =  ;  ,  (  )  [  ]  {  }  /*  */</b>
</pre>

- Os outros lexemas são o ID e NUM, definidos pelas expressões regulares abaixo:

<pre>
  <b>ID</b> = letter (letter | digit)*
  <b>NUM</b> = digit digit*
  <b>letter</b> = a | .. | z | A | .. | Z
  <b>digit</b> = 0 | .. | 9
</pre>

- Haverá distinção entre letras em maiúsculo e minúsculo.

- Os espaços em branco consistem em caracter branco (' '), quebra-de-linha ('\n'), e tabs ('\t'). Eles deverão ser ignorados, exceto quando for necessário separar os ID's e NUM's, e palavras reservadas.

- Comentários serão escritos da mesma forma que na linguagem C, usando a notação /* ...  */. Poderão ser inseridos em qualquer parte do código onde é permitido colocar espaços em branco, isto é, comentários **não** poderão ser inseridos dentro de tokens, e podem incluir mais de uma linha.

##### Desenvolvendo o arquivo Flex
<p />

O código flex é dividido em três seções:

<pre>
  <b>definições</b>
  %%
  <b>regras</b>
  %%
  <b>código do usuário</b>
</pre>

Para a linguagem que definimos, criaremos um arquivo chamado `lexico.l` com o código abaixo. Notem que ele é dividido em três seções, onde, na seção intermediária, definimos a ação a ser tomada para cada *pattern*. 

```l
digit[0-9]
letter[a-zA-Z]
ID[a-zA-Z][a-zA-Z0-9]*
WHITESPACE[ ]
quebra[\n]
TAB[\t]

%{
    #define YY_DECL extern "C" int yylex()
    #include<string>
    #include<iostream>
    using namespace std;
    FILE *out ;
	int linha;
%}
%option yylineno
%x COMMENT

%%

{quebra}

"/*" { linha=yylineno; BEGIN(COMMENT); }

<COMMENT>"*/" { BEGIN(INITIAL); }

<COMMENT>(.|\n);

<COMMENT><<EOF>> {fprintf(out,"(%d,ERROR,\"/*\")\n",linha); return 0;}

else|if|int|return|void|while {fprintf(out,"(%d,KEY,\"%s\")\n",yylineno ,yytext);} 

"+"|"-"|"*"|"/"|"<"|"<="|">"|">="|"=="|"!="|"="|";"|","|"("|")"|"["|"]"|"{"|"}" {fprintf(out,"(%d,SYM,\"%s\")\n",yylineno,yytext);}

{WHITESPACE}+|{quebra}|{TAB}+
 
{digit}+ {fprintf(out,"(%d,NUM,\"%s\")\n",yylineno,yytext);}

{digit}+{ID}+ {fprintf(out,"(%d,ERROR,\"%s\")\n",yylineno,yytext); return 0;}

{ID}+ {fprintf(out,"(%d,ID,\"%s\")\n",yylineno,yytext);}

. {fprintf(out,"(%d,ERROR,\"%s\")\n",yylineno,yytext); return 0;}

%%

int yywrap();

int main(int argc, char *argv[]){
    FILE *arquivo = fopen(argv[1],"r");
    if (!arquivo) {
      cout << "Arquivo inexistente" << endl;
      return -1;
    }
    yyin = arquivo;
    out = fopen(argv[2],"w");
    yylex();
    return 0;
}

int yywrap(){
    return 1;
}
```

##### Gerando o arquivo lex.yy.c
<p />

Após criarmos o arquivo `lexico.l` com as regras para o nosso analisador léxico, vamos gerar um arquivo chamado `lex.yy.c`, usando o Flex. Para isso, basta executar o comando abaixo no terminal:

<pre>
flex lexico.l
</pre>

Isso irá criar o `lex.yy.c` no mesmo diretório. Esse arquivo contém uma função chamada `yylex()`, que retorna 1 sempre que uma expressão especificada for encontrada no arquivo de entrada, e 0 quando o final do arquivo for atingido. Cada chamada à função `yylex()` analisa um token; quando a função é chamada novamente, ela começa de onde parou.

##### Gerando o executável

Para gerar o executável, vamos compilar o arquivo gerado pelo Flex através do comando:

<pre>
g++ lex.yy.c -lfl -o lexico
</pre>

##### Testando nosso analisador léxico
<p />

Agora vamos testar nosso analisador léxico! Para isto, iremos criar um arquivo de exemplo chamado `main.c`, que irá conter o nosso programa-fonte:

{% highlight c linenos %}
/* main.c */
void main(void){
  int a;

  a = 10;  /* Testando
              comentário
              em bloco. */
  if(a > 9){
    a = (4 + 5) * 3;
  }
}
{% endhighlight %}

Para iniciar a análise léxica, basta executar o seguinte comando no terminal:

<pre>
./lexico main.c main.lex
</pre>

Com isso, será gerado um arquivo chamado `main.lex` no mesmo diretório, contendo o resultado da análise léxica do nosso código-fonte:

{% highlight c %}
(2,KEY,"void")
(2,ID,"main")
(2,SYM,"(")
(2,KEY,"void")
(2,SYM,")")
(2,SYM,"{")
(3,KEY,"int")
(3,ID,"a")
(3,SYM,";")
(5,ID,"a")
(5,SYM,"=")
(5,NUM,"10")
(5,SYM,";")
(8,KEY,"if")
(8,SYM,"(")
(8,ID,"a")
(8,SYM,">")
(8,NUM,"9")
(8,SYM,")")
(8,SYM,"{")
(9,ID,"a")
(9,SYM,"=")
(9,SYM,"(")
(9,NUM,"4")
(9,SYM,"+")
(9,NUM,"5")
(9,SYM,")")
(9,SYM,"*")
(9,NUM,"3")
(9,SYM,";")
(10,SYM,"}")
(11,SYM,"}")
{% endhighlight %}

Podemos ver que o nosso analisador léxico identificou todos os tokens e suas respectivas classes, como era esperado! Além disso, todos os comentários, tabs e quebras de linha foram ignorados, como também já prevíamos. \o/

No processo de compilação de um programa, essas informações são enviadas posteriormente para o analisador sintático, responsável por gerar uma estrutura de dados a partir do código-fonte e verificar se ela está de acordo com a gramática que foi definida para a linguagem. Mas isso já é assunto para um próximo post!

Caso queira se aprofundar ainda mais nessa área, recomendo o [curso de compiladores da Stanford][stanford] (em inglês).

Obrigado a tod@s, e até mais! 😊

[flex]: https://www.gnu.org/software/flex/
[vern]: https://en.wikipedia.org/wiki/Vern_Paxson
[stanford]: https://lagunita.stanford.edu/courses/Engineering/Compilers/Fall2014/about
