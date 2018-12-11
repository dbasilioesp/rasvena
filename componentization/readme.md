# Componentização

O primeiro passo ao receber um layout para desenvolver sua versão web é fazer um exercício de componentização dos elementos que formam aquele layout.

![](./layout.png)

Seguindo o exemplo acima, seria criado 3 componentes e uma View que junta os componentes para criar a página por assim dizer:

- ProductItem
- ProductList
- StoreView

## ProductItem

### Estrutura e Estilo

O primeiro seria o ProductItem em si, contendo a imagem do produto, o nome e o preço. Ficando sua estrutura da seguinta forma:

```html
<div class="product-item">
  <div class="product-item__image">
    <img src="">
  </div>
  <h2 class="product-item__title">Title</h2>
  <div class="product-item__price">R$ 15,00</div>
</div>
```

```scss
.product-item {
  .product-item__image {}
  .product-item__title {}
  .product-item__price {}
}
```

Essa estrutura pode acabar tendo mais classes de ajuda durante o desenvolvimento, como `.product-item__wrap`, ou em separando em regiões como: `.product-item__header`, `.product-item__main`, `.product-item__footer`. Mas sempre deixando para casos que realmente precisem.

Um ponto importante aqui, é não adicionar estilo de dimensão ou posicionamento ao  `.product-item`, assim como margens e posicionamento. Quem vai ficar responsável por esses estilos será os outros componentes que irão utiliza-lo. Deixando ele o mais reútilizavel possível.

## ProductList

### Estrutura e Estilo

A lista de produtos básicamente é um container utilizando vários `ProductItems`, sua estrutura ficaria assim:

```html
<div class="product-list">
  <div class="product-list__item product-title">Title</div>
  <div class="product-list__wrap">
    <div class="product-list__item product-item">
      ...
    </div>
    <div class="product-list__item product-item">
      ...
    </div>
    <div class="product-list__item product-item">
      ...
    </div>
  </div>
</div>
```

```scss
.product-list {
  .product-list__title {}
  .product-list__wrap {}
  .product-list__item {
    margin: 0 5px;
  }
}
```

Por mais que seja uma estrutura bem simples, esse será o componente que será responsável por organizar os `ProductItem`, fornecendo estilo de dimensão como margens. Outras opções seria: usar `position`, `flexbox`, `grid`.


## StoreView

Aqui seria a última camada, juntando o `ProductList` e qualquer outro componente para formar a página ou a view. Nesse caso, poderia ter um componente com informações do site como um `Dashboard`.

```html
<div class="store-view">
  <div class="store-view__dashboard">
    ...
  </div>
  <div class="store-view__product-list">
    ...
  </div>
</div>
```

```scss
.store-view {
  .store-view__dashboard {}
  .store-view__product-list {}
}
```