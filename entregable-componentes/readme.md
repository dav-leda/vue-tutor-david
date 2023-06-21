# Curso Vue.js 💻️ 🛠️
## Tutor: David Leda

---

Hola 🙋‍♂️️ Aquí les paso algunas recomendaciones para el entregable 2: __Componentes__.

__1.__ Por favor hagan la entrega con __un link al repositorio en GitHub__, no en un zip ni Dropbox 🙏️🙏️🙏️

__2.__ Por favor __no pongan todas las entregas en un mismo repositorio__ (ya sea distintas entregas en distintas carpetas o en distintas ramas del repositorio). Por favor __inicien un repositorio nuevo para cada entrega__ 🙏️🙏️🙏️

__3.__ Antes de hacer la entrega __chequeen si no les está dando algún error por consola.__ Si no pueden solucionar el error no hay problema, pueden entregar igual, pero en ese caso avisen al hacer la entrega: "Me está dando tal error por consola."

__4. Componentes:__ La idea es crear un componente de una tabla y que este componente se repita 3 veces, cada una con un color distinto. Y a su vez que cada una de las tablas tenga al menos 3 filas. Acá les paso un ejemplo de como quedaría (no es necesario que les quede exactamente igual, es sólo para que tengan una idea):

<a href="https://vue-entregable-componentes.netlify.app/" target="_blank">https://vue-entregable-componentes.netlify.app/</a>

__5. Anidación de bucles `v-for`:__ Al haber dos bucles `v-for` anidados (uno para las 3 tablas, otro para los items dentro de cada tabla) van a necesitar __2 niveles de iteración__. En el `v-for` sobre el componente de la tabla (que se puede llamar `<table-component>` o como quieran) para que se repitan la 3 tablas, luego otro `v-for` sobre el elemento `<tr>` del `<tbody>` del componente de la tabla para que se repitan las 3 filas (o pueden ser más de 3).

__6. Array en `data`:__ Al haber dos niveles de iteración, en `data` van a necesitar declarar un array que dentro contenga 3 arrays, cada uno con al menos 3 objetos (para cada una de las filas) y cada objeto con al menos 3 propiedades (para cada una de las columnas). Si son productos las propiedades pueden ser: nombre, precio, categoría, stock, etc. Si son personas: nombre, apellido, mail, edad, etc.

__7. Array de colores:__ Cada una de las 3 tablas tiene que tener un color de fondo (o de letra) distinto. Para eso pueden declarar en `data` un array de 3 strings para los colores de CSS, ya sea como palabras ('blue'), como código rgb o como código hexadecimal ('#000077'). O también pueden incluir la propiedad `color` dentro de cada uno de los arrays de las tablas.

