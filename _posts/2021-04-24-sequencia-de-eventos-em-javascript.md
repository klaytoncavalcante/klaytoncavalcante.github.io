---
layout: post
title:  "Controlando a sequência dos eventos em Javascript"
date:   2021-04-24 15:36:00
categories: [Programação, Javascript, Boas práticas]
---

Uma das maiores causas de bugs em códigos Javascript são eventos e como eles são executados. Uma má gestão da sequência em que eventos são disparados leva não só a mais bugs como aumenta a dificuldade da correção deles e da inserção de novas features no mesmo componente ou página.

### Primeiramente, o que são eventos?

O nome já dá uma pequena ideia do que é. Eventos é a forma como lidamos com efeitos externos que podem acontecer a qualquer momento. O exemplo mais simples é o clique do mouse. Você não sabe quando vai acontecer, mas quando acontecer, o código estará preparado para executar uma ou mais funções específicas para este evento.

Outros eventos podem não ter intervenção direta do usuário. Por exemplo, o event `onLoad` do documento. Ele será disparado quando a página terminar de carregar.

<!--more-->

### Javascript event loop

Como você provavelmente já sabe, uma aplicação Javascript roda geralmente em uma única thread, com algumas poucas excerções, como Workers. Para lidar com o processamento de tudo que precisa ser executado, alguém é responsável por orquestrar tudo isso. O nome desse alguém é Javascript Event Loop.

Olhando um pouco embaixo do capô para entender como isso funciona no motor do Javascript, o event loop é uma sequência de execução que consiste de um loop infinito (executando sempre enquanto a aplicação estiver rodando) que está sempre observando por novas funções que devem ser executadas. Ele cria um tipo de fila do que precisa ser executado e cada função roda no momento certo, na sequência em que suas execuções foram agendadas.

Mas como o event loop em si não é o foco deste artigo, [vou deixar este vídeo](https://www.youtube.com/watch?v=8aGhZQkoFbQ) que explica muito bem como o event loop funciona na prática. (Talvez mais tarde eu escreva mais sobre o event loop)

Como o Javascript contém toda uma miríade de eventos possíveis, para tentar manter um pouco o foco deste artigo vou me restringir aos eventos do mouse.

### Mouse events

#### Click events

Acredito que estes sejam os eventos mais utilizados em qualquer aplicação, portanto um bom ponto para começarmos.

Quando pressionamos e soltamos o botão esquerdo do mouse, 3 eventos são disparados, nesta ordem:

1. mousedown: Representa o momento em que o botão foi pressionado
2. mouseup: Representa o momento em que o botão foi solto
3. click: Representa a ação completa de pressionar E soltar o botão do mouse.

Bem simples, certo? O problema acontece quando tentamos plugar funções em dois ou mais destes eventos. Como garantir a sequência em que cada uma função será executada?

**O primeiro passo é**: se cada função estiver em um dos eventos exclusivamente, cada uma será executada na sequência que os eventos são executados, que é a sequência que exemplifiquei acima.

**O segundo passo é**: se um mesmo evento tem mais de uma função associada, elas serão executadas na sequência em que foram associadas. Isso permite, inclusive, o cancelamento da execução das demais funções na sequência, se uma das funções anteriores chamar o método `event.stopImmediatePropagation()`.

**O terceiro passo é**: alguns events propagam de pai para filho. Neste caso, a sequência de execução vai do filho até a raiz. Se algum elemento neste caminho estiver esperando o mesmo evento, ele vai disparar a função associada. Exemplo: uma div com `onclick` onde esta div também contém um botão com `onclick`. Ao clicar no botão, o evento de click do botão será executado e em seguida o evento de click da div será executado. Caso não seja esta a intenção, é possível cancelar a propagação do evento chamando a função `event.stopPropagation()` na função mais interna, em nosso exemplo, no click do botão.

### Outros eventos

Vários outros eventos possuem formas onde um único evento lida com todas as fases da ação (similar ao `onclick`) e possuem eventos específicos para cada fase (similar ao `onmousedown` e `onmouseup`).

Alguns exemplos: 
1. mouseover
2. mouseenter
3. mousemove (executado enquanto o mouse se mover sobre o elemento)
4. mouseout
5. mouseleave


### Extras: 
Fica aqui então o que eu acredito que seja a principal dica: busque ponderar bem quando cada evento deve acontecer. É tentador colocarmos o nosso evento num momento bem à frente dos outros pra "garantir que ele será o último executado". Porém isso abre o risco de, caso queiramos que algum outro evento seja executado após este, será bem complexo de implementar.

Para conhecer outros eventos e como eles funcionam, recomendo a leitura da [documentação no MDN](https://developer.mozilla.org/en-US/docs/Web/Events#event_listing).

Espero que este artigo tenha lhe ajudado a entender um pouco mais sobre eventos no Javascript e como manter uma boa relação entre os eventos.

Até mais e bons códigos!