---
title: Roteamento
type: guide
order: 501
---

## Roteador Oficial

Para a maioria das _Single Page Applications_, é recomendado usar a biblioteca oficialmente suportada [vue-router](https://github.com/vuejs/vue-router). Para mais detalhes, veja a [documentação do vue-router](https://router.vuejs.org/).

## Roteamento Simples do Zero

Se você precisar apenas de rotas muito simples e não deseja adicionar uma biblioteca completa de rotas, você pode renderizar dinamicamente componentes a nível de páginas, deste modo:

``` js
const NotFound = { template: '<p>Página não encontrada</p>' }
const Home = { template: '<p>Página Inicial</p>' }
const About = { template: '<p>Sobre</p>' }

const routes = {
  '/': Home,
  '/sobre': About
}

new Vue({
  el: '#app',
  data: {
    currentRoute: window.location.pathname
  },
  computed: {
    ViewComponent () {
      return routes[this.currentRoute] || NotFound
    }
  },
  render (h) { return h(this.ViewComponent) }
})
```

Combinado com a API de histórico do HTML5, você pode criar um roteador do lado do cliente muito básico, porém totalmente funcional. Para ver isso na prática, confira [este exemplo](https://github.com/chrisvfritz/vue-2.0-simple-routing-example).

## Integração com Roteadores de Terceiros

Se há uma biblioteca de terceiros para rotas que você prefira usar, como [Page.js](https://github.com/visionmedia/page.js) ou [Director](https://github.com/flatiron/director), a integração é [igualmente fácil](https://github.com/chrisvfritz/vue-2.0-simple-routing-example/compare/master...pagejs). Aqui está um [exemplo completo](https://github.com/chrisvfritz/vue-2.0-simple-routing-example/tree/pagejs) usando Page.js.
