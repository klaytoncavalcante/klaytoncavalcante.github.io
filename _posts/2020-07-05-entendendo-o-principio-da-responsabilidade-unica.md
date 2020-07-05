---
layout: post
title:  "Uma dica simples para entender o princípio da responsabilidade única"
date:   2020-07-05 11:50:00
categories: [Código limpo,Boas práticas,Programação]
---

Existe o seguinte ditado: “o papel aceita tudo”. Isso é quase totalmente adaptável para os conceitos de programação. Eu poderia dizer: “o código aceita quase tudo”. Linguagens de programação de escopo aberto possuem a característica de não restringir muito o que o programador pode fazer. Isso lhe dá a liberdade de escrever códigos dos mais diversificados, o que inclui escrever código ruim.

Para evitar que isso ocorra, existem vários padrões e princípios que podem ser seguidos para dar ao programador um trilho sobre o qual ele pode caminhar. Um dos conjuntos de princípios mais conhecidos é o SOLID. A sigla está em inglês e consiste de 5 princípios básicos:
- Single responsibility principle (princípio da responsabilidade única)
- Open/closed principle (princípio de aberto/fechado)
- Liskov substitution principle (princípio da substituição de Liskov)
- Interface segregation principle (princípio da segregação de interface)
- Dependency inversion principle (princípio da inversão de dependência.

Neste artigo falaremos sobre o primeiro deles e como atingi-lo de forma relativamente simples.

<!--more-->

A definição mais básica deste princípio seria:

> Uma classe deve ter um, e somente um, motivo para mudar.

Sim, o termo usado é “classe” mesmo. Este princípio não se aplica somente a funções. Se aplica também a classes. Mas neste artigo focarei mais em funções.

O princípio da responsabilidade única, portanto, indica que funções devem fazer uma coisa e somente uma. Aí você pode pensar: mas e quando eu preciso de mais coisas? Neste caso, uma função deve chamar outra função que faça esta outra coisa e somente ela. Entendeu o conceito? Cada função é um especialista e vários especialistas realizam atingem o objetivo trabalhando em conjunto.

A princípio isso pode parecer um pouco inalcançável, ainda mais naquele projeto legado que tá cheio de código velho e tralha pra todo lado. Mas mesmo nesses projetos é possível aplicar esse princípio sem maiores problemas.

Para isso tenho uma dica que pode ajudar bem.
Alinhe seu pensamento para planejar seu código da seguinte forma:

1. Preciso escrever um método que atinja o objetivo A
2. Para atingir A, é necessário fazer B, C e D.
3. Para atingir B, é necessário fazer E e F.
4. Para atingir C, não é necessário nada extra.
5. Para atingir D, é necessário fazer E e G.
6. E daí por diante.

Explicando com um pouco mais de detalhes, cada letra maiúscula na lista acima representa uma função. A função A tem um objetivo que para ser atingido é necessário chamar outras 3 funções, cada uma com uma responsabilidade: resolver parte do problema de A.

Estas outras funções podem subdividir ainda mais o problema em partes cada vez menores e, quando algo sai do escopo de sua responsabilidade, chama a função responsável por esse outro pedaço do problema.

Os objetivos disso são manter o código curto, fácil de entender, com menos duplicações de código (trechos de códigos que fazem a mesma coisa) e facilitar o reuso de funções. Se você observar, no exemplo acima a função E é chamada tanto no passo 3 como 5, porque ela resolve parte do problema em ambos os casos.

Esse foi o primeiro artigo sobre boas práticas e princípios que ajudam a escrever códigos de qualidade.

Se você quiser se aprofundar mais nos princípios do SOLID, recomendo a leitura do artigo [O que é SOLID: O guia completo para você entender os 5 princípios da POO](https://medium.com/joaorobertopb/o-que-%C3%A9-solid-o-guia-completo-para-voc%C3%AA-entender-os-5-princ%C3%ADpios-da-poo-2b937b3fc530).

Para um aprendizado mais profundo no conceito de código limpo, recomendo a leitura do livro “Clean Code”, de Robert C. Martin.

Até logo e bons códigos.