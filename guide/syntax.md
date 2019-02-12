---
title: Sintaxe de Templates
type: guide
order: 4
---

O Vue.js utiliza uma sintaxe de _templates_ baseada em HTML, permitindo que você vincule declarativamente o DOM renderizado aos dados da instância Vue. Todos os _templates_ do Vue.js são compostos por HTML válido que pode ser compilado por navegadores compatíveis com as especificações e também por compiladores HTML.

Internamente, Vue compila os _templates_ dentro de funções de renderização de Virtual DOM. Combinado com o sistema de reatividade, Vue é capaz de identificar de forma inteligente a menor quantidade possível de componentes a serem "re-renderizados" e aplica o mínimo possível de manipulações DOM quando o estado da aplicação muda.

Se você é familiarizado com os conceitos de Virtual DOM e prefere o poder do JavaScript puro, também é possível [escrever diretamente funções de renderização](/guide/render-function.html) em vez de utilizar _templates_, inclusive podendo contar com o suporte opcional para JSX nestas funções.

## Interpolações

### Texto

O mais básico _data binding_, interpolando texto com a sintaxe _Mustache_ (chaves duplas):

``` html
<span>Mensagem: {{ msg }}</span>
```

A _tag mustache_ vai ser trocada pelo valor da propriedade `msg` do objeto de dados correspondente. Esse texto também reagirá sempre que a propriedade `msg` for modificada.

