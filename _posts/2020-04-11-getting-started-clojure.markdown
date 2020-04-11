---
layout: post
title:  "Primeiros passos com Clojure!"
image: "/assets/images/clojure.png"
date:   2020-04-11 12:00:00
categories: ['Clojure']
---

Nesse período de quarentena, tenho ficado mais tempo em casa e aproveitado pra estudar algumas coisas legais. Uma delas é o **Clojure**, um dialeto variante da linguagem [Lisp][lisp] e, portanto, predominantemente funcional. Neste post, quero compartilhar com vocês o material que estou usando para dar meus primeiros passos na linguagem.

Como dito acima, Clojure é uma linguagem de programação variante do Lisp. Em outras palavras: parênteses intermináveis e sintaxe esquisita. Ela é executada pela JVM (apesar de existirem outras implementações) e preza pelo [princípio da imutabilidade][imutability] e por estruturas de dados persistentes, o que facilita a criação de sistemas concorrentes e torna o código bem mais previsível. Como uma variante do Lisp, possui um console interpretador (repl) semelhante ao do Ruby (irb). Grandes empresas como Netflix, Nubank, eBay e Adobe utilizam o Clojure. Você pode conferir a lista completa [aqui][companies].

<br />
<img class="center"  src="/assets/images/clojure.png">
<br />

Primeiro, vale comentar que em [ClojureDocs.org][clojuredocs] e em [Clojure.org][clojureorg] você encontra a documentação, referências úteis e vários exemplos de uso da linguagem. Além disso, o uso da [IntelliJ IDE][intellij] em conjunto com o plugin [Cursive][cursive] irá facilitar bastante o desenvolvimento dos seus projetos em Clojure.  

Se, assim como eu, você gosta de aprender novas linguagens através da resolução de problemas, recomendo o [Clojure Koans][clojurekoans], um repositório do GitHub com vários exercícios de Clojure. Para iniciar os desafios, você precisará instalar a JDK e o [Leiningen][leiningen]. Em seguida, basta clonar o repositório e rodar o koans usando o comando:

```bash
$ lein koan run
```

Outra opção divertida é o [4Clojure][4clojure]. Lá, você encontra exercícios interativos em vários níveis. Basta fazer o cadastro e iniciar os desafios online!

Também recomendo o [WonderLand Clojure Katas][alice] e o [Parens of the Dead][parens]. O primeiro é um conjunto de exercícios de Clojure inspirado no mundo de *Alice in Wonderland*. Já o [Parens of the Dead][parens] é um screencast de jogos com temática de zumbis, escritos em Clojure e ClojureScript.

Além dos repos e ferramentas citados acima, o livro **Clojure for the Brave and True** está disponível gratuitamente [neste link][brave]. Ele possui tópicos que vão desde a configuração do ambiente até temas mais avançados da linguagem, como programação paralela e concorrente usando Clojure. Caso prefira, você pode comprar uma cópia do livro no [site da Amazon][amazon].

No [ClojureBridge][clojurebridge], é possível encontrar workshops gratuitos de programação Clojure para iniciantes em grupos sub-representados na tecnologia. No ano passado, inclusive, eles organizaram um workshop na minha cidade natal, Salvador, voltado para a população negra.

No YouTube, encontrei [esse curso de introdução ao Clojure][youtube] ministrado por [Mats Gisselson][mats]. Alguns cursos online pagos também estão disponíveis na [Udemy][udemy], mas ainda não me inscrevi em nenhum deles.

Para finalizar, deixo [esse desafio][desafio] de **ClojureScript** (versão do Clojure que compila para JavaScript) do [ClojureScript Koans][clojurescriptkoans] para vocês se divertirem.

É isso, espero que tenham gostado! Se cuidem, e **#fiquememcasa**! 😉

[lisp]: https://en.wikipedia.org/wiki/Lisp_(programming_language)
[clojuredocs]: https://clojuredocs.org
[clojureorg]: https://clojure.org
[imutability]: https://medium.com/@BobGuBobGu/immutability-in-clojure-53476dcb31a3
[companies]: https://clojure.org/community/companies
[4clojure]: http://www.4clojure.com/
[alice]: https://github.com/gigasquid/wonderland-clojure-katas
[parens]: http://www.parens-of-the-dead.com/
[clojurekoans]: https://github.com/functional-koans/clojure-koans
[brave]: https://www.braveclojure.com/clojure-for-the-brave-and-true/
[amazon]: https://www.amazon.com.br/Clojure-Brave-True-Ultimate-Programmer/dp/1593275919
[leiningen]: https://github.com/technomancy/leiningen
[clojurebridge]: https://clojurebridge.org/
[udemy]: https://www.udemy.com/course/learning-clojure/
[clojurescriptkoans]: http://clojurescriptkoans.com/
[desafio]: http://clojurescriptkoans.com/#equality/1
[youtube]: https://www.youtube.com/watch?v=jLsddw8_bt4&list=PLXsXu5srjNlxI7b2smnHxDeMMwR4mVZ2m&index=2
[mats]: https://twitter.com/matgis
[intellij]: https://www.jetbrains.com/idea/download/#section=linux
[cursive]: https://cursive-ide.com/userguide/