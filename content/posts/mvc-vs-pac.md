---
title: "MVC vs PAC"
date: 2018-03-27T12:17:36-03:00
image: /images/posts/MVC-vs-PAC.png
draft: false
---

Fala galerinha do bem! Hoje é o meu primeiro dia de trabalho na [Taller](http://taller.net.br), essa empresa 100% remota que já conquistou meu coração antes mesmo de eu começar a trabalhar aqui.

Bom, hoje recebi um artigo sobre arquitetura e eis que conheço o PAC! Você também não conhece? Não tem problema, aparentemente esse modelo de arquitetura não é muito difundido quanto seu parente mais próximo, o MVC, mas tem sua importância também.

Vamos falar um pouco sobre MVC e depois sobre o PAC, pra no fim dar uma comparada nos dois.

## MVC

![modelo-visão-controlador](/images/posts/mvc-1-300x160.png)

Pois é, o velho MVC =P com toda certeza muito usado ainda hoje.

Tem um conceito bem simples de separação de camadas. Existe um model (M) que representam seus dados, e está relacionado em 99.9% dos casos com sua base de dados. Tem uma view (V) que é apresentada pro seu usuário, onde ele pode visualizar informações provenientes da sua base, ou formulários para gerenciar/alterar/incluir dados. Por fim tem um controller (C) que faz o intermédio entre o model e a view. Geralmente no controller você obtém um model através de um repositório, faz alguns tratamentos em cima dos dados (ou não) e os repassa para serem exibidos na view, ou ainda, os recebe através da view, faz algum tratamento, e os salva na base através de um repositório.

Enfim, essa é a explicação superficial para como é o fluxo nesse padrão de arquitetura. Existem muitas formas de implementar isso, e vários frameworks que funcionam dessa maneira. Pra entender um pouco mais do assunto, deixo aqui o link pra [Wikipedia](https://pt.wikipedia.org/wiki/MVC?oldformat=true) ;)

## PAC

![presenter-abstraction-controller](/images/posts/PAC-300x230.png)

No PAC nós temos uma estrutura um pouco parecida com a do MVC, porém ela se repete várias vezes dentro da aplicação, agrupadas no que chamamos de **Agent** ou **Triad**. Cada um destes agentes tem uma estrutura composta por um Presenter (P), por uma Abstraction (A), por um Controller (C) e podem ser tão simples quanto um objeto ou, tão complexos quanto todo um software. Vamos ver um pouco mais sobre a função de cada componente destes agentes:

- **Presenter**: Como a view do *MVC*, este componente é usado para exibir/obter informações.

- **Abstraction**: Este componente recebe e processa informações, algo próximo ao model no *MVC* com a diferença de que possui uma parte limitada da informação, a que compete ao seu escopo.

- **Controller**: Assim como no *MVC*, este componente controla o fluxo entre o Presenter e a Abstraction. Porém aqui temos a diferença de que este controller também passa informações para outros agentes.

Seguindo nesta linha, vamos ao exemplo clássico quando se fala sobre PAC:

*“Imaginemos um sistema para controle do tráfego aéreo. Um dos agentes recebe informações de um sistema de radar sobre a localização de um 747, e usa estas informações para através do seu componente Presenter, plotar um pontinho na interface. Outro agente recebe informações de um DC-10 decolando e também plota um pontinho na interface. Um outro agente, recebe informações sobre condições do tempo e plota algumas nuvens na interface.”*

Vendo por esta perspectiva, fica mais fácil imaginarmos que cada um dos agentes é uma forma de componente que tem uma responsabilidade única, e que faz parte de algo maior, um ecossistema composto por vários agentes.

## E aí?

Comparando os dois modelos, você pode perceber na imagem que ilustra o fluxo do MVC, que este tem uma deficiência na separação das camadas sendo que, por vezes, uma atualização em algum dado no model dispara uma atualização direto na view, chegando a este fluxo:

![mvc-flow](/images/posts/MVC-–-2.png)

Devido a este problema, na tentativa de tornar essa separação mais clara, outros modelos da própria implementação do MVC surgiram. Problema este resolvido no modelo PAC, onde obrigatoriamente todo o fluxo da informação e atualização é capturado e controlado pelo controller, chegando a este modelo:

![pac-flow](/images/posts/PAC-–-2.png)

Outro “problema” conhecido no modelo MVC, é de que múltiplas interfaces e controllers operam sobre o mesmo dado, o que causa uma alteração destes dados de diversas fontes diferentes. Problema solucionado no PAC no sentido de que cada agente cuida de uma porção menor da informação e qualquer outro agente que necessite de tal informação vai,  via controller, “conversar” com outro agente.

Por fim, há muito para se falar sobre ambas as arquiteturas, mas esta aqui é uma pincelada mais abstrata na tentativa de exemplificar de uma forma simples, como cada uma delas funciona e difere entre si.

Espero ter cumprido com meu objetivo, muito obrigado e até a próxima!