Você também pode realizar interpolações únicas que não são atualizada quando os dados mudam através da [diretiva v-once](/api/#v-once), mas lembre-se que isso afetará qualquer _binding_ realizado no mesmo nó:

``` html
<span v-once>Esse valor nunca será modificado: {{ msg }}</span>
```

### HTML

As chaves duplas interpretam os dados como texto simples, e não HTML. Para que você exiba HTML, utilize a diretiva `v-html`:

``` html
<p>Interpolação textual: {{ rawHtml }}</p>
<p>Diretiva v-html: <span v-html="rawHtml"></span></p>
```

{% raw %}
<div id="example1" class="demo">
  <p>Interpolação textual: {{ rawHtml }}</p>
  <p>Diretiva v-html: <span v-html="rawHtml"></span></p>
</div>
<script>
new Vue({
  el: '#example1',
  data: function () {
  	return {
  	  rawHtml: '<span style="color: red">Sou vermelho</span>'
  	}
  }
})
</script>
{% endraw %}

Os conteúdos do `span` serão substituídos com o valor da propriedade `rawHtml`, interpretada como HTML puro - _data bindings_ são ignorados. Note que você não pode utilizar a diretiva `v-html` para compor _templates_ parciais, porque o Vue não é uma _engine_ baseada em _templates_ através de String. Em vez disso, componentes são a maneira indicada como peça fundamental de composição e reutilização de elementos de interface.

<p class="tip">Dinamicamente renderizar HTML sem precauções pode ser muito perigoso, pois pode levar a [ataques XSS](https://pt.wikipedia.org/wiki/Cross-site_scripting). Utilize a interpolação de HTML apenas em conteúdos que você confia e **nunca** em conteúdos enviados por seus usuários.</p>

### Atributos

Chaves duplas não podem ser usadas em atributos HTML. Para isso, utilize a [diretiva v-bind](../api/#v-bind):

``` html
<div v-bind:id="dynamicId"></div>
```

No caso de atributos booleanos, onde sua mera existência implica em `true`, `v-bind` funciona um pouco diferente. Neste exemplo:

``` html
<button v-bind:disabled="isButtonDisabled">Botão</button>
```

Se `isButtonDisabled` possui um valor `null`, `undefined` ou `false`, o atributo `disabled` nem mesmo será incluído no elemento `<button>` renderizado.

### Expressões JavaScript

Até aqui nós apenas estivemos vinculando propriedades simples diretamente em nossos _templates_. Mas o Vue.js suporta todo o poder das expressões JavaScript dentro de qualquer tipo de _data binding_:

``` html
{{ number + 1 }}

{{ ok ? 'SIM' : 'NÃO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```

Essas expressões serão compiladas como JavaScript no escopo de dados da instância do Vue. A única restrição é que cada _binding_ pode conter **apenas uma expressão**, então os códigos a seguir são exemplos de coisas que **NÃO** funcionarão:

``` html
<!-- isso é uma atribuição, e não uma expressão -->
{{ var a = 1 }}

<!-- controle de fluxo também não funciona, utilize expressões ternárias -->
{{ if (ok) { return message } }}
```

<p class="tip">Expressões em _templates_ são restritas a um ambiente controlado e somente possuem acesso a uma lista de variáveis globais permitidas, como `Math` e `Date`. Você não deve tentar acessar variáveis globais definidas pelo usuário em uma expressão de _template_.</p>

## Diretivas

Diretivas são atributos especiais com o prefixo `v-`. Espera-se que os valores atribuídos às diretivas sejam **uma simples expressão Javascript** (com a excessão do `v-for`, que será discutido posteriormente). O trabalho de uma diretiva é aplicar reativamente efeitos colaterais ao DOM, ou seja, realizar algum efeito quando o valor da expressão é modificado. Vamos revisar o exemplo que vimos na introdução:

``` html
<p v-if="seen">Agora você me viu</p>
```

Aqui, a diretiva `v-if` irá remover/inserir o elemento `<p>` baseado na veracidade do valor da expressão `seen`.

### Parâmetros

Algumas diretivas podem aceitar um "parâmetro", denotado pelo símbolo de dois pontos após a diretiva. Por exemplo, a diretiva `v-bind` é utilizada para atualizar um atributo HTML reativamente:

``` html
<a v-bind:href="url"> ... </a>
```

Aqui `href` é o parâmetro, que informará à diretiva `v-bind` para interligar o atributo `href` do elemento ao valor da expressão `url` de forma reativa.

Outro simples exemplo é a diretiva `v-on`, que observa eventos do DOM:

``` html
<a v-on:click="doSomething"> ... </a>
```

Aqui o valor é o nome do evento DOM que ela está escutando. Falaremos sobre gerenciamento de eventos com mais detalhes em breve.

### Modificadores

Modificadores são sufixos especiais denotados por um ponto, que indicam que aquela diretiva deve ser vinculada de alguma maneira especial. Por exemplo, o modificador `.prevent` indica que o `v-on` chamará a função `event.preventDefault()` quando o evento for disparado:

``` html
<form v-on:submit.prevent="onSubmit"> ... </form>
```

Você verá outros exemplos de modificadores futuramente, [modificadores para `v-on`](events.html#Modificadores-de-Eventos) e [modificadores para `v-model`](forms.html#Modificadores), quando estivermos explorando tais funcionalidades.

## Abreviações

O prefixo `v-` serve como dica visual para identificar atributos específicos do Vue nos _templates_. Isso é útil quando se está utilizando o Vue para aplicar comportamento dinâmico em HTML existente, mas você pode achar um pouco verboso se precisar usar frequentemente. Ao mesmo tempo, o uso do prefixo `v-` se torna menos importante quando se está construindo uma [SPA](https://en.wikipedia.org/wiki/Single-page_application), onde o Vue gerencia cada _template_. Assim, Vue oferece abreviações especiais para as duas diretivas mais utilizadas, `v-bind` e o `v-on`:

### Abreviação para `v-bind`

``` html
<!-- sintaxe completa -->
<a v-bind:href="url"> ... </a>

<!-- abreviação -->
<a :href="url"> ... </a>
```

### Abreviação para `v-on`

``` html
<!-- sintaxe completa -->
<a v-on:click="doSomething"> ... </a>

<!-- abreviação -->
<a @click="doSomething"> ... </a>
```

Essas abreviações podem parecer um pouco diferentes do HTML normalmente utilizado, mas os caracteres `:` e `@` são válidos para nomes de atributos em todos os navegadores que o Vue.js suporta. Além disso, não aparecerão no código renderizado. Essa sintaxe é totalmente opcional, mas você provavelmente vai apreciar quando utilizar diretivas frequentemente.
