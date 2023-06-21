# Curso Vue.js 💻️ 🛠️
## Tutor: David Leda
---


Hola 🙋‍♂️️ Acá les paso algunas recomendaciones para entregable 4: __Formulario__


__1. GitHub:__ Por favor hagan la entrega con __un link al repositorio en GitHub__, no en un zip ni Dropbox 🙏️🙏️🙏️

__2. Una entrega por repo:__ Por favor __no pongan todas las entregas en un mismo repositorio__ (ya sea distintas entregas en distintas carpetas o en distintas ramas del repositorio). Por favor __inicien un repositorio nuevo para cada entrega__.

__3. Errores en consola:__ Antes de hacer la entrega __chequeen si no les está dando algún error por consola.__ Si no pueden solucionar el error no hay problema, pueden entregar igual, pero en ese caso avisen al hacer la entrega: "Me está dando tal error por consola."

__4. CSS: No es obligatorio usar Bootstrap__. Pueden usar CSS nativo, o SASS, o algún otro framework de CSS como Tailwind o Bulma.

Si tienen un archivo `.css` global lo pueden poner dentro del directorio `assets` en una carpeta que se llame `css`.

__5. Formulario:__ El formulario debe contener __al menos 4 inputs__ y __al menos 4 tipos distintos de input__ (`text`, `number`, `email`, `password`, `radio`, `checkbox`, etc). No es necesario que todos los inputs tengan validaciones, pero __al menos 4 de los inputs deben tener validaciones__ (al menos 2 validaciones por input, una para _required_ y alguna otra más).

Acá les paso un ejemplo de cómo debería quedar. No es obligatorio que les quede exactamente igual, ni que los inputs sean los mismos, se los paso únicamente como guía:

<a href="https://curso-vue-formulario.vercel.app/" target="_blank">https://curso-vue-formulario.vercel.app/</a>

__6. JSON:__ No es necesario usar un _objeto JSON_, pueden crear un objeto dentro de _data_ que a su vez contenga el valor de cada uno de los campos del formulario:

```js
data: () => ({
  formData : {
    nombre: '',
    apellido: '',
    edad: '',
    email: '',
    cursos: []
  },
  formState: {}
})
```
Cada una de estas propiedades dentro de `formData` debe estar conectada con cada uno de los inputs usando `v-model`. 

Luego, cuando el formulario es enviado, pueden enviar `formData` a App.vue mediante un `emit`, y de App.vue a __TableComponent__ mediante `props`:

```html
<template>
  <main>

    <FormularioVue @emit-form="addUser"/>
    <TableComponent :users="users"/>

  </main>
</template>

<script>

import TableComponent from '@/components/TableComponent.vue'
import FormularioVue from '@/components/FormularioVue.vue'

export default {

  name: 'App',

  components: {
    TableComponent, FormularioVue
  },

  data: () => ({
    users: [],  
  }),

  methods: {
    addUser(user) {    
      this.users.push(user)
    }
  }
}
</script>
```

