# Curso Vue.js 💻️ 🛠️
## Tutor: David Leda
### Entrega: Complementario Componentes
---

Hola 🙋‍♂️️ Aquí les paso algunas recomendaciones para el entregable 3: __Complementario Componentes__.

Este entregable __no es obligatorio__, si quieren no lo hagan, aunque les recomiendo que lo intenten así se van familiarizando con [Vue SFC](https://es.vuejs.org/v2/guide/single-file-components.html) (lo que en las diapos figura, erróneamente, como Vue CLI 🤦‍♂️️).

__1. GitHub:__ Por favor hagan la entrega con __un link al repositorio en GitHub__, no en un zip ni Dropbox 🙏️🙏️🙏️

__2. Un repositorio por entrega:__ Por favor __no pongan todas las entregas en un mismo repositorio__ (ya sea distintas entregas en distintas carpetas o en distintas ramas del repositorio). Por favor __inicien un repositorio nuevo para cada entrega__ 🙏️🙏️🙏️

__3. Errores en consola:__ Antes de hacer la entrega __chequeen si no les está dando algún error por consola.__ Si no pueden solucionar el error no hay problema, pueden entregar igual, pero en ese caso avisen al hacer la entrega: "Me está dando tal error por consola."

__4. Creación de un proyecto con Vue SFC:__ Recuerden que primero tienen que tener instalado [Node.js](https://nodejs.org/es). 

Luego abren en la terminal el directorio en el que tienen sus proyectos de Vue (si usan Windows sería con click derecho sobre la carpeta, luego `Open in Terminal`) y en la terminal ponen:

```sh
# para iniciar un proyecto con Vue 2
npm init vue@2

# para iniciar un proyecto con Vue 3
npm init vue@3
```
Si les pide que instalen el _package_ `create-vue` le ponen que sí.

Luego les va a pedir que ingresen el nombre del nuevo proyecto y distintas opciones. Por ahora le puden poner no a todo.

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

Si todo salió bien, ahí van a ver el proyecto con el `scaffolding` inicial de Vite + Vue.

Seguramente les va a aparecer en VS Code un cartel sugiriendo que instalen la extensión __Volar__ para Vue. Les recomiendo que la instalen, pero antes chequeen si no tienen instalada __Vetur__ (otra extensión para Vue) y desinstalen Vetur ya que no es conveniente que estén las dos activas al mismo tiempo.

Puede ser que Volar les subraye el tag `script` como si estuviese mal. Esto es porque Volar está pre-configurada para usar la Composition API con el tag `<script setup>` o con TypeScript, con el tag `<script lang="ts">`. Pueden ignorarlo (no afecta en nada) o pueden incluir `lang="ts"` para que desaparezca ese subrayado.

__5. Vue 2 / Vue 3:__ Si optan por iniciar el proyecto con Vue 3 van a ver que el archivo __main.js__ es bastante distinto al de Vue 2:

Con __Vue 2__:

```js
import Vue from 'vue'

import App from './App.vue'
import router from './router'

import './assets/main.css'

new Vue({
  router,
  render: (h) => h(App)
}).$mount('#app')
```

Con __Vue 3__:

```js
import './assets/main.css'

import { createApp } from 'vue'

import App from './App.vue'
import router from './router'

const app = createApp(App)

app.use(router)
app.mount('#app')
```

Es decir que __el funcionamiento interno de Vue en cada una de las dos versiones es muy distinto__. En Vue 2 lo primero que se ejecuta al iniciar la aplicación es la importación de __la librería de Vue entera__ (`import Vue from 'vue'`) mientras que en Vue 3 lo primero que se importa es __únicamente la función `createApp`__ (`import { createApp } from 'vue'`). Al estar importando únicamente una función esto hace que __las aplicaciones creadas con Vue 3 tengan menor peso final que las creadas con Vue 2.__

El problema es que, al ser tan distinto el funcionamiento, __no es posible pasar de una versión a otra solamente cambiando el número de versión en *package.json*__. Para cambiar de versión deben primero cambiar el contenido del archivo __main.js__, pero también tienen que tener en cuenta otras diferencias entre las versiones, como las que se [explican acá](https://www.freecodecamp.org/news/migrate-from-vue2-to-vue3-with-example-project/). Por ejemplo, en Vue 3 ya no hay más __filters__, en su reemplazo deben usar un _method_:

```js
// En Vue 2:
filters: {
  capitalize(string) {
    return string.replace(/\b\w/g, first => first.toUpperCase())
  }
},

// En Vue 3:
methods: {
  capitalize(string) {
    return string.replace(/\b\w/g, first => first.toUpperCase())
  }
}
```

Y si quieren usar __BootstrapVue__, o __VueForm__, en Vue 3 __no van a funcionar__. En Vue 3, para los estilos pueden usar usar CSS nativo, o Tailwind, o [Element Plus](https://element-plus.org/es-ES/), o [Vuestic](https://ui.vuestic.dev/), o [PrimeVue](https://primevue.org/installation), o [Vuetify](https://vuetifyjs.com/en/). Y para los formularios pueden usar [FormKit](https://formkit.com/) o [Vuelidate](https://vuelidate-next.netlify.app/). 

Y recuerden que __la versión de Vue Router para Vue 2 es *Vue Router 3* y la versión para Vue 3 es *Vue Router 4*__. Y deben chequear las diferencias entre el funcionamiento de __Vue Router 3 y Vue Router 4__ [explicadas acá](https://router.vuejs.org/guide/migration/).



__6. PNPM:__ Les recomiendo que en lugar de `npm` usen [pnpm](https://pnpm.io/) que funciona mucho más rápido. Esto es porque `pnpm` reutiliza las dependencias que ya tienen instaladas, lo cual es muy práctico si están creando varios proyectos con las mismas dependencias, como vamos a hacer en este curso. En cambio `npm` vuelve a instalar todo de cero para cada proyecto y funciona mucho más lento. Para poder usar `pnpm` primero deben instalarlo en forma global:

```sh
npm install -g pnpm
```

Y luego, para crear un proyecto de Vue 3 + Vite con `pnpm`:

```sh
pnpm create vue@3
```



__7. Componentes:__ Les recomiendo que borren todos los componentes que se instalaron por defecto al crear el proyecto, excepto `App.vue.`. En App.vue borren todo el contenido, ya que, como habrán visto, usa la sintaxis de la [Composition API](https://vuejs.org/api/sfc-script-setup.html) (lo que en las diapos figura, erróneamente, como Vue CLI 3) que vamos a ver al final del curso (con el `script` arriba y el `template` abajo) y crean un componente con la sintaxis de la [Options API](https://vuejs.org/guide/extras/composition-api-faq.html#will-options-api-be-deprecated) (lo que en las diapos figura, erróneamente, como Vue CLI 2):

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



__8. CSS: No es obligatorio usar Bootstrap.__ Si tienen un archivo de CSS global pueden ponerlo dentro del directorio `/assets` en un nuevo directorio que se llame `/css`, y luego importarlo en `main.js`:

```js
import { createApp } from 'vue'
import App from './App.vue'

import './assets/css/main.css'

createApp(App).mount('#app')
```

Todo lo demás (uso de `props`, uso de `v-for`) es igual al entregable anterior.

<hr>

Cualquier duda que tengan me pueden escribir, preferentemente por Discord ya que la plataforma de CH no siempre funciona bien.

<hr>
