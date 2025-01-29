---
footer: false
---

<script setup>
import { VTCodeGroup, VTCodeGroupTab } from '@vue/theme'
</script>

# Inicio Rápido {#quick-start}

## Prueba Vue Online {#try-vue-online}

- Para probar rápidamente Vue, puedes hacerlo directamente en nuestro [Playground](https://play.vuejs.org/#eNo9jcEKwjAMhl/lt5fpQYfXUQfefAMvvRQbddC1pUuHUPrudg4HIcmXjyRZXEM4zYlEJ+T0iEPgXjn6BB8Zhp46WUZWDjCa9f6w9kAkTtH9CRinV4fmRtZ63H20Ztesqiylphqy3R5UYBqD1UyVAPk+9zkvV1CKbCv9poMLiTEfR2/IXpSoXomqZLtti/IFwVtA9A==).

- Si prefieres una configuración HTML sencilla sin pasos de compilación, puedes utilizar este [JSFiddle](https://jsfiddle.net/yyx990803/2ke1ab0z/) como punto de partida.

- Si ya estás familiarizado con Node.js y el concepto de herramientas de compilación, también puedes probar una configuración de compilación completa directamente desde tu navegador en [StackBlitz](https://vite.new/vue).

## Creación de una Aplicación de Vue {#creating-a-vue-application}

:::tip Prerequisitos

- Familiaridad con la línea de comandos
- Instalar [Node.js](https://nodejs.org/) versión 18.0 o superior
  :::

En esta sección presentaremos cómo crear una [Aplicación de una Sola Página (SPA)](/guide/extras/ways-of-using-vue#single-page-application-spa) de Vue en tu máquina local. El proyecto creado utilizará una configuración de compilación basada en [Vite](https://vitejs.dev) y nos permitirá utilizar los [Componentes de un Solo Archivo (SFCs)](/guide/scaling-up/sfc) de Vue.

Asegúrate de tener instalada una versión actualizada de [Node.js](https://nodejs.org/) y que tu directorio de trabajo actual sea donde quieras crear un proyecto. Ejecuta el siguiente comando en tu línea de comandos (sin el signo `$`):

<VTCodeGroup>
  <VTCodeGroupTab label="npm">

  ```sh
  $ npm create vue@latest
  ```

  </VTCodeGroupTab>
  <VTCodeGroupTab label="pnpm">

  ```sh
  $ pnpm create vue@latest
  ```

  </VTCodeGroupTab>
  <VTCodeGroupTab label="yarn">

  ```sh
  $ yarn create vue@latest
  ```

  </VTCodeGroupTab>
  <VTCodeGroupTab label="bun">

  ```sh
  $ bun create vue@latest
  ```

  </VTCodeGroupTab>
</VTCodeGroup>

Este comando instalará y ejecutará [create-vue](https://github.com/vuejs/create-vue), la herramienta oficial de estructuración de proyectos de Vue. Se te presentarán solicitudes para varias características opcionales como TypeScript y soporte de pruebas:

<div class="language-sh"><pre><code><span style="color:var(--vt-c-green);">✔</span> <span style="color:#A6ACCD;">Project name: <span style="color:#888;">… <span style="color:#89DDFF;">&lt;</span><span style="color:#888;">your-project-name</span><span style="color:#89DDFF;">&gt;</span></span></span>
<span style="color:var(--vt-c-green);">✔</span> <span style="color:#A6ACCD;">Add TypeScript? <span style="color:#888;">… <span style="color:#89DDFF;text-decoration:underline">No</span> / Yes</span></span>
<span style="color:var(--vt-c-green);">✔</span> <span style="color:#A6ACCD;">Add JSX Support? <span style="color:#888;">… <span style="color:#89DDFF;text-decoration:underline">No</span> / Yes</span></span>
<span style="color:var(--vt-c-green);">✔</span> <span style="color:#A6ACCD;">Add Vue Router for Single Page Application development? <span style="color:#888;">… <span style="color:#89DDFF;text-decoration:underline">No</span> / Yes</span></span>
<span style="color:var(--vt-c-green);">✔</span> <span style="color:#A6ACCD;">Add Pinia for state management? <span style="color:#888;">… <span style="color:#89DDFF;text-decoration:underline">No</span> / Yes</span></span>
<span style="color:var(--vt-c-green);">✔</span> <span style="color:#A6ACCD;">Add Vitest for Unit testing? <span style="color:#888;">… <span style="color:#89DDFF;text-decoration:underline">No</span> / Yes</span></span>
<span style="color:var(--vt-c-green);">✔</span> <span style="color:#A6ACCD;">Add an End-to-End Testing Solution? <span style="color:#888;">… <span style="color:#89DDFF;text-decoration:underline">No</span> / Cypress / Playwright</span></span>
<span style="color:var(--vt-c-green);">✔</span> <span style="color:#A6ACCD;">Add ESLint for code quality? <span style="color:#888;">… <span style="color:#89DDFF;text-decoration:underline">No</span> / Yes</span></span>
<span style="color:var(--vt-c-green);">✔</span> <span style="color:#A6ACCD;">Add Prettier for code formatting? <span style="color:#888;">… <span style="color:#89DDFF;text-decoration:underline">No</span> / Yes</span></span>
<span></span>
<span style="color:#A6ACCD;">Scaffolding project in ./<span style="color:#89DDFF;">&lt;</span><span style="color:#888;">your-project-name</span><span style="color:#89DDFF;">&gt;</span>...</span>
<span style="color:#A6ACCD;">Done.</span></code></pre></div>

Si no estás seguro de alguna opción, simplemente elige `No` pulsando enter por ahora. Una vez creado el proyecto, sigue las instrucciones para instalar las dependencias e iniciar el servidor de desarrollo:

<VTCodeGroup>
  <VTCodeGroupTab label="npm">

  ```sh
  $ cd <your-project-name>
  $ npm install
  $ npm run dev
  ```

  </VTCodeGroupTab>
  <VTCodeGroupTab label="pnpm">

  ```sh
  $ cd <your-project-name>
  $ pnpm install
  $ pnpm run dev
  ```

  </VTCodeGroupTab>
  <VTCodeGroupTab label="yarn">

  ```sh
  $ cd <your-project-name>
  $ yarn
  $ yarn dev
  ```

  </VTCodeGroupTab>
  <VTCodeGroupTab label="bun">

  ```sh
  $ cd <your-project-name>
  $ bun install
  $ bun run dev
  ```

  </VTCodeGroupTab>
</VTCodeGroup>

Ahora deberías tener tu primer proyecto Vue funcionando. Ten en cuenta que los componentes de ejemplo del proyecto generado están escritos utilizando la [Composition API](/guide/introduction#composition-api) y `<script setup>`, en lugar de la [Options API](/guide/introduction#options-api). He aquí algunos consejos adicionales:

- La configuración de IDE recomendada es [Visual Studio Code](https://code.visualstudio.com/) + la extensión [Vue - Official](https://marketplace.visualstudio.com/items?itemName=Vue.volar). Si utilizas otros editores, consulta la sección [Soporte para IDE](/guide/scaling-up/tooling#ide-support).
- En la [Guía de Herramientas](/guide/scaling-up/tooling) se discuten más detalles sobre las herramientas, incluyendo la integración con frameworks backend.
- Para obtener más información sobre la herramienta de compilación subyacente Vite, consulta la [documentación de Vite](https://vitejs.dev).
- Si decides utilizar TypeScript, consulta la guía [Usando Vue con TypeScript](typescript/overview.html).

Cuando estés listo para enviar tu aplicación a producción, ejecuta lo siguiente:

<VTCodeGroup>
  <VTCodeGroupTab label="npm">

  ```sh
  $ npm run build
  ```

  </VTCodeGroupTab>
  <VTCodeGroupTab label="pnpm">

  ```sh
  $ pnpm run build
  ```

  </VTCodeGroupTab>
  <VTCodeGroupTab label="yarn">

  ```sh
  $ yarn build
  ```

  </VTCodeGroupTab>
  <VTCodeGroupTab label="bun">

  ```sh
  $ bun run build
  ```

  </VTCodeGroupTab>
</VTCodeGroup>

Esto creará una compilación de tu aplicación lista para producción en el directorio `./dist` del proyecto. Consulta la guía de [Implementación de Producción (Deployment)](/guide/best-practices/production-deployment) para aprender más sobre cómo enviar tu aplicación a producción.

[Siguientes Pasos >](#siguientes-pasos)

## Usando Vue desde un CDN {#using-vue-from-cdn}

Puedes usar Vue directamente desde un CDN a través de una etiqueta script:

```html
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
```

Aquí estamos usando [unpkg](https://unpkg.com/), pero también puedes usar cualquier CDN que sirva paquetes npm, por ejemplo [jsdelivr](https://www.jsdelivr.com/package/npm/vue) o [cdnjs](https://cdnjs.com/libraries/vue). Por supuesto, también puedes descargar este archivo y servirlo tú mismo.

Cuando se utiliza Vue desde un CDN, no hay ningún "paso de compilación" involucrado. Esto hace que la configuración sea mucho más simple, y es adecuado para mejorar el HTML estático o la integración con un marco de backend. Sin embargo, no podrás usar la sintaxis de Componentes de un Solo Archivo (SFC).

### Usando la Compilación Global {#using-the-global-build}

El enlace anterior carga la _compilación global_ de Vue, donde todas las APIs de alto nivel están expuestas como propiedades en el objeto `Vue` global. Aquí hay un ejemplo completo usando la compilación global:

<div class="options-api">

```html
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>

<div id="app">{{ message }}</div>

<script>
  const { createApp } = Vue

  createApp({
    data() {
      return {
        message: '¡Hola Vue!'
      }
    }
  }).mount('#app')
</script>
```

[Demo en Codepen](https://codepen.io/vuejs-examples/pen/QWJwJLp)

</div>

<div class="composition-api">

```html
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>

<div id="app">{{ message }}</div>

<script>
  const { createApp, ref } = Vue

  createApp({
    setup() {
      const message = ref('¡Hola Vue!')
      return {
        message
      }
    }
  }).mount('#app')
</script>
```

[Demo en Codepen](https://codepen.io/vuejs-examples/pen/eYQpQEG)

:::tip
Muchos de los ejemplos de la Composition API a lo largo de la guía utilizarán la sintaxis `<script setup>`, que requiere herramientas de compilación. Si deseas utilizar Composition API sin un paso de compilación, consulta el uso de la opción [`setup()`](/api/composition-api-setup).
:::

</div>

### Usando el Módulo de Construcción ES {#using-the-es-module-build}

En el resto de la documentación, utilizaremos principalmente la sintaxis de [módulos ES](https://developer.mozilla.org/es/docs/Web/JavaScript/Guide/Modules). La mayoría de los navegadores modernos soportan ahora módulos ES de forma nativa, por lo que podemos usar Vue desde un CDN a través de módulos ES nativos como este:

<div class="options-api">

```html{3,4}
<div id="app">{{ message }}</div>

<script type="module">
  import { createApp } from 'https://unpkg.com/vue@3/dist/vue.esm-browser.js'

  createApp({
    data() {
      return {
        message: '¡Hola Vue!'
      }
    }
  }).mount('#app')
</script>
```

</div>

<div class="composition-api">

```html{3,4}
<div id="app">{{ message }}</div>

<script type="module">
  import { createApp, ref } from 'https://unpkg.com/vue@3/dist/vue.esm-browser.js'

  createApp({
    setup() {
      const message = ref('¡Hola Vue!')
      return {
        message
      }
    }
  }).mount('#app')
</script>
```

</div>

Observa que estamos usando `<script type="module">`, y que la URL importada del CDN apunta a la **compilación de módulos ES** de Vue.

<div class="options-api">

[Demo en Codepen](https://codepen.io/vuejs-examples/pen/VwVYVZO)

</div>
<div class="composition-api">

[Demo en Codepen](https://codepen.io/vuejs-examples/pen/MWzazEv)

</div>

### Habilitar mapas de importación {#enabling-import-maps}

En el ejemplo anterior, estamos importando desde la URL del CDN, pero en el resto de la documentación verás código como este:

```js
import { createApp } from 'vue'
```

Podemos enseñarle al navegador dónde localizar la importación de `vue` usando [Import Maps](https://caniuse.com/import-maps):

<div class="options-api">

```html{1-7,12}
<script type="importmap">
  {
    "imports": {
      "vue": "https://unpkg.com/vue@3/dist/vue.esm-browser.js"
    }
  }
</script>

<div id="app">{{ message }}</div>

<script type="module">
  import { createApp } from 'vue'

  createApp({
    data() {
      return {
        message: '¡Hola Vue!'
      }
    }
  }).mount('#app')
</script>
```

[Demo en Codepen](https://codepen.io/vuejs-examples/pen/wvQKQyM)

</div>

<div class="composition-api">

```html{1-7,12}
<script type="importmap">
  {
    "imports": {
      "vue": "https://unpkg.com/vue@3/dist/vue.esm-browser.js"
    }
  }
</script>

<div id="app">{{ message }}</div>

<script type="module">
  import { createApp, ref } from 'vue'

  createApp({
    setup() {
      const message = ref('Hello Vue!')
      return {
        message
      }
    }
  }).mount('#app')
</script>
```

[Demo en Codepen](https://codepen.io/vuejs-examples/pen/YzRyRYM)

</div>

También puedes añadir entradas para otras dependencias al mapa de importación, pero asegúrate de que apuntan a la versión de los módulos ES de la biblioteca que pretendes utilizar.

:::tip Soporte para Navegadores de Mapas de Importación
Los mapas de importación es una función relativamente nueva de los navegadores. Asegúrate de utilizar un navegador dentro de su [rango de compatibilidad](https://caniuse.com/import-maps). En particular, sólo es compatible con Safari 16.4+.
:::

:::warning Notas sobre el Uso en Producción
Los ejemplos mostrados hasta ahora usan la versión de desarrollo de Vue. Si quieres usar Vue desde un CDN en producción, asegúrate de consultar la guía de [Implementación en Producción (Deployment)](/guide/best-practices/production-deployment#without-build-tools).

Aunque es posible usar Vue sin un sistema de compilación, una alternativa a considerar es usar [`vuejs/petite-vue`](https://github.com/vuejs/petite-vue) que podría ser más adecuado para el contexto donde [`jquery/jquery`](https://github.com/jquery/jquery) (en el pasado) o [`alpinejs/alpine`](https://github.com/alpinejs/alpine) (en el presente) podrían ser usados en su lugar.
:::

### Distribución de los Módulos {#splitting-up-the-modules}

A medida que profundizamos en la guía, puede que necesitemos dividir nuestro código en archivos JavaScript separados para que sean más fáciles de gestionar. Por ejemplo:

```html
<!-- index.html -->
<div id="app"></div>

<script type="module">
  import { createApp } from 'vue'
  import MyComponent from './my-component.js'

  createApp(MyComponent).mount('#app')
</script>
```

<div class="options-api">

```js
// my-component.js
export default {
  data() {
    return { count: 0 }
  },
  template: `<div>El contador está en {{ count }}</div>`
}
```

</div>
<div class="composition-api">

```js
// my-component.js
import { ref } from 'vue'
export default {
  setup() {
    const count = ref(0)
    return { count }
  },
  template: `<div>count is {{ count }}</div>`
}
```

</div>

Si abres el `index.html` de arriba directamente en tu navegador, verás que arroja un error porque los módulos ES no pueden trabajar sobre el protocolo `file://`, que es el protocolo que utiliza el navegador cuando abre un archivo local.

Por razones de seguridad, los módulos ES sólo pueden funcionar sobre el protocolo `http://` que utilizan los navegadores cuando abren páginas en la web. Para que los módulos ES funcionen en nuestra máquina local, necesitamos servir el `index.html` sobre el protocolo `http://`, con un servidor HTTP local.

Para iniciar un servidor HTTP local, primero instala [Node.js](https://nodejs.org/es/) y luego ejecuta `npx serve` desde la línea de comandos en el mismo directorio donde está tu archivo HTML. También puedes utilizar cualquier otro servidor HTTP que pueda servir archivos estáticos con los tipos MIME correctos.

Puede que hayas notado que la plantilla del componente importado está en línea como una cadena JavaScript. Si estás usando VSCode, puedes instalar la extensión [es6-string-html](https://marketplace.visualstudio.com/items?itemName=Tobermory.es6-string-html) y prefijar las cadenas con un comentario `/*html*/` para obtener resaltado de la sintaxis para ellas.

## Siguientes pasos {#next-steps}

Si te has saltado la [Introducción](/guide/introduction), te recomendamos encarecidamente que la leas antes de pasar al resto de la documentación.

<div class="vt-box-container next-steps">
  <a class="vt-box" href="/guide/essentials/application.html">
    <p class="next-steps-link">Continúa con la Guía</p>
    <p class="next-steps-caption">La guía te lleva a través de cada aspecto del framework con todos sus detalles.</p>
  </a>
  <a class="vt-box" href="/tutorial/">
    <p class="next-steps-link">Prueba el Tutorial</p>
    <p class="next-steps-caption">Para aquellos que prefieren aprender las cosas de forma práctica.</p>
  </a>
  <a class="vt-box" href="/examples/">
    <p class="next-steps-link">Mira los Ejemplos</p>
    <p class="next-steps-caption">Explora ejemplos de las principales características y las tareas comunes de la UI.</p>
  </a>
</div>