__7. Filtros de vista:__ Recuerden que si están usando Vue 3 los _filters_ no les van a funcionar, ya que [fueron deprecados](https://v3-migration.vuejs.org/breaking-changes/filters.html#filters). En Vue 3 pueden ser reemplazados por `methods`, que funcionan igual:

```js
filters: {
  capitalize(string) {
    return string.replace(/\b\w/g, first => first.toUpperCase())
  }
},
methods: {
  capitalize(string) {
    return string.replace(/\b\w/g, first => first.toUpperCase())
  }
}
```

Y en el template:

```html
<tbody>
  <tr v-for="(user, i) in users" :key="i">
    <!-- Usando filters -->
    {% raw %}
    <td>{{ user.nombre | capitalize }}</td>
    
    <!-- Usando methods -->
    <td>{{ capitalize(user.nombre) }}</td>
    {% endraw %}
  </tr>
</tbody>
```

__8. Reseteo del formulario:__ Tal como dice la consigna, luego de enviado el formulario los campos se deben vaciar para permitir volver a cargar datos. Van a notar que si envían los datos del formulario en forma directa __al resetear el formulario los datos también desaparecen en el componente de la tabla__.

Por ejemplo, si lo hacen así:

```js
methods: {
  submitForm() {
    if (this.formState.$invalid) {
      return
    } else {
      // Emitir los datos del formulario
      // desde el componente del formulario a App.vue
      this.$emit('emit-form', this.formData)
  
      // Resetear las validaciones
      this.formState._reset()
      // Resetear los campos del formulario
      this.resetFormData()
    }
  },
  resetFormData() {
    Object.keys(this.formData).forEach(key => this.formData[key] = '')
    this.cursos = []
  }
}
```
Esto es porque el sistema de reactividad de Vue funciona por referencias a objetos. Esto quiere decir que al hacer un `push` de `formData` dentro del array de usuarios lo que estamos pusheando __no es el objeto sino una referencia__. Entonces, al vaciar las propiedades de `formData` __no sólo las estamos vaciando en el componente del formulario sino también en el array de usuarios__, ya que [ambas referencias apuntan al mismo objeto](https://frontendlab.vercel.app/vue/formulario/#array-push-spread-operator).

Para evitar que esto ocurra __deben clonar el objeto__. Esto se puede hacer de distintas formas, la más simple es con un [spread operator](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Operators/Spread_syntax#spread_en_literales_tipo_objeto):

```js
this.$emit('emit-form', { ...this.formData })
```

Pero recuerden que la clonación de objetos con el _spread operator_ sólo funciona con objetos simples (con un solo nivel de anidación) pero [no en objetos complejos](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax#spread_in_array_literals). Para clonar objetos complejos deben usar el método nativo de JavaScript [structuredClone](https://developer.mozilla.org/en-US/docs/Web/API/structuredClone).

__9. VueForm:__ Si quieren pueden usar la librería [VueForm](https://github.com/fergaldoyle/vue-form) tal como se vio en clase, pero recuerden que __VueForm sólo funciona con Vue 2, no con Vue 3__. Para Vue 3 pueden usar [FormKit](https://formkit.com/) o [Vuelidate](https://vuelidate-next.netlify.app/).

La documentación de VueForm no es muy clara que digamos. Les paso esta guía de lo que significa cada uno de los estados del formulario en __VueForm__:

`formState.$submitted`: el formulario fue enviado
<br>
`formState.$invalid`: el formulario contiene errores
<br>
`formState.$touched`: el campo fue focalizado
<br>
`formState.$dirty`: el usuario ingresó algo en el campo
<br>
`formState.$error`: objeto con los errores que contiene el campo
<br>
<br>
Y luego, al usarlas como condicionales de los mensajes de validación:

```html
<!-- Mostrar el mensaje de error si el formulario fue enviado ($submitted)
y el usuario ingresó algo ($dirty) pero lo que ingresó es incorrecto -->

<div
  v-if="formState.$submitted && formState.$dirty" 
  slot="minlength" 
  class="alert alert-danger"
>Este campo requiere como mínimo 2 caracteres.</div>
```

Tal como indica la documentación de __VueForm__, si el campo contiene distintos mensajes de error (_scoped messages_) debe usarse `v-if`, no `show`:

<a href="https://www.npmjs.com/package/vue-form#displaying-messages" target="_blank">https://www.npmjs.com/package/vue-form#displaying-messages</a>



__10. Formulario sin librerías:__ Si prefieren no usar librerías [acá les paso un tutorial sobre cómo hacerlo](https://frontendlab.vercel.app/vue/formulario/). __No es obligatorio que lo hagan tal cual como dice ahí__, se los paso únicamente por si les sirve como guía.


<hr>

Cualquier duda que tengan me pueden escribir, preferentemente por Discord ya que la plataforma de CH no siempre funciona bien 🤷‍♂️️

<hr>
