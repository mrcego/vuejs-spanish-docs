# Componente v-model {#component-v-model}

## Uso básico {#basic-usage}

`v-model` se puede usar en un componente para implementar una vinculación bidireccional.

<div class="composition-api">

A partir de Vue 3.4, el método recomendado para lograr esto es usando la macro [`defineModel()`](/api/sfc-script-setup#definemodel):

```vue
<!-- Child.vue -->
<script setup>
const model = defineModel()

function update() {
  model.value++
}
</script>

<template>
  <div>El `v-model` vinculado al padre es: {{ model }}</div>
</template>
```

El padre puede entonces enlazar un valor con `v-model`:

```vue-html
<!-- Parent.vue -->
<Child v-model="count" />
```

El valor devuelto por `defineModel()` es una referencia (`ref`). Puede ser accedido y mutado como cualquier otra referencia, excepto que actúa como un enlace bidireccional entre un valor del padre y uno local:

- Su `.value` está sincronizado con el valor enlazado por el `v-model` del padre;
- Cuando es mutado por el hijo, también hace que el valor enlazado del padre se actualice.

Esto significa que también puedes enlazar esta referencia a un elemento de entrada nativo con `v-model`, lo que facilita envolver elementos de entrada nativos mientras se proporciona el mismo uso de `v-model`:

```vue
<script setup>
const model = defineModel()
</script>

<template>
  <input v-model="model" />
</template>
```

[Pruébalo en la Zona de Práctica](https://play.vuejs.org/#eNqFUtFKwzAU/ZWYl06YLbK30Q10DFSYigq+5KW0t11mmoQknZPSf/cm3eqEsT0l555zuefmpKV3WsfbBuiUpjY3XDtiwTV6ziSvtTKOLNZcFKQ0qiZRnATkG6JB0BIDJen2kp5iMlfSOlLbisw8P4oeQAhFPpURxVV0zWSa9PNwEgIHtRaZA0SEpOvbeduG5q5LE0Sh2jvZ3tSqADFjFHlGSYJkmhz10zF1FseXvIo3VklcrfX9jOaq1lyAedGOoz1GpyQwnsvQ3fdTqDnTwPhQz9eQf52ob+zO1xh9NWDBbIHRgXOZqcD19PL9GXZ4H0h03whUnyHfwCrReI+97L6RBdo+0gW3j+H9uaw+7HLnQNrDUt6oV3ZBzyhmsjiz+p/dSTwJfUx2+IpD1ic+xz5enwQGXEDJJaw8Gl2I1upMzlc/hEvdOBR6SNKAjqP1J6P/o6XdL11L5h4=)

### Detrás de escena {#under-the-hood}

`defineModel` es un macro de conveniencia. El compilador lo expande a lo siguiente:

- Una propiedad llamada `modelValue`, con la cual se sincroniza el valor del ref local;
- Un evento llamado `update:modelValue`, que se emite cuando se muta el valor del ref local.

Así es como implementarías el mismo componente hijo mostrado anteriormente antes de la versión 3.4:

```vue
<script setup>
const props = defineProps(['modelValue'])
const emit = defineEmits(['update:modelValue'])
</script>

<template>
  <input
    :value="props.modelValue"
    @input="emit('update:modelValue', $event.target.value)"
  />
</template>
```

Como puedes ver, es un poco más largo. Sin embargo, es útil entender qué está sucediendo internamente.

Dado que `defineModel` declara una prop, puedes declarar las opciones de la prop asociadas pasándola a `defineModel`:

```js
// Haciendo que el v-model sea obligatorio
const model = defineModel({ required: true })

// Proporcionando un valor predeterminado
const model = defineModel({ default: 0 })
```

:::warning
Si tienes un valor `default` para la prop `defineModel` y no proporcionas ningún valor para esta prop desde el componente padre, puede causar una desincronización entre el componente padre y el componente hijo. En el ejemplo siguiente, el `myRef` del padre es indefinido, pero el `model` del hijo es 1:

```js
// componente hijo:
const model = defineModel({ default: 1 })

// componente padre:
const myRef = ref()
```

```html
<Child v-model="myRef"></Child>
```

:::

</div>

<div class="options-api">

Primero, volvamos a revisar cómo se utiliza `v-model` en un elemento nativo:

```vue-html
<input v-model="searchText" />
```

Internamente, el compilador de plantillas expande `v-model` a su equivalente más detallado para nosotros. Entonces, el código anterior hace lo mismo que lo siguiente:

```vue-html
<input
  :value="searchText"
  @input="searchText = $event.target.value"
/>
```

Cuando se utiliza en un componente, `v-model` se expande a esto:

```vue-html
<CustomInput
  :model-value="searchText"
  @update:model-value="newValue => searchText = newValue"
/>
```

Sin embargo, para que esto funcione realmente, el componente `<CustomInput>` debe hacer dos cosas:

1. Vincular el atributo `value` de un elemento nativo `<input>` a la propiedad `modelValue`
2. Cuando se desencadena un evento `input` nativo, emitir un evento personalizado `update:modelValue` con el nuevo valor

Aquí está en acción:

```vue
<!-- CustomInput.vue -->
<script>
export default {
  props: ['modelValue'],
  emits: ['update:modelValue']
}
</script>

<template>
  <input
    :value="modelValue"
    @input="$emit('update:modelValue', $event.target.value)"
  />
</template>
```

Ahora `v-model` debería funcionar perfectamente con este componente:

```vue-html
<CustomInput v-model="searchText" />
```

[Pruébalo en la Zona de Práctica](https://play.vuejs.org/#eNqFkctqwzAQRX9lEAEn4Np744aWrvoD3URdiHiSGvRCHpmC8b93JDfGKYGCkJjXvTrSJF69r8aIohHtcA69p6O0vfEuELzFgZx5tz4SXIIzUFT1JpfGCmmlxe/c3uFFRU0wSQtwdqxh0dLQwHSnNJep3ilS+8PSCxCQYrC3CMDgMKgrNlB8odaOXVJ2TgdvvNp6vSwHhMZrRcgRQLs1G5+M61A/S/ErKQXUR5immwXMWW1VEKX4g3j3Mo9QfXCeKU9FtvpQmp/lM0Oi6RP/qYieebHZNvyL0acLLODNmGYSxCogxVJ6yW1c2iWz/QOnEnY48kdUpMIVGSllD8t8zVZb+PkHqPG4iw==)

Otra forma de implementar `v-model` dentro de este componente es usar una propiedad computada modificable con un getter y un setter. El método `get` debería devolver la propiedad `modelValue` y el método `set` debería emitir el evento correspondiente:

```vue
<!-- CustomInput.vue -->
<script>
export default {
  props: ['modelValue'],
  emits: ['update:modelValue'],
  computed: {
    value: {
      get() {
        return this.modelValue
      },
      set(value) {
        this.$emit('update:modelValue', value)
      }
    }
  }
}
</script>

<template>
  <input v-model="value" />
</template>
```

</div>

## Argumentos de `v-model` {#v-model-arguments}

```vue
<MyComponent v-model:title="bookTitle" />
```

<div class="composition-api">

En un componente hijo, podemos admitir el argumento correspondiente pasando una cadena a `defineModel()` como su primer argumento:

```vue
<!-- MyComponent.vue -->
<script setup>
const title = defineModel('title')
</script>

<template>
  <input type="text" v-model="title" />
</template>
```

[Pruébalo en la Zona de Práctica](https://play.vuejs.org/#eNqFkl9PwjAUxb9K05dhglsMb2SQqOFBE9Soj31Zxh0Uu7bpHxxZ9t29LWOiQXzaes7p2a+9a+mt1unOA53S3JaGa0csOK/nTPJaK+NISwxUpCOVUTVJMJoM1nJ/r/BNgnS9nWYnWujFMCFMlkpaRxx3AsgsFI6S3XWtViBIYda+Dg3QFLUWkFwxmWcHFqTAhQPUCwe4IiTf3Mzbtq/qujzDddRPYfruaUzNGI1PRkmG0Twb+uiY/sI9cw0/0VdQcQnL0D5KovgfL5fa4/69jiDQOOTo+S6SOYtfrvg63VolkauNN0lLxOUCzLN2HMkYnZLoBK8QQn0+Rs0ZD+OjXm6g/Dijb20TNEZfDFgwOwQZPIdzAWQN9uLtKXIPJtL7gH3BfAWrhA+Mh9idlyvEPslF2of4J3G5freLxoG0x0MF0JDsYp5RHE6Y1F9H/8adpJO4j8mOdl/Hw/nf)

Si también se necesitan opciones de prop, deben pasarse después del nombre del model:

```js
const title = defineModel('title', { required: true })
```

<details>
<summary>Uso previo a 3.4</summary>

```vue
<!-- MyComponent.vue -->
<script setup>
defineProps({
  title: {
    required: true
  }
})
defineEmits(['update:title'])
</script>

<template>
  <input
    type="text"
    :value="title"
    @input="$emit('update:title', $event.target.value)"
  />
</template>
```

[Pruébalo en la Zona de Práctica](https://play.vuejs.org/#eNp9kE1rwzAMhv+KMIW00DXsGtKyMXYc7D7vEBplM8QfOHJoCfnvk+1QsjJ2svVKevRKk3h27jAGFJWoh7NXjmBACu4kjdLOeoIJPHYwQ+ethoJLi1vq7fpi+WfQ0JI+lCstcrkYQJqzNQMBKeoRjhG4LcYHbVvsofFfQUcCXhrteix20tRl9sIuOCBkvSHkCKD+fjxN04Ka57rkOOlrMwu7SlVHKdIrBZRcWpc3ntiLO7t/nKHFThl899YN248ikYpP9pj1V60o6sG1TMwDU/q/FZRxgeIPgK4uGcQLSZGlamz6sHKd1afUxOoGeeT298A9bHCMKxBfE3mTSNjl1vud5x8qNa76)

</details>
</div>
<div class="options-api">

En este caso, en lugar de la prop `modelValue` predeterminada y el evento `update:modelValue`, el componente hijo debe esperar una prop `title` y emitir un evento `update:title` para actualizar el valor del padre:

```vue
<!-- MyComponent.vue -->
<script>
export default {
  props: ['title'],
  emits: ['update:title']
}
</script>

<template>
  <input
    type="text"
    :value="title"
    @input="$emit('update:title', $event.target.value)"
  />
</template>
```

[Pruébalo en la Zona de Práctica](https://play.vuejs.org/#eNqFUNFqwzAM/BVhCm6ha9hryMrGnvcFdR9Mo26B2DGuHFJC/n2yvZakDAohtuTTne5G8eHcrg8oSlFdTr5xtFe2Ma7zBF/Xz45vFi3B2XcG5K6Y9eKYVFZZHBK8xrMOLcGoLMDphrqUMC6Ypm18rzXp9SZjATxS8PZWAVBDLZYg+xfT1diC9t/BxGEctHFtlI2wKR78468q7ttzQcgoTcgVQPXzuh/HzAnTVBVcp/58qz+lMqHelEinElAwtCrufGIrHhJYBPdfEs53jkM4yEQpj8k+miYmc5DBcRKYZeXxqZXGukDZPF1dWhQHUiK3yl63YbZ97r6nIe6uoup6KbmFFfbRCnHGyI4iwyaPPnqffgGMlsEM)

</div>

## Multiples vinculaciones a `v-model` {#multiple-v-model-bindings}

```vue
<UserName
  v-model:first-name="first"
  v-model:last-name="last"
/>
```

<div class="composition-api">

Al aprovechar la capacidad de apuntar a una prop y evento específicos como aprendimos anteriormente con los [`argumentos de v-model`](#v-model-arguments), ahora podemos crear múltiples enlaces `v-model` en una sola instancia de componente.

Cada `v-model` se sincronizará con una prop diferente, sin la necesidad de opciones adicionales en el componente:

```vue
<script setup>
const firstName = defineModel('firstName')
const lastName = defineModel('lastName')
</script>

<template>
  <input type="text" v-model="firstName" />
  <input type="text" v-model="lastName" />
</template>
```

[Pruébalo en la Zona de Práctica](https://play.vuejs.org/#eNqFkstuwjAQRX/F8iZUAqKKHQpIfbAoUmnVx86bKEzANLEt26FUkf+9Y4MDSAg2UWbu9fjckVv6oNRw2wAd08wUmitLDNhGTZngtZLakpZoKIkjpZY1SdCadNK3Ab3IazhowzQ2/ES0MVFIYSwpucbvxA/qJXO5FsldlKr8qDxL8EKW7kEQAQsLtapyC1gRkq3vp217mOccwf8wwLksRSlYIoMvCNkOarmEahyODAT2J4yGgtFzhx8UDf5/r6c4NEs7CNqnpxkvbO0kcVjNhCyh5AJe/SW9pBPOV3DJGvu3dsKFaiyxf8qTW9gheQwVs4Z90BDm5oF47cF/Ht4aZC75argxUmD61g9ktJC14hXoN2U5ZmJ0TILitbyq5O889KxuoB/7xRqKnwv9jdn5HqPvGnDVWwTpNJvrFSCul2efi4DeiRigqdB9RfwAI6vGM+5tj41YIvaJL9C+hOfNxerLzHYWhImhPKh3uuBnFJ/A05XoR9zRcBTOMeGo+wcs+yse)

<details>
<summary>Uso previo a 3.4</summary>

```vue
<script setup>
defineProps({
  firstName: String,
  lastName: String
})

defineEmits(['update:firstName', 'update:lastName'])
</script>

<template>
  <input
    type="text"
    :value="firstName"
    @input="$emit('update:firstName', $event.target.value)"
  />
  <input
    type="text"
    :value="lastName"
    @input="$emit('update:lastName', $event.target.value)"
  />
</template>
```

[Pruébalo en la Zona de Práctica](https://play.vuejs.org/#eNqNUc1qwzAMfhVjCk6hTdg1pGWD7bLDGIydlh1Cq7SGxDaOEjaC332yU6cdFNpLsPRJ348y8idj0qEHnvOi21lpkHWAvdmWSrZGW2Qjs1Azx2qrWyZoVMzQZwf2rWrhhKVZbHhGGivVTqsOWS0tfTeeKBGv+qjEMkJNdUaeNXigyCYjZIEKhNY0FQJVjBXHh+04nvicY/QOBM4VGUFhJHrwBWPDutV7aPKwslbU35Q8FCX/P+GJ4oB/T3hGpEU2m+ArfpnxytX2UEsF71abLhk9QxDzCzn7QCvVYeW7XuGyWSpH0eP6SyuxS75Eb/akOpn302LFYi8SiO8bJ5PK9DhFxV/j0yH8zOnzoWr6+SbhbifkMSwSsgByk1zzsoABFKZY2QNgGpiW57Pdrx2z3JCeI99Svvxh7g8muf2x)

</details>
</div>
<div class="options-api">

```vue
<script>
export default {
  props: {
    firstName: String,
    lastName: String
  },
  emits: ['update:firstName', 'update:lastName']
}
</script>

<template>
  <input
    type="text"
    :value="firstName"
    @input="$emit('update:firstName', $event.target.value)"
  />
  <input
    type="text"
    :value="lastName"
    @input="$emit('update:lastName', $event.target.value)"
  />
</template>
```

[Pruébalo en la Zona de Práctica](https://play.vuejs.org/#eNqNkk1rg0AQhv/KIAETSJRexYYWeuqhl9JTt4clmSSC7i7rKCnif+/ObtYkELAiujPzztejQ/JqTNZ3mBRJ2e5sZWgrVNUYbQm+WrQfskE4WN1AmuXRwQmpUELh2Qv3eJBdTTAIBbDTLluhoraA4VpjXHNwL0kuV0EIYJE6q6IFcKhsSwWk7/qkUq/nq5be+aa5JztGfrmHu8t8GtoZhI2pJaGzAMrT03YYQk0YR3BnruSOZe5CXhKnC3X7TaP3WBc+ZaOc/1kk3hDJvYILRQGfQzx3Rct8GiJZJ7fA7gg/AmesNszMrUIXFpxbwCfZSh09D0Hc7tbN6sAWm4qZf6edcZgxrMHSdA3RF7PTn1l8lTIdhbXp1/CmhOeJRNHLupv4eIaXyItPdJEFD7R8NM0Ce/d/ZCTtESnzlVZXhP/vHbeZaT0tPdf59uONfx7mDVM=)

</div>

## Manejo de modificadores de `v-model` {#handling-v-model-modifiers}

Cuando estábamos aprendiendo sobre las vinculaciones de entrada de formularios, vimos que `v-model` tiene [modificadores integrados](/guide/essentials/forms#modifiers): `.trim`, `.number` y `.lazy`. En algunos casos, es posible que también desees que el `v-model` en tu componente de entrada personalizado admita modificadores personalizados.

Creemos un ejemplo de modificador personalizado, `capitalize`, que capitalice la primera letra de la cadena de texto proporcionada por la vinculación de `v-model`:

```vue-html
<MyComponent v-model.capitalize="myText" />
```

<div class="composition-api">

Los modificadores agregados a un componente `v-model` pueden ser accedidos en el componente hijo mediante la destructuración del valor de retorno de `defineModel()`, de esta manera:

```vue{4}
<script setup>
const [model, modifiers] = defineModel()

console.log(modifiers) // { capitalize: true }
</script>

<template>
  <input type="text" v-model="model" />
</template>
```

Para ajustar condicionalmente cómo debe ser leído o escrito el valor en función de los modificadores, podemos pasar opciones `get` y `set` a `defineModel()`. Estas dos opciones reciben el valor en la obtención / establecimiento de la ref del model y deben devolver un valor transformado. Así es como podemos utilizar la opción `set` para implementar el modificador `capitalize`:

```vue{6-8}
<script setup>
const [model, modifiers] = defineModel({
  set(value) {
    if (modifiers.capitalize) {
      return value.charAt(0).toUpperCase() + value.slice(1)
    }
    return value
  }
})
</script>

<template>
  <input type="text" v-model="model" />
</template>
```

[Pruébalo en la Zona de Práctica](https://play.vuejs.org/#eNp9UsFu2zAM/RVClzhY5mzoLUgHdEUPG9Bt2LLTtIPh0Ik6WxIkyosb5N9LybFrFG1OkvgeyccnHsWNtXkbUKzE2pdOWQKPFOwnqVVjjSM4gsMKTlA508CMqbMRuu9uDd80ajrD+XISi3WZDCB1abQnaLoNHgiuY8VsNptLvV72TbkdPwgbWxeE/ALY7JUHpW0gKAurqKjVI3rAFl1He6V30JkA3AbdKvLXUzXt+8Zssc6fM6+l6NtLAUtusF6O3cRCvFB9yY2SiYFw+8KSYcY/qfEC+FCVQuf/8rxbrJTG+4hkxyiWq2ZtUQecQ3oDqAqyMWeieyQAu0bBaUh5ebkv3A1lH+Y5md/WorstPGZzeHfGfa1KzD6yxzH11B/TCjHC4dPlX1j3P0CdjQ5S79/Z3WhpPF91lDz7Uald/uCNZj/TFFJE91SN7rslxX5JsRrmk6Koa/P/a4qRC7gY4uUey3+vxB/8Icak+OHQo2tRihGjwu2QtUb47te3pHsEWXWomX0B/Ine1CFq7Gmfg96y7Akvqf2StoKXcePvDoTaD0NFocnhxJeClyRu2FujP8u9yq+GnxGnJxSEO+M=)

<details>
<summary>Uso previo a 3.4</summary>

```vue{11-13}
<script setup>
const props = defineProps({
  modelValue: String,
  modelModifiers: { default: () => ({}) }
})

const emit = defineEmits(['update:modelValue'])

function emitValue(e) {
  let value = e.target.value
  if (props.modelModifiers.capitalize) {
    value = value.charAt(0).toUpperCase() + value.slice(1)
  }
  emit('update:modelValue', value)
}
</script>

<template>
  <input type="text" :value="modelValue" @input="emitValue" />
</template>
```

[Pruébalo en la Zona de Práctica](https://play.vuejs.org/#eNp9Us1Og0AQfpUJF5ZYqV4JNTaNxyYmVi/igdCh3QR2N7tDIza8u7NLpdU0nmB+v5/ZY7Q0Jj10GGVR7iorDYFD6sxDoWRrtCU4gsUaBqitbiHm1ngqrfuV5j+Fik7ldH6R83u5GaBQlVaOoO03+Emw8BtFHCeFyucjKMNxQNiapiTkCGCzlw6kMh1BVRpJZSO/0AEe0Pa0l2oHve6AYdBmvj+/ZHO4bfUWm/Q8uSiiEb6IYM4A+XxCi2bRH9ZX3BgVGKuNYwFbrKXCZx+Jo0cPcG9l02EGL2SZ3mxKr/VW1hKty9hMniy7hjIQCSweQByHBIZCDWzGDwi20ps0Yjxx4MR73Jktc83OOPFHGKk7VZHUKkyFgsAEAqcG2Qif4WWYUml3yOp8wldlDSLISX+TvPDstAemLeGbVvvSLkncJSnpV2PQrkqHLOfmVHeNrFDcMz3w0iBQE1cUzMYBbuS2f55CPj4D6o0/I41HzMKsP+u0kLOPoZWzkx1X7j18A8s0DEY=)

</details>
</div>

<div class="options-api">

Los modificadores agregados a un componente `v-model` serán proporcionados al componente a través de la propiedad `modelModifiers`. En el ejemplo a continuación, hemos creado un componente que contiene una propiedad `modelModifiers` que por defecto es un objeto vacío:

```vue{11}
<script>
export default {
  props: {
    modelValue: String,
    modelModifiers: {
      default: () => ({})
    }
  },
  emits: ['update:modelValue'],
  created() {
    console.log(this.modelModifiers) // { capitalize: true }
  }
}
</script>

<template>
  <input
    type="text"
    :value="modelValue"
    @input="$emit('update:modelValue', $event.target.value)"
  />
</template>
```

Observa que la propiedad `modelModifiers` del componente contiene `capitalize` y su valor es `true`, debido a que se estableció en la vinculación `v-model` como `v-model.capitalize="myText"`.

Ahora que tenemos nuestra propiedad configurada, podemos verificar las claves del objeto `modelModifiers` y escribir un controlador para cambiar el valor emitido. En el código a continuación, capitalizaremos la cadena de texto cada vez que el elemento `<input />` genere un evento `input`.

```vue{13-15}
<script>
export default {
  props: {
    modelValue: String,
    modelModifiers: {
      default: () => ({})
    }
  },
  emits: ['update:modelValue'],
  methods: {
    emitValue(e) {
      let value = e.target.value
      if (this.modelModifiers.capitalize) {
        value = value.charAt(0).toUpperCase() + value.slice(1)
      }
      this.$emit('update:modelValue', value)
    }
  }
}
</script>

<template>
  <input type="text" :value="modelValue" @input="emitValue" />
</template>
```

[Pruébalo en la Zona de Práctica](https://play.vuejs.org/#eNqFks1qg0AQgF9lkIKGpqa9iikNOefUtJfaw6KTZEHdZR1DbPDdO7saf0qgIq47//PNXL2N1uG5Ri/y4io1UtNrUspCK0Owa7aK/0osCQ5GFeCHq4nMuvlJCZCUeHEOGR5EnRNcrTS92VURXGex2qXVZ4JEsOhsAQxSbcrbDaBo9nihCHyXAaC1B3/4jVdDoXwhLHQuCPkGsD/JCmSpa4JUaEkilz9YAZ7RNHSS5REaVQPXgCay9vG0rPNToTLMw9FznXhdHYkHK04Qr4Zs3tL7g2JG8B4QbZS2LLqGXK5PkdcYwTsZrs1R6RU7lcmDRDPaM7AuWARMbf0KwbVdTNk4dyyk5f3l15r5YjRm8b+dQYF0UtkY1jo4fYDDLAByZBxWCmvAkIQ5IvdoBTcLeYCAiVbhvNwJvEk4GIK5M0xPwmwoeF6EpD60RrMVFXJXj72+ymWKwUvfXt+gfVzGB1tzcKfDZec+o/LfxsTdtlCj7bSpm3Xk4tjpD8FZ+uZMWTowu7MW7S+CWR77)

</div>

### Modificadores para `v-model` con argumentos {#modifiers-for-v-model-with-arguments}

<div class="options-api">

Para las vinculaciones `v-model` con argumento y modificadores, el nombre de la propiedad generada será `arg + "Modifiers"`. Por ejemplo:

```vue-html
<MyComponent v-model:title.capitalize="myText">
```

Las declaraciones correspondientes deberían ser:

```js
export default {
  props: ['title', 'titleModifiers'],
  emits: ['update:title'],
  created() {
    console.log(this.titleModifiers) // { capitalize: true }
  }
}
```

</div>

Aquí tienes otro ejemplo de cómo usar modificadores con múltiples `v-model` con diferentes argumentos:

```vue-html
<UserName
  v-model:first-name.capitalize="first"
  v-model:last-name.uppercase="last"
/>
```

<div class="composition-api">

```vue
<script setup>
const [firstName, firstNameModifiers] = defineModel('firstName')
const [lastName, lastNameModifiers] = defineModel('lastName')

console.log(firstNameModifiers) // { capitalize: true }
console.log(lastNameModifiers) // { uppercase: true}
</script>
```

<details>
<summary>Uso previo a 3.4</summary>

```vue{5,6,10,11}
<script setup>
const props = defineProps({
firstName: String,
lastName: String,
firstNameModifiers: { default: () => ({}) },
lastNameModifiers: { default: () => ({}) }
})
defineEmits(['update:firstName', 'update:lastName'])

console.log(props.firstNameModifiers) // { capitalize: true }
console.log(props.lastNameModifiers) // { uppercase: true}
</script>
```

</details>
</div>
<div class="options-api">

```vue{15,16}
<script>
export default {
  props: {
    firstName: String,
    lastName: String,
    firstNameModifiers: {
      default: () => ({})
    },
    lastNameModifiers: {
      default: () => ({})
    }
  },
  emits: ['update:firstName', 'update:lastName'],
  created() {
    console.log(this.firstNameModifiers) // { capitalize: true }
    console.log(this.lastNameModifiers) // { uppercase: true}
  }
}
</script>
```

</div>
