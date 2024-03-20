# API de Composición: Inyección de Dependencias {#composition-api-dependency-injection}

## provide() {#provide}

Proporciona un valor que puede ser inyectado por componentes descendientes.

- **Tipo**

  ```ts
  function provide<T>(key: InjectionKey<T> | string, value: T): void
  ```

- **Detalles**

  `provide()` toma dos argumentos: la clave, que puede ser una cadena o un symbol, y el valor que se va a inyectar.

  Cuando se usa TypeScript, la clave puede ser un símbolo convertido a `InjectionKey` - un tipo de utilidad proporcionado por Vue que extiende `Symbol`, que se puede utilizar para sincronizar el tipo de valor entre `provide()` e `inject()`.

  Similar a las APIs de registro de hooks de ciclo de vida, `provide()` debe ser llamado sincrónicamente durante la fase `setup()` de un componente.

- **Ejemplo**

  ```vue
  <script setup>
  import { ref, provide } from 'vue'
  import { fooSymbol } from './injectionSymbols'

  // proporcionar un valor estático
  provide('foo', 'bar')

  // proporcionar un valor reactivo
  const count = ref(0)
  provide('count', count)

  // proporcionar con claves de symbol
  provide(fooSymbol, count)
  </script>
  ```

- **Véase también**:
  - [Guía - provide / inject](/guide/components/provide-inject)
  - [Guía - Tipado de provide / inject](/guide/typescript/composition-api#typing-provide-inject)

## inject() {#inject}

Inyecta un valor proporcionado por un componente de nivel superior o la aplicación (a través de `app.provide()`).

- **Tipo**

  ```ts
  // sin valor predeterminado
  function inject<T>(key: InjectionKey<T> | string): T | undefined

  // con valor predeterminado
  function inject<T>(key: InjectionKey<T> | string, defaultValue: T): T

  // con factoría
  function inject<T>(
    key: InjectionKey<T> | string,
    defaultValue: () => T,
    treatDefaultAsFactory: true
  ): T
  ```

- **Detalles**

  El primer argumento es la clave de inyección. Vue recorrerá la estructura padre para localizar un valor proporcionado con una clave coincidente. Si varios componentes en la estructura padre proporcionan la misma clave, el más cercano al componente que inyecta "ocultará" a los que están más arriba en la cadena. Si no se encuentra ningún valor con la clave coincidente, `inject()` devuelve `undefined` a menos que se proporcione un valor predeterminado.

  El segundo argumento es opcional y es el valor predeterminado que se utilizará cuando no se encuentre ningún valor coincidente. También puede ser una función de factoría para devolver valores que son costosos de crear. Si el valor predeterminado es una función, entonces se debe pasar `false` como tercer argumento para indicar que la función debe utilizarse como el valor en lugar de la factoría.

  Similar a las APIs de registro de hooks de ciclo de vida, `inject()` debe ser llamado sincrónicamente durante la fase `setup()` de un componente.

  Cuando se usa TypeScript, la clave puede ser de tipo `InjectionKey` - un tipo de utilidad proporcionado por Vue que extiende `Symbol`, que se puede utilizar para sincronizar el tipo de valor entre `provide()` e `inject()`.

- **Ejemplo**

  Suponiendo que un componente padre ha proporcionado valores como se muestra en el ejemplo anterior de `provide()`:

  ```vue
  <script setup>
  import { inject } from 'vue'
  import { fooSymbol } from './injectionSymbols'

  // inyectar valor estático con valor predeterminado
  const foo = inject('foo')

  // inyectar valor reactivo
  const count = inject('count')

  // inyectar con claves de símbolo
  const foo2 = inject(fooSymbol)

  // inyectar con valor predeterminado
  const bar = inject('foo', 'valor predeterminado')

  // inyectar con factoría de valor predeterminado
  const baz = inject('foo', () => new Map())

  // inyectar con valor predeterminado de función, pasando el 3er argumento
  const fn = inject('función', () => {}, false)
  </script>
  ```

- **Véase también**:
  - [Guía - provide / inject](/guide/components/provide-inject)
  - [Guía - Tipado de provide / inject](/guide/typescript/composition-api#typing-provide-inject)
