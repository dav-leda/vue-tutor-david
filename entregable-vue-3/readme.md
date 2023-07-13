# Curso Vue.js 💻️ 🛠️
## Tutor: David Leda
### Entregable Complementario: Vue 3
---

Hola 🙋‍♂️️ Acá les paso algunas recomendaciones para el __Entregable Complementario Vue 3__:

__1. Errores en consola:__ Antes de hacer la entrega __por favor chequeen si no les está dando algún error por consola__ 🙏️🙏️🙏️

Si no pueden solucionar el error no hay problema, pueden entregar igual, pero en ese caso avisen al hacer la entrega: "Me está dando tal error por consola."

__2. Consigna:__ La consigna pide __convertir a Vue 3__ alguno de los desafíos de las clases 7 a 12. En esas clases sólo hubo dos desafíos, el del __formulario sin Vuex (clase 8)__ y el del __formulario con Vuex (clase 12)__, todas las demás entregas fueron de Proyecto Final, así que deben elegir entre alguno de esos dos.

__3. Vue 3:__ Recuerden que Vue 3 es solamente una versión de la librería de Vue. A lo que realmente apunta este entregable es a usar la sintaxis de la [Composition API](https://frontendlab.vercel.app/vue/vue2-vue3/#options-api-vs-composition-api) tal como vimos en clase. 

En cuanto a __Vue CLI 3__, es solamente una versión de [Vue CLI](https://cli.vuejs.org/guide/), la línea de comandos para crear proyectos de Vue con Webpack, pero no tienen nada que ver con Vue 3. De hecho, la última versión de Vue CLI __es la 5, no la 3__.

En cuanto a las versiones de Vue, recuerden que a partir del 31/12/2023 Vue 2 va a pasar al estado __End of Life__, por lo cual lo más recomendable es dejar de usar Vue 2. Pero esto __no quiere decir que sea necesario dejar de usar la [Options API](https://frontendlab.vercel.app/vue/vue2-vue3/#options-api-vs-composition-api)__, es decir la forma de crear componentes de Vue con las opciones _data, props, computed, methods_, etc. La Options API puede seguir siendo usada con Vue 3. 

Sin embargo, para este entegable deben usar la __Composition API__, al menos en la forma híbrida que se vio en clase (combinando __setup()__ con __data()__). Aunque la documentación de Vuelidate ofrece como opción esta forma híbrida, les recomiendo que usen únicamente la __Composition API__ (usando __ref__ o __reactive__ en lugar de __data()__ para generar reactividad) ya que esa forma híbrida suele ser usada únicamente como un paso intermedio entre Vue 2 y Vue 3, pero __no es la forma usual de usar Vue 3__, como pueden ver en los ejemplos de la documentación de Vue:

https://vuejs.org/examples/#crud

__4. Setup:__ Les recomiendo que usen `<script setup>` en lugar de la función `setup()` ya que actualmente esta es la forma más usual de usar Vue 3. Además, al usar `<script setup>` __no es necesario retornar un objeto reactivo__ para poder acceder a sus propiedades en el template, esto se da en forma automática. Otra ventaja de `<script setup>` es que __no es necesario declarar los componentes hijos dentro de *components*__. Con sólo importarlos ya están disponibles para ser usados:

```html
<script setup lang="ts">

import FormComponent from '@/components/FormComp.vue'
import TableComponent from '@/components/TableComp.vue'

import { reactive } from 'vue'
import type { PropType } from 'vue'

interface User {
  firstname: string
  lastname: string
  email: string
  password: string
}

// No es necesario importar defineProps para usarlo
defineProps({
  users: {
    type: Array as PropType<User[]>
  }
})

// En cambio reactive sí, debe ser importado
const state = reactive({
  users: [] as User[]
})

// En lugar de methods se pueden declarar funciones normales de JS
// En este caso es con TypeScript, pero no es obligatorio usar TS
function addUser(user: User): void {
  state.users.push(user)
}
</script>

<!-- La convención en Vue 3 es poner el script arriba y el template abajo
pero no es obligatorio hacerlo de esta forma. -->
<template>
    
  <h1>Formulario con Vue 3</h1>
  
  <FormComp @submit-form="addUser"/>
  <TableComp :users="state.users"/>
  
</template>
```

Otra ventaja de Vue 3 es que permite usar __TypeScript__ sin necesidad de instalarlo. De todas formas, __no es obligatorio usar TypeScript en Vue 3.__

__5. Vite:__ Tal como vimos en clase, la forma recomendada de crear proyectos con Vue 3 es usando Vite en lugar de Vue CLI. Para crear un proyecto de Vue 3 con Vite no es necesario instalar nada adicional:

```sh
# npm
npm init vue@3

# pnpm
pnpm create vue@3

# yarn 
yarn create vite
```
Otra opción, en lugar de crear un nuevo proyecto de cero, es migrar el proyecto de Vue CLI a Vite y de Vue 2 a Vue 3.

__6. Vuelidate:__ En el entregable anterior del formulario usábamos la librería __VueForm__, pero VueForm no es compatible con Vue 3, así que deben usar otra librería (Vuelidate es la que vimos en clase, pero hay otras, como FormKit).

Para instalar Vuelidate:

```sh
pnpm i @vuelidate/core @vuelidate/validators
```
Y para usarla en el componente del formulario:

```js
import { useVuelidate } from '@vuelidate/core'

// Importar los métodos de validación:
import { required, alpha, email, minLength, helpers } from '@vuelidate/validators'
```

A diferencia de VueForm, Vuelidate no incluye componentes como `<validate>` o `<field-messages>`. Para el template del formulario se puede usar HTML nativo, siempre recordando usar `@submit.prevent` para evitar el comportamiento por defecto del elemento `<form>`:

```html
<form @submit.prevent="submitForm">
```

__7. v-model:__ La documentación de Vuelidate recomienda usar la propiedad [$model en los v-model del formulario](https://vuelidate-next.netlify.app/guide.html#using-the-model-property). 

En el caso en que estén usando la __Composition API__ no es necesario anteponer `state` (o como hayan llamado al objeto reactivo) a cada propiedad del formulario, con `v$` es suficiente (si están usando la __Options API__, sí, deben anteponer __state, o formData, o formState__, o como hayan llamado al objeto reactivo):

```html
<input
  name="email" 
  type="text"
  v-model="v$.email.$model"
/>
```

A diferencia de VueForm, no es necesario que el __type__ del input coincida con el método de validación, pueden usar cualquier __type__ y la validación es asignada mediante el objeto __v$__.

__8. Data reactiva:__ Recuerden que a diferencia de la Options API, en la Composition API la información reactiva no se declara con __data__ sino con __ref__ (para valores primitivos) o __reactive__ (para objetos o arrays). Para usar __ref__ o __reactive__ antes deben importarlos:

```js
import { reactive } from 'vue' 

const state = reactive({
  firstname: '',
  lastname: '',
  email: '',
  password: ''
})
```

__9. Validaciones:__ Según la documentación de Vuelidate, al usar la Composition API las validaciones deben estar dentro de un objeto llamado [rules](https://vuelidate-next.netlify.app/#alternative-syntax-composition-api), no dentro del método __validations()__:

```js
const rules = {
  firstName: { required, alpha, minLength(2) },
  lastName: { required, alpha, minLength(2) }, 
  email: { required, email },
  password: { required, customValidator } 
}

const v$ = useVuelidate(rules, state)
```

Por defecto, los mensajes de error de Vuelidate están en inglés. Para poder tener los __mensajes en español__ pueden usar el método `helpers.withMessage()`:

```js
const rules = {
  firstname: { 
    required: helpers.withMessage('Este campo es obligatorio.', required),
    alpha: helpers.withMessage('Este campo sólo puede contener letras.', alpha), 
    minLength: helpers.withMessage('Este campo debe tener al menos 2 caracteres.', minLength(2))
  },
  lastname: { 
    required: helpers.withMessage('Este campo es obligatorio.', required),
    alpha: helpers.withMessage('Este campo sólo puede contener letras.', alpha), 
    minLength: helpers.withMessage('Este campo debe tener al menos 2 caracteres.', minLength(2))
  },
  // etc...
}
```
Para no tener que repetir los mensajes, pueden crear un objeto de mensajes:

```js
const mensajes = {
  required: 'Este campo es obligatorio.',
  alpha: 'Este campo sólo puede contener letras.',
  email: 'Formato de e-mail inválido.',
  minLength: 'Este campo debe tener al menos 2 caracteres.',
  passValidator: 'El password debe tener al menos 6 caracteres, una letra y un número.'
}

const rules = {
  firstname: { 
    required: helpers.withMessage(mensajes.required, required),
    alpha: helpers.withMessage(mensajes.alpha, alpha), 
    minLength: helpers.withMessage(mensajes.minLength, minLength(2))
  },
  lastname: { 
    required: helpers.withMessage(mensajes.required, required),
    alpha: helpers.withMessage(mensajes.alpha, alpha), 
    minLength: helpers.withMessage(mensajes.minLength, minLength(2))
  },
  // etc...
}
```

Y luego los mensajes pueden ser mostrados en el template iterando sobre el array de errores o mostrando uno por vez (el primero del array):

```html
<small>{{ v$.firstname.$errors[0]?.$message }}</small>
```

__10. Submit:__ Antes de enviar el formulario deben chequear que todos los campos cumplan con las validaciones. Para eso deben usar el método asíncrono `$validate`. Este método debe ser accedido mediante la propiedad `value` ya que internamente Vuelidate usa una `ref`:

```js
interface User {
  firstname: string
  lastname: string
  email: string
  password: string
}

// No es necesario importar el metodo defineEmits()
// Tampoco es obligatorio usar TypeScript para declarar los emits
// const emits = defineEmits(['submit-form'])

const emits = defineEmits<{ 'submit-form': [user: User] }>()

async function submitForm() {
  
  const valid = await v$.value.$validate()
  
  if (valid) {

    // Emitir el formulario
    emits('submit-form', {...state})
    
    // Resetear las validaciones 
    v$.value.$reset()

    // Resetear el formulario
    Object.keys(state).forEach(key => state[key] = '')
  } 
}
```

__11. Vuex:__ En este caso estoy usando como modelo el primer desafío del formulario, usando props y emits, no Vuex. Si deciden hacerlo con Vuex recuerden que la versión de Vuex para Vue 3 es __Vuex 4__.


<hr>

Cualquier duda que tengan me pueden escribir por la plataforma o por Discord.

<hr>