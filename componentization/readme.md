
# Pensando em componentização

Oi, meu nome é David Basilio, e eu já venho trabalhando com web a alguns anos, acho que 8 até agora, atualmente me considero um desenvolvedor Full-Stack com plus em Front-end. Focado mais em criação de site, no layout e seu desenvolvimento visual, mais que na criação de aplicativos web. Isso não quer dizer que não passei por tecnologias mais antigas como Backbone, RequireJS e Angular 1.6. 

Por focar mais no layout, isso me fez demorar um pouco mais para olhar de uma forma efetiva que a idéia de componentização que o React trouxe. E após experimentar criar os sites usando essa formato de criação comecei a pensar mais fácilmente em como estruturar meu código de estilo (CSS, SCSS).

No tutorial abaixo é um exemplo que eu acredito vá acontecer com frequencia na criação do site, talvez até chamar de um clássico no desenvolvimento de layout web. 

Antes de pular para o tutorial, vou mostrar uma forma caseira de criar um espaço de trabalho para testar a criação do estilo junto ao html. Atualmente eu uso a lib Storybook quando estou criando junto com alguma ferramenta de componentização, tipo VueJS.

## Usando html puro

Para o desenvolvimento do estilo não é preciso estar usando uma ferramenta como React, VueJs ou Angular. Você pode começar criando uma pasta com um nome que denomine que é um ambiente de criação e não para o produto final. Ex: previews, styleguide, style-components, etc. Depois criar um ou mais arquivos html apenas apontando para o CSS gerado pelo seu Bundler (Webpack, Gulp, Grunt), e adicionar essa estrutura para cada componente que for criando:


```html
<head>
  <!-- Se for ter mais que um arquivo com os componentes, pode jogar esse estilo para outro arquivo e importar aqui com a tag link -->
  <style>
    .preview .wrapper {
      padding: 60px 0 200px;
    }

    .preview .container {
      max-width: 1200px;
      padding: 0 30px;
      margin: 0 auto;
    }

    .preview .title {
      background: #c94968;
      padding: 5px 15px;
      color: white;
    }

    .preview section {
      margin-bottom: 2rem;
    }
  </style> 
</head>
<body class="preview">

  <div class="wrapper">
    <section>
      <div class="container">
        <p class="title">Link</p>
      </div>
      <!-- Your component below -->
      <p>No que você está pensando <a href="#">hoje?</a></p>
    </section>

    <section>
      <div class="container">
        <p class="title">Botões</p>
      </div>
      <!-- Your component below -->
      <button>Veja como chegar lá</button>
    </section>

    <section>
      <div class="container">
        <p class="title">Input</p>
      </div>
      <!-- Your component below -->
      <input type="text" placeholder="Pesquisar">
    </section>
  </div>
</body>
```

## Componentizando

O primeiro passo ao receber um layout para desenvolver sua versão web é fazer um exercício de componentização dos elementos que formam aquele layout. Usando a imagem abaixo como exemplo:

![Layout](./layout2.png)

Seguindo o exemplo acima, seria criado 3 componentes e uma View que junta os componentes para criar a página por assim dizer:

- Infocard
- InfocardList
- HomeView

## Estrutura e Estilo

### Infocard

O primeiro seria o InfocardItem em si, contendo uma imagem, um titulo e uma descrição. Ficando sua estrutura da seguinta forma:

```html
<div class="infocard">
  <figure class="infocard__image">
    <img src="">
  </figure>
  <h2 class="infocard__title">Title</h2>
  <p class="infocard__description">Lorem ...</p>
  <button class="infocard__button">Ver mais</button>
</div>
```

```scss
.infocard {
  .infocard__image {}
  .infocard__title {}
  .infocard__description {}
  .infocard__button {}
}
```

Essa estrutura pode acabar tendo mais classes de ajuda durante o desenvolvimento, como `.infocard__wrap`, ou em separando em regiões como: `.infocard__header`, `.infocard__main`, `.infocard__footer`. Mas sempre deixando para casos que realmente precisem.

