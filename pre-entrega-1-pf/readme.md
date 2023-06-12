# Curso Vue.js 💻️ 🛠️
## Tutor: David Leda
### Primera Pre-entrega del Proyecto Final
---

Hola 🙋‍♂️️ Acá les paso algunas recomendaciones para la __Pre-entrega 1 del Proyecto Final__:

__1. GitHub:__ Por favor hagan la entrega con __un link al repositorio en GitHub__, no en un zip ni Dropbox 🙏️🙏️🙏️

__2. Una entrega por repo:__ Por favor __no pongan todas las entregas en un mismo repositorio__ (ya sea distintas entregas en distintas carpetas o en distintas ramas del repositorio). Por favor __inicien un repositorio nuevo para cada entrega__ 🙏️🙏️🙏️

__3. Errores en consola:__ Antes de hacer la entrega __chequeen si no les está dando algún error por consola.__ Si no pueden solucionar el error no hay problema, pueden entregar igual, pero en ese caso avisen al hacer la entrega: "Me está dando tal error por consola."

__4. CSS: No es obligatorio usar Bootstrap__. Pueden usar CSS nativo, o SASS, o algún otro framework de CSS como Tailwind o Bulma.

Si tienen un archivo `.css` global lo pueden poner dentro del directorio `assets` en una carpeta que se llame `css`.

