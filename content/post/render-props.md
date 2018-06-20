---
title: "Render Props para reutilização de componentes"
date: 2018-06-06T12:49:41-03:00
image: /images/posts/render-props.png
---

A sua, a minha, a nossa!

Fala minha gente, tudo certinho? Faz pouco mais de um mês que fui agraciado com o conhecimento sobre *Render Props* e hoje tô aqui pra passar pra vocês o que tem acontecido desde então.

## API

*Render Prop* já está no React faz um bom tempo, porém sua utilização ainda não era recomendada dados alguns problemas. Mas, a API foi completamente reescrita e, a partir da versão 16.3, ela se tornou oficial. Você com certeza vai encontrar muitos projetos utilizando essa técnica de criação e reutilização de componentes.

## Re-utilizando de verdade!

Bom, um dos grandes benefícios que *Render Props* traz, na minha visão, é a verdadeira **RE-UTILIZAÇÃO**, além de ser muito simples. Você de fato cria componentes que podem ser reutilizados por todo o seu código de uma forma bem declarativa, tornando seu código mais limpo.

Permite também você usar ainda mais componentes stateless, o que em grandes aplicações traz um benefício em performance, já que você diminui a necessidade de implementação de lógica e verificação de *repaint* (PS: Para entender melhor como funciona o ciclo de repaint, de uma lida aqui: https://reactjs.org/docs/react-component.html) em vários pontos do app.

Vamos dar uma olhada em como isso funciona na prática com um exemplo bem simples. Vamos imaginar que queremos saber a quantidade em pixels que a página foi *“scrollada”*. Podemos montar um simples componente da seguinte forma:

```javascript
class ScrollSpy extends PureComponent {
  state = {
    pixelsFromTop: 0
  }

  componentDidMount () {
    this.spyScroll()
  }

  spyScroll = () => {
    window.addEventListener('scroll', () => {
      this.setState({
        pixelsFromTop: window.pageYOffset
      })
    })
  }

  render () {
    const { children } = this.props
    const { pixelsFromTop } = this.state

    return children(pixelsFromTop)
  }
}
```

Pronto, agora temos um componente que “atacha” um evento ao *window* que monitora o scroll da página, assim que o usuário rolar ela pra cima ou para baixo, a distância em *pixels* vai ser definida no state e devolvida para a função filha.

Você deve prestar atenção para o que muda aqui: perceba que, no método *render*, estamos executando a função ***children***, ou seja, ao invés de estar renderizando algo, estamos invocando uma função que será executada para aí sim renderizar o que recebermos como children do nosso componente **ScrollSpy**.

Dessa forma, quando a gente for utilizar esse componente, receberemos uma função, com os parâmetros que passamos para *children*. Veja como a utilização é simples:

```javascript
...
<ScrollSpy>
  // Parâmetros que passamos para o children
  { pixelsFromTop => (
    <span>Distance: { pixelsFromTop }px</span> // O que será renderizado
  ) }
</ScrollSpy>
...
```

E é isso, agora sempre que você precisar da distância em *pixels*, você pode usar o *ScrollSpy* e ele te dará essa informação. Da forma “tradicional” você teria de implementar a lógica em cada componente que você quisesse obter essa informação, ou adicionar a um *state* global, usar *redux*, ou algo nesse sentido. Vai dizer, muito mais simples assim não é?

Continuando, o componente *ScrollSpy* também poderia receber props como qualquer outro componente, você poderia capturar essas props, e criar alguma lógica em cima disso. Vamos dar uma olhada no componente ***Query do Apollo***.

```javascript
<Query query={userQuery}>
  { ({ loading, error, { user } }) => (
    <span>Name: { user.name }</span>
  ) }
</Query>
```

Perceba que estamos passando uma prop *query* para o componente ***Query***. Essa *query* será executada e retornará informações do usuário. Além disso, enquanto a *query* está sendo executada, o componente nos informa através do parâmetro loading que a requisição ainda não acabou, e se houver algum erro, através do parâmetro error. Você pode ler mais sobre o componente ***Query*** e outros do ***Apollo** na doc oficial: https://www.apollographql.com/docs.

Dados esses exemplos, quero deixar o link do github para um projeto que acho muito bacana e que é construído usando esse conceito de *render prop*. É o projeto do *Renato Ribeiro*, ***react-powerplug*** -> https://github.com/renatorib/react-powerplug.

## HELLLL

Bom, quando você começa a usar *render prop*, acaba se apaixonando. E você vai perceber que qualquer coisa agora poderia virar uma *render prop*. Mas cuidado, isso pode ser uma armadilha! Sempre analise bem se realmente o que você quer fazer, vale aplicar a técnica. Nesse sentido, você lembra do *CALLBACK HELL*? Se não lembra ou não sabe o que é, você pode ler sobre aqui: http://callbackhell.com.

*Callback Hell* foi tão comentado a um tempo atrás que ganhou até um site =P Aqui a gente pode encontrar outro problema, dá só uma olhada:

```javascript
...
<RP1>
  { param1 => (
    <RP2>
      { param2 => (
        <span>HELLLL!!! { param1 } - { param2 }</span>
      ) }
    </RP2>
  ) }
</RP1>
...
```

Você já pode imaginar onde esse código iria parar né? Para tal problema, existe um projeto bem bacana que nos ajuda a resolver isso, é o ***react-adopt***! O projeto é do *Pedro Nauck*, já deixo aqui o link para você conferir: https://github.com/pedronauck/react-adopt.

Neste caso, aplicando o *react-adopt* teríamos o seguinte resultado:

```javascript
...
const Composed = adopt({
  param1: <RP1 />,
  param2: <RP2 />
})

<Composed>
  { ({ param1, param2 }) => <span>Agora SIM!<span> }
</Composed>
...
```

Muito melhor não é mesmo?

## See you

Então é isso aí minha gente, espero que tenham gostado! E antes de ir, deixo também o link da doc oficial: https://reactjs.org/docs/render-props.html – Sempre é bom ler direto da fonte \o/

Abraços galera, e até a próxima.
