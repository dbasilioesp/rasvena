# Componentização

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

