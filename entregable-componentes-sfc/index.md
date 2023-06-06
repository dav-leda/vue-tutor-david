# Curso Vue.js 💻️ 🛠️
## Tutor: David Leda

---

Hola 🙋‍♂️️ Aquí les paso algunas recomendaciones para el entregable 3: __Complementario Componentes__.

Este entregable __no es obligatorio__, si quieren no lo hagan, aunque les recomiendo que lo intenten así se van familiarizando con [Vue SFC](https://es.vuejs.org/v2/guide/single-file-components.html) (lo que en las diapos figura, erróneamente, como Vue CLI 🤦‍♂️️).

__1. GitHub:__ Por favor hagan la entrega con __un link al repositorio en GitHub__, no en un zip ni Dropbox 🙏️🙏️🙏️

__2. Un repositorio por entrega:__ Por favor __no pongan todas las entregas en un mismo repositorio__ (ya sea distintas entregas en distintas carpetas o en distintas ramas del repositorio). Por favor __inicien un repositorio nuevo para cada entrega__ 🙏️🙏️🙏️

__3. Errores en consola:__ Antes de hacer la entrega __chequeen si no les está dando algún error por consola.__ Si no pueden solucionar el error no hay problema, pueden entregar igual, pero en ese caso avisen al hacer la entrega: "Me está dando tal error por consola."

__4. Creación de un proyecto con Vue SFC:__ Recuerden que, antes que nada, tienen que tener instalado [Node.js](https://nodejs.org/es). 

Luego abren en la terminal el directorio en el que tienen sus proyectos de Vue (si usan Windows sería con click derecho sobre la carpeta, luego `Open in Terminal`) y en la terminal ponen:

```sh
npm init vue@3
```

Ahí les va a pedir que ingresen el nombre del nuevo proyecto y distintas opciones. Por ahora le puden poner NO a todo.

Luego se dirigen al dir donde está el nuevo proyecto, instalan las deps y abren VS Code:

```sh
cd nombre-del-proyecto
npm i
code .
```
Luego, en la terminal de VS Code:

```sh
npm run dev
```
Y abren el browser en la URL que les indica (generalmente es `http://localhost:5173/`).

Si todo salió bien, ahí van a ver el proyecto con el `scaffolding` inicial de Vite + Vue 3.

Seguramente les va a aparecer en VS Code un cartel sugiriendo que instalen la extensión __Volar__ para Vue. Les recomiendo que la instalen, pero antes chequeen si no tienen instalada __Vetur__ (otra extensión para Vue) y desinstalen Vetur ya que no es conveniente que estén las dos activas al mismo tiempo.

Puede ser que Volar les subraye el tag `script` como si estuviese mal. Esto es porque Volar está pre-configurada para usar la Composition API con el tag `<script setup>` o con TypeScript, con el tag `<script lang="ts">`. Pueden ignorarlo (no afecta en nada) o pueden incluir `lang="ts"` para que desaparezca ese subrayado.

__5. PNPM:__ Les recomiendo que en lugar de `npm` usen [pnpm](https://pnpm.io/) que funciona mucho más rápido. Esto es porque `pnpm` reutiliza las dependencias que ya tienen instaladas, lo cual es muy práctico si están creando varios proyectos con las mismas deps, como vamos a hacer en este curso. En cambio `npm` vuelve a instalar todo de cero para cada proyecto y funciona mucho más lento. Para poder usar `pnpm` primero deben instalarlo en forma global:

```sh
npm install -g pnpm
```

Y luego, para crear un proyecto de Vue 3 + Vite con `pnpm`:

```sh
pnpm create vue@3
```

__6. Componentes:__ Les recomiendo que borren todos los componentes que se instalaron por defecto al crear el proyecto, excepto `App.vue.`. En App.vue borren todo el contenido, ya que, como habrán visto, usa la sintaxis de la [Composition API](https://vuejs.org/api/sfc-script-setup.html) (lo que en las diapos figura, erróneamente, como Vue CLI 3) que vamos a ver al final del curso (con el `script` arriba y el `template` abajo) y crean un componente con la sintaxis de la [Options API](https://vuejs.org/guide/extras/composition-api-faq.html#will-options-api-be-deprecated) (lo que en las diapos figura, erróneamente, como Vue CLI 2):

```html
<template>
  <div>
    
  </div>
</template>

<script>

export default {

  components: {
    
  },

  data: () => ({ 
    
  }),

  computed: {
    
  },

  methods: {
 
  }
}

</script>

<style scoped>

.clase-de-css {

}

</style>
```
Al usar el atributo `scoped` en el tag `style` las clases de css que usen se van a aplicar solamente a este componente y no otro, lo cual es práctico cuando quieren evitar que las clases se pisen entre sí y den resultados inesperados. Pero si quieren declarar clases globales pueden borrar el atributo `scoped`.

Luego crean el componente de la tabla usando esta misma estructura e importan el componente de la tabla en App.vue:

```html

<template>

  <h1>Tablas de productos:</h1>
  
  <TableComponent
    v-for="(table, i) in tables" :key="i"
    :products="table"
    :color="colors[i]"
  />

</template>

<script>

import TableComponent from './components/TableComponent.vue'

export default {

  components: {
    TableComponent
  },

  data: () => ({ 
    tables: [],
    colors: []
  }),

  computed: {

  },

  methods: {

  }
}

</script>
```

A diferencia de Vue CDN, en donde la convención es declarar los componentes usando kebab-case, en Vue SFC la convención es nombrarlos usando PascalCase: `<TableComponent/>`. Pero pueden hacerlo con kebab-case si prefieren: `<table-component/>`.

__7. CSS: No es obligatorio usar Bootstrap.__ Si tienen un archivo de CSS global pueden ponerlo dentro del directorio `/assets` en un nuevo directorio que se llame `/css`, y luego importarlo en `main.js`:

```js
import { createApp } from 'vue'
import App from './App.vue'

import './assets/css/main.css'

createApp(App).mount('#app')
```

Todo lo demás (uso de `props`, uso de `v-for`) es igual al entregable anterior.

<hr>

Cualquier duda que tengan me pueden escribir, preferentemente por Discord ya que la plataforma de CH no siempre funciona bien 🤷‍♂️️