__8. key:__ Recuerden que el `v-for` siempre tiene que incluir un [key](https://es.vuejs.org/v2/guide/list.html#key) para que cada elemento de la iteración tenga una identificación única en el [DOM](https://developer.mozilla.org/es/docs/Glossary/DOM). Lo ideal es que el `key` sea el `id` del item dentro del array (`item.id` o como lo hayan llamado) ya que el `id` por definición es único. Pero si el objeto no tiene una propiedad `id` se puede usar __el índice del array__ como `key`:

{% raw %}
```html
<tr v-for="(product, i) in products" :key="i">
  <td>{{ product.name }}</td>
</tr>
```
{% endraw %}

__9. `for...in`:__ Aunque en JavaScript nativo (también llamado _Vanilla JS_) `for...in` se usa [para iterar sobre propiedades de objetos](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Statements/for...in) mientras que `for...of` se usa [para iterar sobre arrays](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Statements/for...of), en Vue se puede iterar sobre un array de cualquiera de las dos formas, aunque en la documentación de Vue [siempre usan __in__ en vez de __of__](https://es.vuejs.org/v2/guide/list.html#v-for-con-un-Componente) 🤷‍♂️️ Les recomiendo que usen __in__, porque es lo que se suele usar en Vue:

{% raw %}
```html
<tr v-for="product in products" :key="product.id">
  <td>{{ product.name }}</td>
</tr>
```
{% endraw %}

__10. Props:__ Para pasar el array de productos desde el ciclo `v-for` en index.html al componente de la tabla recuerden que tienen que usar __props__. Y para que el componente de la tabla pueda recibir esas props tienen que declararlas en ese componente indicando el tipo de dato (en este caso, Array):

```js
props: {
  products: Array,
  color: String
},
```
Y luego en `index.html` el componente de la tabla recibe las props mediante un `v-bind:nombre-de-la-prop`, abreviado como `:nombre-de-la-prop` (en este caso `:products`):

```html
<table-component v-for="(table, i) in tables" :key="i"
  :products="table"
  :color="colors[i]"
/>
```

__11. Declaración de componentes:__ Recuerden que para poder usar un componente dentro de `index.html` debe estar declarado en `components`, como en este ejemplo:

<a href="https://github.com/dav-leda/vue-cdn-components-props/blob/master/main.js" target="_blank">https://github.com/dav-leda/vue-cdn-components-props/blob/master/main.js</a>



Para declararlo no es necesario usar __kebab-case__:

```js
// app.js
components: {
  'componente-uno': ComponenteUno
}
```
Puede ser declarado usando __PascalCase__:

```js
// app.js
components: {
  ComponenteUno
}
```
Y luego ser usado en el template (o en `index.html`) con __kebab-case__. Vue hace la conversión automática de un formato a otro:

```html
<componente-uno :colors-prop="colorsData"/>
```

__12. Inline HTML:__ Para poder ver más claramente el código HTML dentro del _template_ del componente les recomiendo que instalen la extensión __Inline HTML__ para VS Code:

<a href="https://marketplace.visualstudio.com/items?itemName=pushqrdx.inline-html" target="_blank">https://marketplace.visualstudio.com/items?itemName=pushqrdx.inline-html</a>


Esto es porque al ser un _template literal_ dentro de un archivo .js les va a aparecer todo el HTML del mismo color, con lo cual resulta difícil distinguir los elementos.

Para que se active el resaltador de sintaxis de __Inline HTML__ tienen que poner `/*html*/` antes del backtick:

```js
  template: /*html*/`
    <table>
      <thead>
        <tr>
          <th>Producto</th>
          <th>Precio</th>
          <th>Stock</th>
        </tr>
      </thead>

      // etc...      
`
```

__13. Opcional:__ Según [la documentación de Vue](https://vuejs.org/guide/quick-start.html#using-the-es-module-build) al estar usando componentes la forma más correcta de importar la librería de Vue sería dentro de un [módulo de JavaScript](https://developer.mozilla.org/es/docs/Web/JavaScript/Guide/Modules), no en `index.html`. Esto permite que los componentes sean utilizados como módulos individuales (o sea, archivos .js que tienen su propio [scope](https://developer.mozilla.org/es/docs/Glossary/Scope)) y puedan ser exportados e importados de un componente a otro. __No es obligatorio que lo hagan así__ pero sería bueno que lo intenten porque de esta forma el paso de Vue CDN a [Vue SFC](https://es.vuejs.org/v2/guide/single-file-components.html) les va a resultar más fácil ya que Vue SFC funciona con [imports](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Statements/import) y [exports](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Statements/export). Vue SFC (o sea, Vue con Single File Components) es lo que en las diapos figura (erróneamente 🤦‍♂️️) como [Vue CLI](https://cli.vuejs.org/).

Acá les paso un ejemplo de cómo sería la importación de la librería de Vue dentro de un módulo:

<a href="https://github.com/dav-leda/vue3-cdn-componentes/blob/master/main.js" target="_blank">https://github.com/dav-leda/vue3-cdn-componentes/blob/master/main.js</a>

Como ven, cada componente está en un archivo `.js` aparte.

Y acá les paso como ejemplo el archivo `main.js` de un proyecto en Vue SFC (este archivo es creado automáticamente al iniciar un proyecto de Vue con `npm init vue@3` o `pnpm create vue@3`). Como ven, es muy parecido:

<a href="https://github.com/dav-leda/vue3-counter-store/blob/master/src/main.js" target="_blank">https://github.com/dav-leda/vue3-counter-store/blob/master/src/main.js</a>


Les paso también este sitio con la explicación sobre el uso de módulos en Vue CDN:

<a href="https://vue-cdn-props-router.netlify.app/" target="_blank">https://vue-cdn-props-router.netlify.app/</a>


__14. Opcional:__ Aunque la consigna no lo pide sería bueno que usen `fetch` para obtener la información de las tablas en lugar de declararla en `data`. Esto es porque este tipo de información (productos de un e-commerce, usuarios de una red social, etc) __siempre proviene de una [fuente externa](https://frontendlab.vercel.app/vue/fetch-en-vue/#usando-fetch-en-un-e-commerce)__, en general de una [API REST](https://rockcontent.com/es/blog/api-rest/), __nunca está *hardcodeada* en el frontend__.

Para poder tener la información de las tablas en una API REST lo más simple es que usen [MockApi](https://mockapi.io/).

Otra forma más simple aún es <a href="https://frontendlab.vercel.app/vue/fetch-en-vue/#simulando-una-api-rest" target="_blank">simular una API REST con GitHub Pages</a>.

Y así pueden acceder al archivo .json servido desde GitHub:

<a href="https://dav-leda.github.io/api/tables/" target="_blank">https://dav-leda.github.io/api/tables/</a>


Y para usar `fetch` dentro de un _method_ pueden hacerlo así:

```js
data: () => ({
  products: []
}),

created() {
  // La URL de la API REST que estén usando:
  this.getData(url)
},

methods: {
  async getData(url) {
    try {
      const res = await fetch(url)
      this.products = await res.json() 
    } catch(error) {
      console.log(error)
    }
  }
}
```
<hr>

Cualquier duda que tengan me pueden escribir, preferentemente por Discord ya que la plataforma de CH no siempre funciona bien 🤷‍♂️️