Um ponto importante aqui, é não adicionar estilo de dimensão ou posicionamento ao  `.inforcard`, assim como margens e posicionamento. Quem vai ficar responsável por esses estilos será os outros componentes que irão utiliza-lo. Deixando ele o mais reútilizavel possível.

### InfocardList

A lista de infocards básicamente é um container utilizando vários `Infocard`, sua estrutura ficaria assim:

```html
<div class="inforcard-list">
  <div class="infocard-list__title">Title</div>
  <div class="infocard-list__wrap">
    <div class="infocard-list__item infocard">
      ...
    </div>
    <div class="infocard-list__item infocard">
      ...
    </div>
    <div class="infocard-list__item infocard">
      ...
    </div>
  </div>
</div>
```

```scss
.infocard-list {
  .infocard-list__title {}
  .infocard-list__wrap {}
  .infocard-list__item {
    margin: 0 5px;
  }
}
```

Por mais que seja uma estrutura bem simples, esse será o componente que será responsável por organizar os `Infocard`, fornecendo estilo de dimensão como margens. Outras opções seria: usar `position`, `flexbox`, `grid`.


### HomeView

Aqui seria a última camada, juntando o `InfocardList` e qualquer outro componente para formar a página ou a view. Nesse caso, poderia ter um componente com informações do site como um `Dashboard`.

```html
<div class="home-view">
  <div class="home-view__dashboard">
    ...
  </div>
  <div class="home-view__inforcard-list">
    ...
  </div>
</div>
```

```scss
.home-view {
  .home-view__dashboard {}
  .home-view__infocard-list {}
}
```

## Scripting It With Vue

### Estados

Utilizando o componente `Infocard` como exemplo, agora poderiamos colocar ele dentro de um componente VueJs e assim adicionar estados ao componente que mudariam seu estilo:

```js
v-bind:class="{'selected': isSelected}"
```

```js
props: {
  isSelected: Boolean
}
```

```html
// Infocard.vue
<template>
  <div class="infocard" v-bind:class="{'selected': isSelected}">
    ...
  </div>
</template>

<script>
export default {
  name: "Infocard",
  props: {
    isSelected: Boolean
  }
}
</script>
```

Esse é um exemplo que ao instanciar esse componente e ao passarmos o parametro `isSelected`, o componente possuirá o classe `selected` podendo ser adicionado regras de estilo que irão mudar o visual do componente.

### Eventos e Responsabilidades

Nesse mesmo componente nós possuimos um botão com titulo `Ver mais`, o componente sozinho não sabe o que deve acontecer ao clica-lo. Então devemos subir a responsábilidade para quem está utilizando-o. Segue um exemplo de como emitir um evento ao clicar no botão:

```html
<button class="infocard__button" v-on:click="onClick">Ver mais</button>
```

```js
methods: {
  onClick: function() {
    this.$emit('onClickChild');
  }
}
```

```html
// Infocard.vue
<template>
  <div class="infocard" v-bind:class="{'selected': isSelected}">
    ...
    <button class="infocard__button" v-on:click="onClick">Ver mais</button>
  </div>
</template>

<script>
export default {
  name: "Infocard",
  props: {
    isSelected: Boolean
  },
  methods: {
    onClick: function() {
      this.$emit('onClickChild');
    }
  }
}
</script>
```

Aqui nós adicionamos um emiter de evento com nome `onClickChild` para que o componente pai possa adicionar uma função para escutar o evento cada vez que ele for emitido. 

Ok, agora já temos um componente com todo seu comportamento isolado, podendo ser reutilizado em qualquer contexto. 

#### Escutando um evento

Segue um exemplo de um componente pai escutando o evento de click do botão e carregando uma nova tela.

```html
// ParentTest
<template>
  <div class="parent-test" >
    <Infocard v-on:onClickChild="onClickParent" />
  </div>
</template>

<script>
import Infocard from "./Infocard.vue";

export default {
  name: "ParentTest",
  methods: {
    onClickParent: function() {
      this.$route.push({name: 'Post1'});
    }
  },
  ...
}
</script>
```

