# Curso Vue.js 💻️ 🛠️
## Tutor: David Leda
### Segunda Pre-entrega del Proyecto Final
---

Hola 🙋‍♂️️ Acá les paso algunas recomendaciones para la __Pre-entrega 2 del Proyecto Final__:

__1. GitHub:__ Por favor hagan la entrega con __un link al repositorio en GitHub__, no en un zip ni Dropbox 🙏️🙏️🙏️

__2. Errores en consola:__ Antes de hacer la entrega __por favor chequeen si no les está dando algún error por consola__ 🙏️🙏️🙏️

Si no pueden solucionar el error no hay problema, pueden entregar igual, pero en ese caso avisen al hacer la entrega: "Me está dando tal error por consola."

__3. Vue 2 / Vue 3:__ Recuerden que si están usando la __versión 3__ de Vue __BootstrapVue__, __VueForm__ y __Vue Router 3__ no les van a funcionar, ya que sólo funcionan con Vue 2. Si quieren hacer el proyecto con Vue 3 (lo que sería lo más recomendable, ya que Vue 2 será _deprecado_ a fines de este año) pueden usar CSS nativo, o Tailwind, o [Element Plus](https://element-plus.org/es-ES/), o [Vuestic](https://ui.vuestic.dev/), o [PrimeVue](https://primevue.org/installation), o [Vuetify](https://vuetifyjs.com/en/). Y para el formulario de Signup pueden usar [FormKit](https://formkit.com/) o [Vuelidate](https://vuelidate-next.netlify.app/). Y usar __Vue Router 4__ en lugar de Vue Router 3.

Si de todas formas quieren usar BootstrapVue traten de hacerlo con [tree shaking](https://bootstrap-vue.org/docs/#tree-shaking-with-module-bundlers), de lo contrario se van a instalar los 80 componentes que __pesan más de 1MB__ 😬️ 

__4. Vue Router:__ Aunque lo más lógico sería que __Vue Router 3__ sea la versión del router para __Vue 3__, no es así 😒️ El router para __Vue 3__ es [Vue Router 4](https://router.vuejs.org/guide/migration/), mientras que el router para __Vue 2__ es [Vue Router 3](https://v3.router.vuejs.org/guide/)... Cosas raras que tiene Vue 🤷‍♂️️ 

Las versiones 3 y 4 de Vue Router son bastante parecidas, pero tengan en cuenta que al usar __Vue Router 4__ (con Vue 3) el archivo `main.js` debería quedarles así:

```js
import { createApp } from 'vue'
import App from './App.vue'

import router from './router'

import './assets/css/main.css'

createApp(App)
  .use(router)
  .mount('#app')
```

Y el archivo `index.js` dentro del directorio `/router` debería ser algo así:

```js
// src/router/index.js

import { createRouter, createWebHistory } from 'vue-router'
import HomeView from '../views/HomeView.vue'

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),

  routes: [
    {
      path: '/',
      name: 'home',
      component: HomeView
    }
  ]
})
```
Y luego en el array `routes` le van agregando las rutas que quieran, siempre recordando que antes deben importar la `view` que van a usar arriba de todo (las `views` son lo mismo que en un componente de Vue, pero se usan como _vistas_ de Vue Router).

Y en __App.vue__ incluyen __RouterView__ que es donde se van a renderizar las distintas vistas:

```html
<template>

  <nav>
    <RouterLink to="/">Home</RouterLink>
    <RouterLink to="/login">Login</RouterLink>
    <RouterLink to="/cart">Carrito</RouterLink>
  </nav>

  <RouterView/>

</template>

<script>
// No es necesario declararlos dentro del objeto components
// Con importarlos es suficiente: 
import { RouterLink, RouterView } from 'vue-router'
</script>
```
__No es obligatorio que los *RouterLink* y la *RouterView* estén dentro del mismo componente__, pueden tener los __RouterLink__ en otro componente (por ejemplo, en __NavBar__) o también es posible __no usar *RouterLink*__ y manejar el ruteo con eventos `@click` en botones:

```html
<button @click="$router.push('/login')">Login</button>
```
O con _methods_:

```html
<a href="" @click.prevent="goToProductDetail">
  Ver detalle del producto
</a>
```
Y en el script:

```js
methods: {
  goToProductDetail() {
    this.$router.push({ 
      name: 'product', 
      params: { id: this.product.id } 
    })
  }
}
```

__5. RouterView__: Si en la primera entrega pusieron toda la lógica del carrito y del Login en App.vue, comunicando los componentes con _props_ y _emits_ ahora van a tener un problema porque al usar Vue Router __los componentes no están emparentados en forma directa con App.vue__ sino que pasan a través de __RouterView__, que es donde se renderizan las vistas.

Una solución podría ser poner todos los _props_ y _emits_ en __RouterView__:

```html
<RouterView
  :products="products"
  :cart="cart"
  :user="user"
  @add-to-cart="addToCart"
  @remove-from-cart="removeFromCart"
  @update-qty-in-cart="updateQtyInCart"
  @login-user="loginUser"
  @logout-user="logoutUser"
  @signup-user="signupUser"
/>
```
Pero a eso habría que sumar la lógica de la vista de Admin para modificar los productos, que es parte de esta entrega:

```html
<RouterView
  :products="products"
  :product-id="productId"
  :cart="cart"
  :user="user"
  @add-to-cart="addToCart"
  @remove-from-cart="removeFromCart"
  @update-qty-in-cart="updateQtyInCart"
  @login-user="loginUser"
  @logout-user="logoutUser"
  @signup-user="signupUser"
  @add-product="addProduct"
  @remove-product="removeProduct"
  @update-product="updateProduct"
/>
```
Funciona, pero es muy propenso a errores, porque __RouterView__ es un componente genérico que solamente renderiza las vistas que le pasa Vue Router.

La solución sería usar una herramienta de administración de estado global como [Vuex](https://vuex.vuejs.org/) o [Pinia](https://pinia.vuejs.org/). El problema es que Vuex lo vamos a ver recién en las clases 11 y 12. Mientras tanto, la solución más fácil es que usen objetos de JavaScript como [stores](https://frontendlab.vercel.app/vue/carrito/#store-para-el-carrito) en un archivo `.js` externo a los componentes que luego sea importado en cada componente que lo necesite.

Además, la estructura de las _stores_ es muy similar a la de Vuex (y más similar aún a la de Pinia), con lo cual el paso de _stores_ a Vuex les va a resultar más fácil.

__6. Axios:__ La consigna dice que deben **integrar Axios**. Para eso recuerden que primero deben instalarlo (`npm i axios`).

El problema con Axios es su peso (29.5Kb). Teniendo en cuenta que todo Vue pesa unos 52Kb, sumar 29Kb sólo para ahorrarse un par de líneas de código en la petición HTTP no tiene mucho sentido, pero bueno, si quieren úsenlo 🤷‍♂️️

Una opción mejor es que usen el método nativo `fetch`. 

Y otra opción aún mejor es que usen la librería [dedalo-ax](https://www.npmjs.com/package/dedalo-ax) 🙂️ Funciona igual que Axios (en su funcionalidad básica, o sea, para hacer peticiones __get, post, put y delete__) pero __pesa solamente 778 bytes__ y fue creada por quien les habla 🙋‍♂️️

```sh
npm install dedalo-ax
```
Y en el componente:

```js
const { VITE_API_URL: baseUrl } = import.meta.env
const endpoint = baseUrl + '/products'

import ax from 'dedalo-ax'

export default {

  data: () => ({ 
    products: []
  }),

  async created() {
    this.products = await ax.get(endpoint)
  }
}
```

Y para hacer un **POST**:

```js
import ax from 'dedalo-ax'

const { VITE_API_URL: baseUrl } = import.meta.env
const endpoint = baseUrl + '/products'

const someProduct = {
  name: 'Chocotorta',
  description: 'Con chocolinas.',
  price: 1000,
  stock: 1,
  imgsrc: 'https://dav-leda.github.io/images-bakery/chocotorta.jpg'
}

export default {
  methods: {
    async addNewProduct() {
      const res = await ax.post(endpoint, someProduct)
      console.log(res)
    }
  }
}
```


__7. MockAPI:__ La consigna dice que deben **consumir los recursos desde el backend en MockAPI.** Para crear una cuenta en [MockAPI](https://mockapi.io)  pueden seguir [estas instrucciones](https://frontendlab.vercel.app/vue/simulando-un-login/#mockapi). En MockAPI deben __crear 2 recursos: uno para productos y otro para usuarios.__ 

Es decir que el JSON con el listado de productos y el del listado de usuarios deben estar en MockAPI, __no *hardcodeados* dentro de los componentes en *data*__:

![mockapi](https://dav-leda.github.io/api/images/mockapi-relations.png)

Hasta el año pasado el plan gratuito de MockAPI permitía generar hasta 3 _resources_ e incluso vincularlos, como se ve en la imagen (por ejemplo vincular el recurso de usuarios con el de pedidos para que automáticamente cada pedido quede asociado a un usuario en particular). Pero desde hace unos meses limitaron su plan gratuito a 2 _resources_ 😐️ con lo cual, para poder asociar cada pedido a un usuario hay que crear un array de pedidos dentro de cada objeto de usuario 😒️

Tengan en cuenta que MockAPI genera un nuevo _id_ automáticamente cada vez que agregamos un nuevo documento al recurso. Aunque el _id_ que genera MockAPI es un número, __lo guarda como un String, no como un Number__. Por lo cual, si en la primera pre-entrega __tenían la propiedad *id* con tipo Number, deben cambiarla a String:__

```js
props: {
  product: {
    id: String,
    name: String,
    price: Number,
    stock: Number,
    imgsrc: String
  }
}
```

__8. Variables de entorno:__ En cuanto a la URL de MockAPI, recuerden que MockAPI genera una URL única que contiene el token de su cuenta personal:

https://numero-de-token.mockapi.io/api/

Sería bueno que guarden esta URL en una [variable de entorno](https://frontendlab.vercel.app/vue/simulando-un-login/#variables-de-entorno) de lo contrario quedaría expuesta en el repositorio y cualquiera puede usar su cuenta de MockAPI sin necesidad de crear una propia (hay bots que recorren automáticamente todos los repositorios de GitHub buscando este tipo de información). 

Para usar variables de entorno pueden seguir las instrucciones que están [acá](https://frontendlab.vercel.app/vue/simulando-un-login/#variables-de-entorno).

__9. Estructura general del proyecto:__ Como dice la consigna, el proyecto debe incluir __Login__ y __Signup__ (registro de usuarios), __listado de productos__, __carrito de compras, listado de pedidos y ABM de productos__. 

Sería bueno que agrupen los componentes en distintas carpetas, según su función: los del carrito en una carpeta, los del usuario en otra, los del admin en otra. Y si usan íconos SVG, crear un componente para cada uno y ponerlos en una carpeta llamada _icons_.

Y lo mismo para las vistas (views): las de admin en una, las de usuario (cliente) en otra.

Y también para los archivos .js con la lógica: la lógica del `fetch` en un archivo .js dentro de una carpeta llamada _services_ (esa es la convención para nombrar a los servicios de acceso a una API). Y para la lógica de las funciones reutilizables (o sea, para formatear precios, formatear fechas, guardar en _localStorage_) la convención es que la carpeta se llame _utils_ (o _helpers_).

Al final de esta página pueden ver un ejemplo de cómo podría quedar la estructura de archivos.



__10. ABM de productos__: La consigna dice:

__*Crear un recurso en el backend para listar productos o servicios, incorporando los métodos (GET, POST, PUT, DELETE).*__

__*Tendrás que crear un usuario Admin (ABM de productos e Información general).*__

Esta parte de la consigna es bastante confusa, pero si ven el ejemplo de proyecto final que está en la página 15 de la clase 1, a lo que se están refiriendo con esto es a un [CMS](https://es.wikipedia.org/wiki/Sistema_de_gesti%C3%B3n_de_contenidos) en forma de [CRUD](https://es.wikipedia.org/wiki/CRUD).

__ABM__ quiere decir __Alta, Baja, Modificación__, que es otra forma de decir __CRUD__. La idea es que el usuario _admin_ pueda crear, modificar o borrar productos y estos cambios deben guardarse en el backend de MockAPI usando POST, PUT y DELETE.

Para probar el ejemplo de Coder House que está en la página 15 de la clase 1 el usuario es __admin@admin__ y el password __123__

También pueden probar entrando como _admin_ en este otro ejemplo (usuario: __admin1__, password: __test123__):

<a href="https://vue-bakery-2.vercel.app/" target="_blank">https://vue-bakery-2.vercel.app/</a>

Luego de hacer Login como admin van a ver que en el dropdown del usuario aparecen las opciones: Mi Perfil, Pedidos, __Productos__ y Logout. Si entran a __Productos__ van a ver el __CMS de productos__ con la opción de agregar nuevos productos, editar algún dato del producto, o borrar el producto.

*Si quieren probar la opción de borrar productos por favor haganlo con alguno que hayan creado ustedes, por favor no borren los productos listados, así no tengo que volver a cargarlos.* 🙏️ 🙏️ 🙏️ 

Para la vista de agregar un nuevo producto y de actualizar un producto sería bueno que __reutilicen el mismo componente__ cambiando únicamente el _id_ de la ruta:

En Vue Router:

```js
{
  path: '/admin/product/:id',
  name: 'add-update-product',
  component: AddOrUpdateProduct
},
```

Y en _AdminView.vue_:

```js
goToAddProduct() {
  this.$router.push({ 
    name: 'add-update-product', 
    params: { id: 'new-product' } 
  })
},

goToEditProduct(id) {
  this.$router.push({ 
    name: 'add-update-product', 
    params: { id } 
  })
},
```
Y luego, en el componente __AddOrUpdateProduct.vue__, si el _id_ de la ruta es `new-product` entonces `this.product` puede ser un objeto vacío o un _placeholder_ con datos iniciales. Y si no es `new-product`, hacer un `fetch` a MockAPI para buscar los datos del producto a ser modificado haciendo uso del _id_ de la ruta:

```js
const { VITE_API_URL: baseUrl } = import.meta.env;

const placeholderProduct = {
  name: 'Chocotorta',
  description: 'Con chocolinas.',
  price: 1000,
  stock: 1,
  imgsrc: 'https://dav-leda.github.io/images-bakery/chocotorta.jpg'
}

import ax from 'dedalo-ax'

export default {

  data: () => ({
    this.product: null
  }),

  async created() {

    const id = this.$route.params.id;
    const endpoint = `${baseUrl}/products/${id}`;

    if (id === 'new-product') {
      this.product = placeholderProduct
    } else {
      this.product = await ax.get(endpoint)
    }
  }
}
```

__11. Pedidos:__ La consigna dice:

__*Crear un último recurso que será el carrito, integrando GET y POST para realizar y revisar pedidos.*__

Esto también es bastante confuso...😕️ Lo que están pidiendo es que el usuario _admin_ pueda ver un listado de __todos los pedidos realizados por todos los usuarios__. Si se fijan en el ejemplo de la página 15 de la clase 1, la página de Admin tiene una sección llamada __Pedidos__ donde se muestran todos los pedidos. Aunque este es un ejemplo de proyecto final completo (con Vuex), la 2da pre-entrega incluye la sección de pedidos, porque la 3era pre-entrega consiste únicamente en sumar Vuex al proyecto, pero sin agregar más funcionalidades.

El problema es que la consigna dice _crear un recurso que será el carrito_, pero en realidad __no es un recurso para el carrito, es para pedidos__, lo cual tiene más sentido que guardar el carrito en el backend, ya que la información del carrito es efímera (se borra cuando el usuario completa la compra) mientras que la información de pedidos debería ser guardada en algún lado. 

Hay algunos e-commerce (Mercado Libre) que también guardan la información del carrito en el backend y luego la borran cuando el pedido fue realizado, y hay otros que la guardan en _localStorage_ (y también, luego de completado el pedido, la borran). En este caso, no es necesario que guarden el carrito pero sí los pedidos.

El problema es que __el plan gratuito de MockAPI sólo permite crear 2 recursos, y ya usamos uno para usuarios y otro para productos__ 😬️

La solución es que dentro del recurso de usuarios, en el objeto de cada usuario haya __un array de pedidos (orders)__ (al final de esta página pueden ver un ejemplo de cómo les quedaría el JSON de usuarios en MockAPI con este array de pedidos).

Este array de pedidos debería tener 3 propiedades: __timestamp__ (el día y hora en que fue realizado el pedido), __total__ (el costo total del pedido) y __products__ (un array que contiene el detalle de cada producto comprado).

Para formatear la fecha (timestamp) pueden usar esta _helper function_ y ponerla en un archivo `.js` aparte (la convención es poner este tipo de funciones en un directorio llamado `/utils` dentro de `/src`):

```js
// src/utils/helpers.js

// Opciones para la fecha:
const options = {
  weekday: 'long',
  year: 'numeric',
  month: 'long',
  day: 'numeric',
  hour: 'numeric',
  minute: 'numeric'
}

// Crear y formatear la fecha
export function formattedDate() {
  const date = new Date()
  const timestamp = date.toLocaleDateString('es-ES', options)
  return timestamp + 'hs.'
}
```
Otra opción es guardar en el backend la fecha en bruto (`new Date()`) y luego formatearla en el momento de mostrarla.

Pero recuerden que para agregar un nuevo pedido dentro de este array __no pueden usar POST, deben usar PUT__. Si usan POST estarían generando un nuevo usuario, pero lo que deben hacer es modificar una propiedad dentro del objeto del usuario (o sea, la propiedad __orders__).

El _method_ para hacer esto podría ser algo así:

```js
import ax from 'dedalo-ax'

import { cartStore } from '@/stores/cartStore'
import { userStore } from '@/stores/userStore'
import { formattedDate } from '@/utils/helperFunctions'

const { VITE_MOCKAPI: baseUrl } = import.meta.env;

export default {

  data: () => ({ 
    cartStore, userStore
  }),

  methods: {
    async confirmOrder() {
      if (this.userStore.user.loggedIn) {

        const timestamp = formattedDate()
        const total = this.cartStore.totalPrice
        
        // Crear el objeto order que será agregado al array
        const order = {
          timestamp, total,
          // Clonar el carrito con un spread operator
          products: [ ...this.cartStore.cart]
        }
        
        // Obtener el usuario en MockAPI
        const endpoint = '/users/' + this.userStore.user.id
        const user = await ax.get(endpoint)
        
        // push al array orders dentro del objeto user
        user.orders.push(order)
         
        // Actualizar el objeto del usuario en MockAPI 
        const res = await ax.put(endpoint, user)
        console.log(res)

        // Mostrar un mensaje de confirmación de la compra
        // Puede ser en una modal o en una nueva vista
        this.$router.push('/confirm-order')
      } 
    }
  }
}
```


__12. Login y Signup:__ La consigna dice:

__*Crear un Login y Registro de usuarios utilizando los métodos GET y POST.*__

Es decir, el Login y Signup deben usar los métodos [GET](https://frontendlab.vercel.app/vue/simulando-un-login/#fetch-service) (para chequear en MockAPI si existe un usuario con el nombre y password ingresados) y [POST](https://frontendlab.vercel.app/vue/simulando-un-signup/#usando-un-fetch-de-tipo-post) (para guardar los datos del usuario en MockAPI en el caso del Signup).

Sería bueno que para chequear si existe un usuario con el nombre (o email) y passwords ingresados por el usuario __no hagan este chequeo en el frontend con *find()*__ ⚠️

```js
const endpoint = baseUrl + '/users'

// Traten de NO hacer esto:
const users = await ax.get(endpoint)
const user = users.find(user => user.username === this.form.username) 
```

Esto es porque traer al frontend __todos los datos de todos los usuarios incluyendo sus passwords__ es considerado una mala práctica, porque de esta forma cualquier usuario que sepa usar las Dev Tools del browser __podría tener acceso a todos los passwords de todos los usuarios__ 🤦

La única información que debería llegarle al frontend son los datos del usuario que está intentando hacer Login, __no de todos los usuarios__. Para poder pedirle a MockAPI únicamente este dato deben usar [search params](https://github.com/mockapi-io/docs/wiki/Code-examples#filtering). 

También se puede hacer esto mismo usando la sintaxis para [query strings](https://www.semrush.com/blog/url-parameters/), con un `?` luego del enpoint, luego el nombre de la propiedad a buscar (en este caso es `username`), luego un `=` y luego el dato que se quiere buscar (en este caso, lo que ingresó el usuario en el formulario de Login, o sea, `this.form.username`):

```js
const endpoint = baseUrl + '/users?username=' + this.form.username
```

MockAPI siempre retorna un array, y si no encuentra nada, retorna un array vacío. Por eso la respuesta al _query_ es el pimer elemento del array (`res[0]`). 

Entonces, el _method_ para el Login podría quedar así:

```js
async loginUser() {

  const baseUrl = 'https://123456789.mockapi.io/api'
   
  // Usando template literals en lugar de +
  const endpoint = `${baseUrl}/users?username=${this.form.username}`;

  const res = await ax.get(endpoint)

  // MockAPI siempre retorna un array
  // La respuesta al query es el pimer elemento del array
  // Si no encuentra nada, retorna un array vacío
  const user = res[0]
  
  if (!user) {
    this.errorMessage = 'Usuario no registrado.'

  // El chequeo del password no debería hacerse en el frontend, pero bueh... 🤷‍♂️️
  } else if (user.password !== this.form.password) {
    this.errorMessage = 'Contraseña incorrecta.'
  
  } else {
    // Por ahora guardar el usuario en data
    // luego debe ser guardado en Vuex o Pinia
    this.user = user
    
    this.form.resetForm()
    
    // Si van a guardar el usuario en localStorage
    // para persistir la sesión sería bueno
    // borrar el email y el password antes, por seguridad
    delete user.email
    delete user.password
    saveInStorage('user', user)
  }
}
```

Aunque lo que estamos haciendo no sea un e-commerce real sería bueno que lo hagan de esta forma como buena práctica. 

Y recuerden que la vista de Login debe mostrarse al cliquear en el botón (o ícono) de Login, __no en la vista inicial__. Esto es porque en un e-commerce (a diferencia de una red social o el sitio de un banco) __lo primero que el usuario debe ver son los productos, sin tener que loggearse para poder verlos__. El Login se hace recién cuando el usuario finaliza la compra (o antes, si el usuario quiere).


__13. Validaciones:__ Tanto el Login como el Signup deben tener validaciones. La validación del Login es que haya un usuario con el nombre (o e-mail) ingresado y que el password coincida con el registrado en el backend. Las instrucciones sobre cómo hacer esto las pueden encontrar [acá](https://frontendlab.vercel.app/vue/simulando-un-login/) (no es obligatorio que lo hagan de esta forma, se los paso únicamente por si les sirve como guía).

En el caso del Signup, las validaciones deben ser: que el nombre no sea ni demasiado corto ni demasiado largo, que el formato de e-mail sea correcto y que el password tenga algún tipo de condición (por ejemplo: al menos una mayúscula, al menos un número o al menos un guión). En el Signup también sería bueno chequear si no hay ya un usuario previamente registrado con ese nombre (o email). Las instrucciones sobre cómo hacer esto las pueden encontrar [acá](https://frontendlab.vercel.app/vue/simulando-un-signup/#simulando-un-signup).



__14. Bloqueos de navegación:__ Sería bueno incluir alguna forma de bloquear el ingreso forzado a la vista de Admin si, por ejemplo, un usuario __que no está loggeado__ ingresa a la ruta `/admin` solamente poniendo la URL de la ruta en la barra de navegación del browser:

http://localhost:5173/admin

Al hacer esto el usuario debería ser redirigido a la ruta de Login (`/login`) y no permitirle entrar en forma directa a `/admin`.

Y lo mismo si el usuario hizo Logout y luego vuelve atrás hacia `/admin` con el botón ⬅️ del browser: nuevamente, debería ser redirigido a `/login`.

Esto se puede hacer poniendo un condicional en `created()` dentro de _AdminView.vue_ o haciendo uso de las [navigation guards](https://router.vuejs.org/guide/advanced/navigation-guards.html#navigation-guards) de Vue Router:

```js
router.beforeEach((to, from, next) => {

  if (
    to.path.includes('admin') && 
    !userStore.loggedIn && 
    !userStore.user?.isAdmin
  ) 
    { 
      next({name: 'login'})

    } else if (
      to.path.includes('order') && 
      !userStore.loggedIn
      ) 
        { 
          next({name: 'login'})
          
        } else {
          next()
        }
})

```

__15.__ Este es un ejemplo de cómo podría quedar la estructura de archivos del proyecto:

```
.
├── index.html
│   
├── package.json
│   
├── .env.example
│   
├── vite.config.js
│   
├── /public
│   └── favicon.ico
│   
└── /src
    ├── App.vue
    │
    ├── /assets
    │   └── /css
    │       ├── atomic.css
    │       ├── main.css
    │       └── semantic.css
    │
    ├── /components
    │   │
    │   ├── /admin
    │   │   └── AdminTable.vue
    │   │
    │   ├── /cart
    │   │   ├── CartButtons.vue
    │   │   ├── CartCounter.vue
    │   │   ├── CartSideBar.vue
    │   │   └── CartTable.vue
    │   │
    │   ├── /form
    │   │   ├── EmailInput.vue
    │   │   ├── FirstNameInput.vue
    │   │   ├── LastNameInput.vue
    │   │   ├── PasswordInput.vue
    │   │   ├── ToolTip.vue
    │   │   └── UsernameInput.vue
    │   │
    │   ├── /icons
    │   │   └── CartIcon.vue, etc, etc
    │   │
    │   ├── /user
    │   │    ├── OrderDetail.vue
    │   │    └── UserAvatar.vue
    │   │ 
    │   ├── ModalWindow.vue
    │   ├── NavBar.vue
    │   └── ProductCard.vue
    │   
    ├── main.js
    │   
    ├── /router
    │   └── index.js
    │   
    ├── /services
    │   └── fetchService.js
    │   
    ├── /stores
    │   ├── cartStore.js
    │   ├── formStore.js
    │   ├── productsStore.js
    │   ├── userStore.js
    │   └── validationClass.js
    │   
    ├── /utils
    │   └── helperFunctions.js
    │   
    └── /views
        │
        ├── /admin
        │   ├── AddOrUpdateProduct.vue
        │   ├── AdminView.vue
        │   └── TotalOrdersView.vue
        │
        ├── /user
        │   ├── ConfirmOrderView.vue
        │   ├── LoginView.vue
        │   ├── OrdersView.vue
        │   ├── SignupMessage.vue
        │   └── SignupView.vue
        │
        ├── HomeView.vue
        ├── NotFoundView.vue
        └── ProductView.vue
```


__16.__ Y acá pueden ver un ejemplo de cómo quedaría el JSON de usuarios en MockAPI sumándole el array de pedidos (orders):

```json
[
  {
    "id": "4",
    "firstname": "Linus",
    "lastname": "Octocat",
    "username": "user1",
    "password": "$2a$08$xUuXJym2o2OZUU3l3g/mEeasbeditdtN6Cp7LBVujS.H.9WJkeJaS",
    "email": "linus@octocat.com",
    "avatar": "https://dav-leda.github.io/api/images/avatar-mac.png",
    "isAdmin": false,
    "orders": [
    {
      "timestamp": "jueves, 22 de junio de 2023, 13:37hs.",
      "total": 2940,
      "products": [
      {
        "id": "18",
        "name": "Lemon Pie",
        "price": 1200,
        "stock": 12,
        "description": "Producto elaborado con ingredientes 100% orgánicos.",
        "imgsrc": "https://dav-leda.github.io/images-bakery/apple-pie.jpg",
        "qty": 2,
        "subtotal": 2400
      },
      {
        "id": "16",
        "name": "Donuts de chocolate",
        "price": 180,
        "stock": 12,
        "description": "Producto elaborado con ingredientes 100% orgánicos.",
        "imgsrc": "https://dav-leda.github.io/images-bakery/donuts-choco.jpg",
        "qty": 3,
        "subtotal": 540
      }
      ]
    },
    {
        "timestamp": "martes, 20 de junio de 2023, 13:25hs.",
        "total": 700,
        "products": [
          {
            "id": "4",
            "name": "Muffin de manzana",
            "price": 350,
            "stock": 12,
            "description": "Producto elaborado con ingredientes 100% orgánicos.",
            "imgsrc": "https://dav-leda.github.io/images-bakery/muffin-manzana.jpg",
            "qty": 2,
            "subtotal": 700
          }
        ]
      }
    ]
  }
]
```
Si se fijan van a ver que el password está _hasheado_. El password real es __test123__, lo que se guardó en MockAPI es un _hash_ de ese password. En general es considerado una buena práctica guardar los passwords en el backend en forma de _hash_ __para que ni siquiera el admin pueda saber cuáles son los passwords ingresados por los usuarios__ (ya que el _hashing_, a diferencia de la encriptación, es irreversible). No es necesario que lo hagan de esta forma, pero si quieren hacerlo pueden encontrar las instrucciones [acá](https://frontendlab.vercel.app/vue/simulando-un-login/#encriptacion-del-password).


<hr>

Cualquier duda que tengan me pueden escribir, preferentemente por Discord ya que la plataforma de CH no siempre funciona bien.

<hr>