__5. Vite:__ Les recomiendo que usen [Vite](https://frontendlab.vercel.app/vue/vite/) para crear los proyectos, no Vue CLI, tal como indica [la documentación oficial de Vue](https://vuejs.org/guide/scaling-up/tooling.html#vue-cli).

Si entran a la página de Vue CLI van a ver un cartel que dice que [Vue CLI se encuentra en modo mantenimiento](https://cli.vuejs.org/#getting-started) y fue reemplazado por Vite:

```
Vue CLI is in Maintenance Mode

For new projects, please use create-vue to scaffold Vite-based projects.
```
Tal como indica [la documentación de Vue](https://vuejs.org/guide/quick-start.html#creating-a-vue-application) para crear un proyecto con __Vite + Vue__, dentro del directorio donde tengan sus proyectos de Vue, ejecutan este comando:

```sh
npm init vue@latest
```

__6. Volar (extensión para VS Code):__ Al crear el proyecto con Vite es posible que VS Code les muestre un cartel sugiriendo que instalen la extensión __Volar__ para Vue 3. Les recomiendo que la instalen, pero antes chequeen si no tienen instalada __Vetur__ (la extensión para Vue 2). En ese caso desinstalen Vetur ya que no es conveniente que estén las dos activas al mismo tiempo.

Puede ser que Volar les subraye el tag `script` como si estuviese mal. Esto es porque Volar está pre-configurada para usar la [Composition API](https://frontendlab.vercel.app/vue/vue2-vue3/#options-api-vs-composition-api) de Vue 3 con el tag `<script setup>`, o con TypeScript, con el tag `<script lang="ts">`. Pueden ignorarlo (no afecta en nada) o pueden incluir `lang="ts"` para que desaparezca ese subrayado.

__7. PNPM:__ Les recomiendo que en lugar de `npm` usen [pnpm](https://pnpm.io/) que funciona mucho más rápido. Esto es porque `pnpm` reutiliza las dependencias que ya tienen instaladas, lo cual es muy práctico si están creando varios proyectos con las mismas dependencias, como vamos a hacer en este curso. En cambio `npm` vuelve a instalar todo de cero para cada proyecto (ocupando más espacio en el disco duro) y funciona mucho más lento. Para poder usar `pnpm` primero deben instalarlo en forma global (por única vez):

```sh
npm install -g pnpm
```

Y luego, para crear un proyecto __Vite + Vue__ con `pnpm`:

```sh
pnpm create vue@3
```

__8. Estructura general del proyecto:__ Como dice la consigna, el proyecto debe incluir __Login__ y __Signup__ (registro de usuarios), __listado de productos__ y __carrito de compras__. El listado de productos puede ser en una tabla, pero sería preferible que lo hagan en forma de __cards__, como en el ejemplo que está al final de la clase 4 (__Modificar un componente: Codermeals__).

Acá les paso un ejemplo como para que se hagan una idea de la estructura general de esta entrega. __No es necesario que el diseño sea igual, sólo la funcionalidad__:

<a href="https://vue-bakery-1.vercel.app/" target="_blank">https://vue-bakery-1.vercel.app/</a>

Tampoco es necesario que usen íconos para los botones, __pueden usar botones de texto__.

En cuanto al carrito, __no es necesario que se muestre en una *sidebar*__, pueden usar una [ventana modal](https://frontendlab.vercel.app/vue/modal-en-vue/) o mostrarlo en una nueva vista simulando un router.

Los componentes de __Login__ y __Signup__ también pueden ser mostrados usando una modal.

Para crear el componente de la ventana modal pueden instalar alguan librería como [Vue Final Modal](https://vue-final-modal.org/) o crearla ustedes mismos siguiendo [estas instrucciones](https://frontendlab.vercel.app/vue/modal-en-vue/).

Si en lugar de la ventana modal prefieren hacerlo simulando un router, les paso este ejemplo que explica cómo hacerlo (en la sección __Props__):

<a href="https://vue-cdn-props-router.netlify.app/" target="_blank">https://vue-cdn-props-router.netlify.app/</a>

__9. Cards de productos:__ Cada card debe incluir el __nombre__ del producto, el __precio__, una __imagen__ del producto y un __botón__ para agregar al carrito. Opcionalmente, también puede incluir botones de ➕️ y ➖️ para modificar el número de unidades de ese producto en el carrito, pero esto también puede ser manejado directamente desde el componente del carrito. 

__10. Información de productos:__ Como dice la consigna, la data de los productos puede provenir de archivos en formato __JSON__ o de propiedades creadas dentro de los componentes con información de prueba. Les recomiendo que lo hagan en formato __JSON__, preferentemente __de una fuente externa__, por ejemplo, [simulando una API REST con GitHub Pages](https://frontendlab.vercel.app/vue/fetch-en-vue/#simulando-una-api-rest), y luego pueden acceder a esta data usando `fetch`. 

Esto es porque para la segunda pre-entrega hay que usar una fuente externa ([MockApi](https://mockapi.io/)) y sería bueno que se vayan acostumbrando a hacerlo de esta forma para que luego el cambio no les resulte tan complicado. Y además, [hay otras razones](https://frontendlab.vercel.app/vue/fetch-en-vue/#usando-fetch-en-un-e-commerce) por las que no es aconsejable que la información de productos esté *hardcodeada* en el frontend. 

Para acceder al archivo JSON con `fetch` pueden hacerlo así:

```js
// src/App.vue

import NavBar from '@/components/NavBar.vue'
import ProductCard from '@/components/ProductCard.vue'

const url = 'https://dav-leda.github.io/api/products'

export default {

  components: {
    NavBar, ProductCard
  },

  data: () => ({
    products: []
  }),
  // Invocar el método getData(url) al crearse el componente
  created() {
    this.getData(url)
  },
  
  methods: {
    async getData(url) {
      try {
        const res = await fetch(url)
        this.products = await res.json() 
      } catch (error) {
        console.log(error)
      }
    }
  }
}
```
O si quieren, también pueden usar [Axios](https://axios-http.com/). En ese caso, deben primero instalar la librería (con `npm i axios`, o sino `pnpm i axios`) e importarla en el componente (con `import axios from 'axios'`).

```js
methods: {
  async getData(url) {
    try {
      const { data } = await axios.get(url)
      this.products = data
    } catch (error) {
      console.log(error)
    }
  }
}
```
La consigna dice que eviten usar plataformas remotas online para suministrar los datos remotos. En realidad, sería preferible que sí usen una fuente remota, pero como la consigna dice que no, no es obligatorio que lo hagan 🤷‍♂️️

__11. Componente de la card:__ Una vez obtenida la información de los productos la pueden enviar desde App.vue al componente de la card mediante `props`:

```html
<ProductCard
  v-for="product in products" :key="product.id"
  :product="product"
/>
```
Y luego, en el componente __ProductCard__ reciben esta información en las `props`:

```js
props: {
  product: {
    id: Number,
    title: String,
    price: Number,
    image: String
  }
},
```
__12. Nombres de los componentes:__ A diferencia de Vue CDN, en [Vue SFC](https://es.vuejs.org/v2/guide/single-file-components.html) (lo que en las diapos es llamado, erróneamente, Vue CLI 🤦‍♂️️) la convención es [nombrar a los componentes usando PascalCase](https://vuejs.org/guide/components/registration.html#component-name-casing), no kebab-case. Pero si les resulta más claro usando kebab-case, lo pueden hacer también.

__13. Imágenes de productos:__ Las imágenes de los productos también deberían provenir de una fuente externa. Poner las imágenes de los productos en el directorio `/assets` [les puede traer muchos problemas](https://frontendlab.vercel.app/vue/fetch-en-vue/#usando-imagenes-en-un-e-commerce) como, por ejemplo, que el path relativo de la imagen deje de funcionar al pasarlo de un componente a otro mediante `props`. El directorio `/assets` debe ser usado únicamente para las imágenes que no cambian (logos, banners, imágenes de fondo), no para las que cambian (productos, avatar de los usuarios, etc).


__14. Carrito:__ Cada vez que el usuario clickea en el botón de agregar al carrito el objeto con la información del producto debería agregarse al array del carrito. El problema es que el componente de la card (ProductCard) y el componente del carrito (CartTable) no tienen relación directa, y [estar pasando esta información entre __componentes no emparentados__ mediante props y emits](https://frontendlab.vercel.app/vue/props-y-emits/#comunicacion-entre-componentes-no-emparentados) es muy complicado y propenso a errores.

Por lo tanto, __la información del carrito debería ser accesible globalmente, no ser pasada de componente a componente__. La forma usual de hacer esto en Vue es con alguna herramienta de administración de estado global como [Vuex](https://vuex.vuejs.org/) o [Pinia](https://pinia.vuejs.org/). Vuex lo vamos a ver recién al final del curso, así que por ahora pueden usar una [store](https://frontendlab.vercel.app/vue/provide-mixins-stores/#stores) simple:

```js
// src/stores/cartStore.js

export const cartStore = {

  cart: [],
  
  cartTotal() {
    return this.cart.reduce((total, item) => total + item.qty, 0)
  },

  cartTotalPrice() {
    return this.cart.reduce((total, item) => total + item.subtotal, 0)
  },
  
  addToCart(product) {
    this.cart.push({
      ...product,
      qty: 1, 
      subtotal: product.price
    })    
  },
}
```

Acá pueden ver [instrucciones más detalladas](https://frontendlab.vercel.app/vue/carrito/) sobre cómo usar una _store_ para el carrito. __No es obligatorio que lo hagan de esta forma__, se los paso únicamente por si les sirve como guía.

__15. Login:__ Les paso también [estas instrucciones](https://frontendlab.vercel.app/vue/simulando-un-login/) para crear el componente de Login. Nuevamente, __no es obligatorio que lo hagan como dice ahí__, se los paso únicamente por si les sirve.

__16. Signup (registro de usuarios):__ Y les paso este otro [tutorial sobre cómo hacer el componente de Signup](https://frontendlab.vercel.app/vue/simulando-un-signup/). Tampoco es obligatorio que lo hagan tal cual como dice ahí.

<hr>

Cualquier duda que tengan me pueden escribir, preferentemente por Discord ya que la plataforma de CH no siempre funciona bien 🤷‍♂️️

<hr>









