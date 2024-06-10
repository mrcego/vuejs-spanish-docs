<script setup>
  import { ref, onMounted } from 'vue'
  import { data } from './errors.data.ts'
  import ErrorsTable from './ErrorsTable.vue'

  const highlight = ref()
  onMounted(() => {
    highlight.value = location.hash.slice(1)
  })
</script>

# Referencia de Códigos de Error en Producción {#error-reference}

## Errores de Tiempo de Ejecución {#runtime-errors}

En compilaciones de producción, el tercer argumento pasado a las siguientes APIs de manejo de errores será un código abreviado en lugar de la cadena de texto completa de información:

- [`app.config.errorHandler`](/api/application.html#app-config-errorhandler)
- [`onErrorCaptured`](/api/composition-api-lifecycle.html#onerrorcaptured) (Composicion API)
- [`errorCaptured`](/api/options-lifecycle.html#errorcaptured) (Options API)

La siguiente tabla mapea los códigos a sus cadenas de información originales completas.

<ErrorsTable kind="runtime" :errors="data.runtime" :highlight="highlight" />

## Errores del Compilador {#compiler-errors}

La siguiente tabla proporciona un mapeo de los códigos de error del compilador en producción a sus mensajes originales.

<ErrorsTable kind="compiler" :errors="data.compiler" :highlight="highlight" />
