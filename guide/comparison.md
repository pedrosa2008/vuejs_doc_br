---
title: Comparação com Outros Frameworks
type: guide
order: 801
---

Esta é definitivamente a página mais difícil de ser escrita do guia, mas sentimos que é importante. É muito provável que você já teve problemas, tentou resolvê-los e já usou outra biblioteca para resolvê-los. Você está aqui pois quer saber se Vue pode resolver seus problemas específicos de uma forma melhor. É isto que esperamos lhe responder.

Nós também tentamos arduamente evitar arbitrariedade. Como a equipe responsável, obviamente gostamos muito do Vue. Há problemas que acreditamos que ele resolva melhor do que qualquer outra opção existente. Se não acreditássemos nisso, não estaríamos trabalhando nele. Entretanto, queremos ser justos e precisos. Nos esforçamos para listar também onde outras bibliotecas oferecem vantagens significativas, como o vasto ecossistema React de renderizadores alternativos ou o suporte retroativo do Knockout até IE6.

Nós também gostaríamos de **sua** ajuda para manter este documento atualizado, afinal o mundo JavaScript se move rápido! Se você perceber alguma inconsistência ou algo que não parece estar certo, por favor, nos mantenha a par [informando um problema](https://github.com/vuejs/vuejs.org/issues/new?title=Inaccuracy+in+comparisons+guide).

## React

React e Vue compartilham muitas similaridades. Ambos:

- utilizam a abordagem de DOM virtual
- provêm componentes visuais reativos e combináveis
- mantêm o foco na biblioteca principal, com preocupações como roteamento e gerenciamento de estado global tratadas por bibliotecas companheiras

Sendo tão similares em escopo, desprendemos mais tempo para refinar esta comparação em relação às outras. Queremos garantir não somente precisão técnica, mas também equilíbrio. Apontamos onde React supera Vue, por exemplo, na riqueza de seu ecossistema e na abundância de renderizadores personalizados.

Com isso dito, é inevitável que a comparação pareça tendenciosa ao Vue para alguns usuários do React, pois muitos dos assuntos explorados são de certo modo subjetivos. Reconhecemos a existência de variadas preferências técnicas, e esta comparação tem como objetivo principal delinear as razões pelas quais Vue poderia ser uma escolha melhor se suas preferências coincidirem com as nossas.

Alguma das seções abaixo podem estar levemente desatualizadas por causa de recentes atualizações no React 16+, então estamos planejando trabalhar com a comunidade React para renovar esta seção em um futuro próximo.

### Desempenho em Execução

Tanto React quanto Vue são excepcionalmente e similarmente rápidos, então desempenho não deve ser um fator decisivo para uma escolha entre eles. Para métricas específicas, pode dar uma olhada neste [comparativo independente](http://www.stefankrause.net/js-frameworks-benchmark7/table.html), que foca no desempenho bruto de renderização/atualização com árvores de componentes bem simples.

#### Esforços de Otimização

No React, quando o estado de um componente muda, ele aciona também a renderização de toda a árvore de componentes filhos. Para evitar renderizações desnecessárias de componentes filhos, você precisa utilizar um `PureComponent` ou implementar `shouldComponentUpdate` sempre que puder. Você também pode precisar utilizar estruturas de dados imutáveis para tornar suas mudanças de estado mais amigáveis a otimizações. Entretanto, em certos casos pode não ser possível se apoiar nestas otimizações, pois `PureComponent` e `shouldComponentUpdate` assumem que o resultado da renderização de toda a árvore interna sempre é determinado pelas propriedades do componente corrente. Se este não for seu caso, tais otimizações podem levar a estado inconsistente no DOM.

No Vue, as dependências de um componente são automaticamente observadas durante sua renderização, desta forma o sistema sabe precisamente quais componentes precisam ser renderizados quando o estado muda. Pode-se considerar que cada componente já tem `shouldComponentUpdate` automaticamente implementado para você, e sem os problemas com componentes filhos.

De forma geral, isto remove a necessidade de conhecimento de toda uma gama de otimizações de desempenho das responsabilidades do desenvolvedor, permitindo-o focar mais em construir a aplicação em si enquanto ela cresce.

### HTML & CSS

No React, tudo é exclusivamente JavaScript. Não apenas as estruturas HTML são expressas através de JSX, mas tendências recentes também apontam para colocar o gerenciamento CSS dentro do JavaScript. Esta abordagem tem seus próprios benefícios, mas também vem com várias contra-partidas que podem não aparecer de cara para todos os desenvolvedores.

Vue abraça as tecnologias Web clássicas e constrói em cima delas. Para mostrar o que isso significa, mergulharemos em alguns exemplos.

#### JSX vs. Templates

No React, todos os componentes expressam sua interface com funções `render` usando JSX, uma sintaxe declarativa estilo XML, embutida dentro do JavaScript.

A utilização de _render functions_ com JSX oferece algumas vantagens:

- Você pode usar o poder total de uma linguagem de programação (JavaScript) para construir sua camada visual. Isto inclui variáveis temporárias, fluxos de controle e referenciamento direto a valores JavaScript no escopo.

- Ferramentas de suporte (como _linting_, checagem de tipos, _autocomplete_ de código) para JSX estão um pouco mais avançadas do que atualmente temos para _templates_ Vue.

No Vue, também temos [funções de renderização](render-function.html) e até mesmo [suporte a JSX](render-function.html#JSX), afinal às vezes você precisa deste poder. Entretanto, oferecemos para a experiência padrão o uso de _templates_ como uma alternativa mais simples. Qualquer HTML válido é um _template_ Vue válido, o que leva a algumas vantagens próprias:

- Para muitos desenvolvedores habituados a trabalhar com HTML, _templates_ parecem mais naturais para ler e escrever. A preferência pode ser algo subjetivo, mas se isto torna o desenvolvedor mais produtivo, então o benefício é objetivo.

- Utilizar _templates_ baseados em HTML torna mais fácil migrar progressivamente aplicações existentes para tirar vantagens dos recursos de reatividade do Vue.

- Também pode ser muito mais simples para _designers_ e desenvolvedores menos experientes compreenderem o código e começarem a participar.

- Pode-se inclusive utilizar pré-processadores como Pug (anteriormente denominado Jade) para a autoria de seus _templates_ Vue.

Alguns argumentam que você precisaria aprender uma nova sintaxe específica de um único domínio para ser capaz de escrever _templates_ - nós acreditamos que este argumento é superficial, na melhor das hipóteses. Primeiramente, usar JSX não significa que o desenvolvedor não precisa aprender algo - sendo uma sintaxe adicional por cima do JavaScript puro, pode ser fácil de aprender para aqueles familiarizados com JavaScript, mas ainda assim há uma curva de aprendizado. Similarmente, usar _templates_ também é apenas uma sintaxe adicional por cima do HTML puro, inclusive com uma curva de aprendizado bem menor para aqueles familiarizados com HTML. E com este novo conhecimento "específico", ajudamos os desenvolvedores a fazer mais com menos código (por exemplo, modificadores `v-on`). A mesma tarefa pode envolver muito mais código quando usamos JSX ou funções de renderização.

Em um nível mais alto, podemos dividir componentes em duas categorias: de apresentação ou de lógica. Recomendamos utilizar _templates_ para componentes de apresentação e funções de renderização ou JSX para componentes de lógica. O percentual destes componentes depende do tipo de aplicação que você está construindo, mas normalmente observamos que componentes de apresentação são muito mais comuns.

#### CSS com Escopo por Componente

A menos que você distribua componentes entre vários arquivos (por exemplo, com [CSS Modules](https://github.com/gajus/react-css-modules)), usar CSS com escopo no React frequentemente se baseia em soluções CSS-no-JS (por exemplo, com [styled-components](https://github.com/styled-components/styled-components), [glamorous](https://github.com/paypal/glamorous) e [emotion](https://github.com/emotion-js/emotion)). Isto introduz um novo paradigma de estilização orientada a componente, diferente do processo de autoria de CSS normal. Adicionalmente, ainda que haja suporte para extrair CSS em um arquivo único durante o _build_, ainda é comum que um _runtime_ precise ser incluído no pacote para que a estilização ocorra corretamente. Embora você ganhe acesso ao dinamismo do JavaScript enquanto constrói seus estilos, por outro lado você obtém um tamanho de pacote e um custo de execução maiores.

Se você é um fã de CSS-no-JS, muitas bibliotecas populares deste tipo suportam Vue (por exemplo, com [styled-components-vue](https://github.com/styled-components/vue-styled-components) e [vue-emotion](https://github.com/egoist/vue-emotion)). A principal diferença entre React e Vue neste âmbito é que o método padrão de estilização no Vue é através das familiares _tags_ `style` em [Componentes Single-File](single-file-components.html).

[Componentes Single-File](single-file-components.html) lhe oferecem acesso completo ao CSS no mesmo arquivo que o restante do código de seu componente.

```html
<style scoped>
  @media (min-width: 250px) {
    .list-container:hover {
      background: orange;
    }
  }
</style>
```

O atributo opcional `scoped` automaticamente cria escopo ao CSS de seu componente adicionando um atributo único (por exemplo, `data-v-21e5b78`) aos elementos e compilando `.list-container:hover` para algo como `.list-container[data-v-21e5b78]:hover`.

Por fim, a estilização em Componentes Single-File no Vue é muito flexível. Através do [vue-loader](https://github.com/vuejs/vue-loader), você pode utilizar qualquer pré-processador, pós-processador ou mesmo integração profunda com [CSS Modules](https://vue-loader.vuejs.org/en/features/css-modules.html) - tudo sem abrir mão de seu elemento `<style>`.

### Escalabilidade

#### Ampliando a Escala

Para aplicações grandes, tanto Vue quanto React oferecem soluções de roteamento robustas. A comunidade React também tem sido muito inovadora em termos de soluções de gerenciamento de estado (como Flux/Redux). Esses padrões de gerenciamento de estado e até [o próprio Redux](https://yarnpkg.com/en/packages?q=redux%20vue&p=1) podem ser integrados facilmente em aplicações Vue. Na verdade, neste modelo, Vue até deu um passo adiante com o [Vuex](https://github.com/vuejs/vuex), uma solução de gerenciamento de estado inspirada em Elm, que se integra profundamente com o Vue, a qual nós acreditamos que oferece uma experiência de desenvolvimento superior.

Outra importante diferença entre os dois ecossistemas é que as bibliotecas companheiras do Vue para gerenciamento de estado e roteamento (dentre [outras preocupações](https://github.com/vuejs)) são oficialmente suportadas e mantidas sempre atualizadas em relação à biblioteca principal. Ao contrário disso, React escolhe deixar tais preocupações para a comunidade, criando um ecossistema mais fragmentado. Por outro lado, por ser mais popular, o ecossistema React é consideravelmente mais rico do que o do Vue.

Por fim, Vue oferece um [CLI gerador de projetos](https://github.com/vuejs/vue-cli) que torna trivialmente simples iniciar um novo projeto usando o sistema de _build_ de sua escolha, incluindo [webpack](https://github.com/vuejs-templates/webpack), [Browserify](https://github.com/vuejs-templates/browserify), ou mesmo [nenhum](https://github.com/vuejs-templates/simple). React também está fazendo avanços nesta área com o [create-react-app](https://github.com/facebookincubator/create-react-app), mas atualmente tem algumas limitações:

- Não permite nenhuma configuração durante a geração do projeto, enquanto os _templates_ de projeto Vue permitem personalização no estilo [Yeoman](http://yeoman.io/).
- Oferece um modelo simples assumindo que você está criando uma aplicação _single-page_, enquanto Vue oferece muita variedade, para vários propósitos e sistemas de _build_.
- Não permite criar projetos a partir de modelos feitos por outros desenvolvedores, o que pode ser muito útil especialmente em ambientes empresariais com padrões estabelecidos.

É importante notar, no entanto, que muitas dessas limitações são decisões de projeto intencionais tomadas pela equipe do create-react-app, as quais têm suas vantagens. Por exemplo, enquanto as necessidades de seu projeto são bem simples e você não precisa "ejetar" para customizar o processo de _build_, é possível mantê-lo atualizado como uma dependência. Você pode ler mais sobre esta [filosofia diferente aqui](https://github.com/facebookincubator/create-react-app#philosophy).

#### Reduzindo a Escala

React é conhecido por sua curva de aprendizado. Antes que você possa realmente começar, você precisa saber sobre JSX e provavelmente ES2015+, uma vez que muitos exemplos React usam sintaxe de classes. Você também precisa aprender sobre processos de transpilação, pois embora você possa tecnicamente usar Babel para compilar código em tempo real diretamente no navegador, não é algo recomendado para a produção.

Enquanto Vue escala ascendentemente tão bem quanto o React, ele também é capaz de reduzir a escala tão bem quanto jQuery. É isso mesmo - tudo que você precisa fazer é colocar uma única _tag_ `<script>` na página:

``` html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

Então você pode começar a escrever código Vue e ainda distribuir a versão minificada em produção sem se sentir culpado ou ter que se preocupar sobre problemas de performance.

E já que você não precisa obrigatoriamente saber sobre JSX, ES2015 ou sistemas de _build_ para começar com o Vue, leva tipicamente menos de um dia para os desenvolvedores lerem os pontos mais importantes [no guia](/guide) e aprenderem o suficiente para criar aplicações não triviais.

### Renderização Nativa

React Native lhe permite escrever aplicativos nativamente renderizados para iOS e Android usando o mesmo modelo de componentes do React. É ótimo que, como desenvolvedor, você possa aplicar o conhecimento de um _framework_ entre múltiplas plataformas. Nesta frente, Vue possui uma colaboração oficial com [Weex](https://weex.apache.org/), um _framework_ de UI multiplataforma criado por Alibaba Group e sendo incubado pela Apache Software Foundation (ASF). Weex lhe permite utilizar a mesma sintaxe de componentes Vue para criar componentes que podem ser renderizados não apenas no navegador, mas também nativamente em iOS e Android!

No presente momento, Weex ainda é um projeto em desenvolvimento ativo e não está tão maduro e testado na prática quanto o ReactNative, mas o desenvolvimento é dirigido às necessidades de produção do maior negócio de comércio eletrônico do mundo, e a equipe Vue colaborará ativamente com a equipe Weex para garantir a experiência mais suave possível aos desenvolvedores Vue.

Outra opção que desenvolvedores Vue terão em breve é utilizar o [NativeScript](https://www.nativescript.org/), através de um [plugin](https://github.com/rigor789/nativescript-vue) desenvolvido pela comunidade.

### Utilizando MobX

MobX se tornou bem popular na comunidade React e, de fato, utiliza um sistema de reatividade quase idêntico ao Vue. De forma simplista, o fluxo de trabalho React + MobX pode ser pensado como um Vue com código mais prolixo. Por isso, se você estiver usando essa combinação e estiver gostando, saltar para o Vue é, provavelmente, o próximo passo lógico.

### Preact e Outras Bibliotecas no Estilo React

Bibliotecas no estilo do React geralmente tentam compartilhar o máximo de suas APIs e ecossistemas com o React até onde for viável. Por essa razão, a grande maioria das comparações acima também se aplicarão a elas. A principal diferença será tipicamente um ecossistema reduzido, muitas vezes significativamente, comparado ao React. Como essas bibliotecas não podem ser 100% compatíveis com tudo no ecossistema React, algumas bibliotecas de ferramentas e complementares podem não ser utilizáveis. Ou, mesmo que pareçam funcionar, elas podem quebrar a qualquer momento, a menos que sua biblioteca específica no estilo do React seja oficialmente compatível com o React.

## AngularJS (Angular 1)

Um pouco da sintaxe Vue é muito similar ao Angular (por exemplo, `v-if` vs. `ng-if`). Isto ocorre pois acreditamos que há muitas coisas que o AngularJS resolveu corretamente, as quais serviram de inspiração para o Vue bem no início de seu desenvolvimento. Entretanto, também há muitas dores que vêm com o AngularJS, onde o Vue tentou oferecer melhorias significativas.

### Complexidade

Vue é muito mais simples que o AngularJS, tanto em termos de API quanto de _design_. Aprender o suficiente para construir aplicações não triviais tipicamente leva menos de um dia, o que não é verdade para o AngularJS.

### Flexibilidade e Modularidade

AngularJS tem fortes opiniões sobre como as aplicações devem ser estruturadas, enquanto Vue é uma solução mais flexível e modular. Ainda que isto torne Vue mais adaptável a uma ampla variedade de projetos, reconhecemos que às vezes é útil ter algumas decisões já tomadas para você, para que possa simplesmente sair codificando.

É por isso que oferecemos o [modelo de projeto com webpack](https://github.com/vuejs-templates/webpack) que você pode configurar dentro de poucos minutos, ainda assim concedendo-lhe acesso a recursos avançados como _hot module reloading_, _linting_, extração de CSS, e muito mais.

### Interligação com Dados

AngularJS adota a abordagem de _two-way binding_ entre escopos, enquanto Vue força um fluxo de dados de mão única entre componentes. Isto permite que o fluxo de dados seja mais compreensível em aplicações não triviais.

### Diretivas vs. Componentes

Vue possui uma clara separação entre diretivas e componentes. Diretivas existem somente para encapsular lógica de manipulação do DOM, enquanto componentes são unidades auto-contidas que possuem sua própria lógica de dados e de apresentação. No AngularJS, diretivas fazem de tudo e componentes são apenas um tipo específico de diretiva.

### Desempenho em Execução

Vue tem melhor desempenho e é muito, muito mais fácil de otimizar, por não utilizar verificação suja de alterações de dados no escopo (_dirty checking_). AngularJS torna-se lento quando há um grande número de observadores (_watchers_), pois a cada vez que qualquer coisa muda no escopo, todos esses observadores precisam ser reavaliados. Além disso, este laço de verificação suja (chamado _digest cycle_), pode ter que executar várias vezes para "estabilizar" se algum observador aciona outra atualização. Usuários do AngularJS por vezes têm de recorrer a técnicas esotéricas para contornar este ciclo e, em algumas situações, não há simplesmente nenhuma maneira de otimizar um escopo com muitos observadores.

O Vue definitivamente não sofre com isso, pois usa um sistema de observação por rastreamento de dependências transparente, com enfileiramento assíncrono - todas as alterações disparam de forma independente, a menos que tenham dependências explícitas.

Curiosamente, há algumas semelhanças na forma como Angular 2 e Vue estão lidando com estes problemas existentes no AngularJS.

## Angular (previamente denominado Angular 2)

Temos uma seção separada para Angular por ser, de fato, um _framework_ completamente diferente do AngularJS. Por exemplo, possui um sistema de componentes de primeira classe, muitos detalhes de implementação foram totalmente reescritos, e a API também mudou drasticamente.

### TypeScript

Angular essencialmente requer a utilização de TypeScript, dado que quase toda sua documentação e recursos de aprendizado são baseados nesta linguagem. TypeScript tem seus benefícios - checagem estática de tipos pode ser muito útil para aplicações de larga escala, e pode ser um grande bônus de produtividade para desenvolvedores acostumados com Java e C#.

Entretanto, nem todo mundo quer TypeScript. Em muitos casos de menor escala, introduzir um sistema de tipagem pode resultar em mais sobrecarga do que ganho de produtividade. Nestes casos você estaria melhor seguindo com Vue, afinal utilizar Angular sem TypeScript pode ser desafiador.

Por fim, ainda que não tão profundamente integrado com TypeScript quanto Angular, Vue também oferece [tipagem oficial](https://github.com/vuejs/vue/tree/dev/types) e [decorador oficial](https://github.com/vuejs/vue-class-component) para aqueles que desejarem utilizar TypeScript com Vue. Também estamos ativamente colaborando com os times do TypeScript e do VSCode da Microsoft para melhorar a experiência TS/IDE para os usuários Vue + TS.

### Desempenho em Execução

Ambos os _frameworks_ são excepcionalmente rápidos, com métricas muito similares nos _benchmarks_. Você pode [navegar por métricas específicas](http://www.stefankrause.net/js-frameworks-benchmark7/table.html) para uma comparação mais granular, mas desempenho provavelmente não é um fator decisivo entre ambos.

### Tamanho

Versões recentes do Angular, com [compilação AOT](https://en.wikipedia.org/wiki/Ahead-of-time_compilation) (_Ahead-Of-Time_) e [tree-shaking](https://en.wikipedia.org/wiki/Tree_shaking), tem sido capazes de derrubar o tamanho do pacote consideravelmente. Todavia, um projeto Vue 2 cheio de recursos com Vuex + Vue Router incluídos (~30KB depois de gzip) ainda é significativamente mais leve do que uma aplicação padrão, compilada AOT, gerada pelo `angular-cli` (~65KB após gzip).

### Flexibilidade

Vue é muito menos opinativo que Angular, oferecendo suporte oficial a vários sistemas de _build_, sem restrições sobre como você estrutura sua aplicação. Muitos desenvolvedores gostam desta liberdade, enquanto outros preferem ter somente "O Jeito Certo" de construir qualquer aplicação.

### Curva de Aprendizado

Para começar com Vue, tudo que você precisa é familiaridade com HTML e JavaScript versão ES5 (ou seja, JavaScript puro). Com estas habilidades básicas, você pode começar a construir aplicações não triviais com menos de um dia de leitura [do guia](/guide).

A curva de aprendizado do Angular é muito mais acentuada. A superfície da API do _framework_ é enorme e o usuário precisará se familiarizar com muito mais conceitos antes de se tornar produtivo. A complexidade do Angular deve-se em grande parte ao seu objetivo de projetar somente aplicações grandes e complexas - mas isso torna o _framework_ muito mais difícil para desenvolvedores menos experientes começarem.

## Ember

Ember é um _framework_ completo projetado para ser altamente opinativo. Ele fornece uma série de convenções estabelecidas e, uma vez que você fique bastante familiarizado com elas, pode se tornar muito produtivo. No entanto, também significa que a curva de aprendizado é elevada e a flexibilidade é sofrível. É uma escolha para você colocar na balança entre adotar um _framework_ fortemente opinativo ou uma biblioteca com um conjunto de ferramentas de baixo acoplamento que funcionam em conjunto. Este último cenário lhe dá mais liberdade, mas também requer que você tome mais decisões arquitetônicas.

Com isto dito, seria provavelmente mais adequado uma comparação entre o núcleo do Vue e as camadas de [templates](https://guides.emberjs.com/v2.7.0/templates/handlebars-basics/) e de [modelo de objetos](https://guides.emberjs.com/v2.7.0/object-model/) do Ember:

- Vue oferece reatividade não obstrusiva em objetos JavaScript tradicionais e propriedades computadas totalmente automáticas. No Ember, você precisa envolver qualquer coisa em Objetos Ember e manualmente declarar dependências para propriedades computadas.

- A sintaxe de _templates_ do Vue se arma com o poder total de expressões JavaScript, enquanto a sintaxe de expressões e _helpers_ do Handlebars é bastante limitada.

- Em termos de performance, Vue supera Ember [por uma margem justa](http://www.stefankrause.net/js-frameworks-benchmark7/table.html), mesmo após a atualização mais recente do motor Glimmer no Ember 2.x. Vue realiza atualizações em lote automaticamente, enquanto no Ember você precisa lidar manualmente com laços de execução em situações de desempenho crítico.

## Knockout

Knockout foi um pioneiro nas áreas de MVVM e rastreamento de dependências e seu sistema de reatividade é muito semelhante ao Vue. O [suporte a navegadores](http://knockoutjs.com/documentation/browser-support.html) é muito impressionante considerando tudo o que faz, suportante retroativamente até o IE6! Vue, por outro lado, suporta apenas IE9 e superiores.

Com o tempo, porém, o desenvolvimento do Knockout começou a diminuir e está começando a dar sinais de sua idade. Por exemplo, seu sistema de componentes carece de um conjunto completo de eventos de ciclo de vida e, embora seja um caso muito comum, a interface para passagem de elementos filhos a um componente pai se mostra um pouco desajeitada em comparação com o [Vue](components.html#Content-Distribution-with-Slots).

Nos parece que existem também diferenças filosóficas na concepção da API que, se você estiver curioso, podem ser demonstradas pela forma como cada um lida com a criação de uma [lista de tarefas simples](https://gist.github.com/chrisvfritz/9e5f2d6826af00fcbace7be8f6dccb89). É definitivamente um tanto subjetivo, mas muitos consideram a API do Vue menos complexo e melhor estruturada.

## Polymer

Polymer é outro projeto patrocinado por Google e, de fato, também foi uma fonte de inspiração para o Vue. Componentes Vue podem ser vagamente comparados com elementos customizados Polymer, e ambos fornecem um estilo de desenvolvimento semelhante. A maior diferença é que o Polymer é construído sobre os mais recentes recursos de Web Components e, portanto, requer _polyfills_ não triviais para funcionar (com um desempenho degradado) em navegadores que não suportam esses recursos de forma nativa. Em contraste, Vue funciona sem qualquer dependência externa ou _polyfills_ até o IE9.

No Polymer, a equipe fez também um sistema de _data-binding_ muito limitado, a fim de compensar o desempenho. Por exemplo, as únicas expressões suportados em _templates_ Polymer são de negação booleana e chamadas simples de métodos. Sua implementação de propriedades computadas também não é muito flexível.

## Riot

Riot 3.0 fornece um modelo de desenvolvimento baseado em componentes similar (os quais são chamados de _tag_ no Riot), com uma API mínima e belamente concebida. Riot e Vue provavelmente compartilham muitas filosofias de _design_. No entanto, apesar de ser um pouco mais pesado que Riot, Vue oferece algumas vantagens significativas:

- Melhor desempenho. Riot [atravessa a árvore DOM](http://riotjs.com/compare/#virtual-dom-vs-expressions-binding) ao invés de utilizar DOM virtual, portanto sofre dos mesmos problemas de desempenho do AngularJS.
- Mais maturidade no suporte a ferramentas de _build_. Vue oferece suporte oficial a [webpack](https://github.com/vuejs/vue-loader) e [Browserify](https://github.com/vuejs/vueify), enquanto Riot deixa para a comunidade a integração com sistemas de _build_.
