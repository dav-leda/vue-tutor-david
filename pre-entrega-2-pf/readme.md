# Curso Vue.js 💻️ 🛠️
## Tutor: David Leda
### Segunda Pre-entrega del Proyecto Final
---

Hola 🙋‍♂️️ Acá les paso algunas recomendaciones para la __Pre-entrega 2 del Proyecto Final__:

__1. GitHub:__ Por favor hagan la entrega con __un link al repositorio en GitHub__, no en un zip ni Dropbox 🙏️🙏️🙏️

__2. Errores en consola:__ Antes de hacer la entrega __por favor chequeen si no les está dando algún error por consola__ 🙏️🙏️🙏️

Si no pueden solucionar el error no hay problema, pueden entregar igual, pero en ese caso avisen al hacer la entrega: "Me está dando tal error por consola."

__3. Axios:__ La consigna dice que deben **integrar Axios**. Para eso recuerden que primero deben instalarlo (`npm i axios`). 

También pueden usar el método nativo `fetch` de JavaScript en lugar de Axios. Usar `fetch` les va a ahorrar unos 30Kb en el _bundle_ final. Teniendo en cuenta que todo Vue pesa unos 50Kb (o sea, Vue 3 con _tree shaking_ pesa 50Kb, Vue 2 pesa unos 70Kb) sumar 30Kb sólo para ahorrarse un par de líneas de código en la petición HTTP no tiene mucho sentido. Pero bueno, la consigna dice usar Axios, así que si quieren úsenlo 🤷‍♂️️

__4. MockAPI:__ La consigna dice que deben **consumir los recursos desde el backend en MockAPI.** Para crear una cuenta en [MockAPI](https://mockapi.io)  pueden seguir [estas instrucciones](https://frontendlab.vercel.app/vue/simulando-un-login/#mockapi). En MockAPI deben __crear 2 recursos: uno para productos y otro para usuarios.__ 

Es decir que el JSON con el listado de productos y el del listado de usuarios deben estar en MockAPI, __no *hardcodeados* dentro de los componentes en *data*__:

![mockapi](https://dav-leda.github.io/api/images/mockapi-relations.png)

Hasta el año pasado el plan gratuito de MockAPI permitía generar hasta 3 _resources_ e incluso vincularlos, como se ve en la imagen (por ejemplo vincular el recurso de usuarios con el de pedidos para que automáticamente cada pedido quede asociado a un usuario en particular). Pero desde hace unos meses limitaron su plan gratuito a 2 _resources_ 😐️ con lo cual, para poder asociar cada pedido a un usuario hay que crear un array de pedidos dentro de cada objeto de usuario 😒️

__5. Variables de entorno:__ En cuanto a la URL de MockAPI, recuerden que MockAPI genera una URL única que contiene el token de su cuenta personal:

https://numero-de-token.mockapi.io/api/

Sería bueno que guarden esta URL en una [variable de entorno](https://frontendlab.vercel.app/vue/simulando-un-login/#variables-de-entorno) de lo contrario quedaría expuesta en el repositorio y cualquiera puede usar su cuenta de MockAPI sin necesidad de crear una propia (hay bots que recorren automáticamente todos los repositorios de GitHub buscando este tipo de información). 

Para usar variables de entorno pueden seguir las instrucciones que están [acá](https://frontendlab.vercel.app/vue/simulando-un-login/#variables-de-entorno).

__6. Estructura general del proyecto:__ Como dice la consigna, el proyecto debe incluir __Login__ y __Signup__ (registro de usuarios), __listado de productos__, __carrito de compras, listado de pedidos y ABM de productos__. 

Sería bueno que agrupen los componentes en distintas carpetas, según su función: los del carrito en una carpeta, los del usuario en otra, los del admin en otra. Y si usan íconos SVG, crear un componente para cada uno y ponerlos en una carpeta llamada _icons_.

Y lo mismo para las vistas (views): las de admin en una, las de usuario (cliente) en otra.

Y también para los archivos .js con la lógica: la lógica del `fetch` en un archivo .js dentro de una carpeta llamada _services_ (esa es la convención para nombrar a los servicios de acceso a una API). Y para la lógica de las funciones reutilizables (o sea, para formatear precios, formatear fechas, guardar en _localStorage_) la convención es que la carpeta se llame _utils_ (o _helpers_).

Este es un ejemplo de cómo podría quedar la estructura de archivos del proyecto:

```
.
├── index.html
│   
├── package.json
│   
├── public
│   └── favicon.ico
│   
├── src
│   ├── App.vue
│   │
│   ├── assets
│   │   └── css
│   │       ├── atomic.css
│   │       ├── main.css
│   │       └── semantic.css
│   │
│   ├── components
│   │   │
│   │   ├── admin
│   │   │   └── AdminTable.vue
│   │   │
│   │   ├── cart
│   │   │   ├── CartButtons.vue
│   │   │   ├── CartCounter.vue
│   │   │   ├── CartSideBar.vue
│   │   │   └── CartTable.vue
│   │   │
│   │   ├── form
│   │   │   ├── EmailInput.vue
│   │   │   ├── FirstNameInput.vue
│   │   │   ├── LastNameInput.vue
│   │   │   ├── PasswordInput.vue
│   │   │   ├── ToolTip.vue
│   │   │   └── UsernameInput.vue
│   │   │
│   │   ├── icons
│   │   │   ├── BackIcon.vue
│   │   │   ├── CartAddIcon.vue
│   │   │   ├── CartIcon.vue
│   │   │   ├── CheckIcon.vue
│   │   │   ├── CloseIcon.vue
│   │   │   ├── EditIcon.vue
│   │   │   ├── EmailIcon.vue
│   │   │   ├── EyeIcon.vue
│   │   │   ├── HeartIcon.vue
│   │   │   ├── PasswordIcon.vue
│   │   │   ├── TrashIcon.vue
│   │   │   ├── UserIcon.vue
│   │   │   └── UsernameIcon.vue
│   │   │
│   │   ├── user
│   │   │    ├── OrderDetail.vue
│   │   │    └── UserAvatar.vue
│   │   │ 
│   │   ├── ModalWindow.vue
│   │   ├── NavBar.vue
│   │   └── ProductCard.vue
│   │   
│   ├── main.js
│   │   
│   ├── router
│   │   └── index.js
│   │   
│   ├── services
│   │   └── fetchService.js
│   │   
│   ├── stores
│   │   ├── cartStore.js
│   │   ├── formStore.js
│   │   ├── productsStore.js
│   │   ├── userStore.js
│   │   └── validationClass.js
│   │   
│   ├── utils
│   │   └── helperFunctions.js
│   │   
│   └── views
│       │
│       ├── admin
│       │   ├── AddOrUpdateProduct.vue
│       │   ├── AdminView.vue
│       │   └── TotalOrdersView.vue
│       │
│       ├── user
│       │   ├── ConfirmOrderView.vue
│       │   ├── LoginView.vue
│       │   ├── OrdersView.vue
│       │   ├── SignupMessage.vue
│       │   └── SignupView.vue
│       │
│       ├── HomeView.vue
│       ├── NotFoundView.vue
│       └── ProductView.vue
│   
└── vite.config.js
```

__7. ABM de productos__: La consigna dice:

__*Crear un recurso en el backend para listar productos o servicios, incorporando los métodos (GET, POST, PUT, DELETE).*__

__*Tendrás que crear un usuario Admin (ABM de productos e Información general).*__

Esta parte de la consigna es bastante confusa, pero si ven el ejemplo de proyecto final que está en la página 15 de la clase 1, a lo que se están refiriendo con esto es a un [CMS](https://es.wikipedia.org/wiki/Sistema_de_gesti%C3%B3n_de_contenidos) en forma de [CRUD](https://es.wikipedia.org/wiki/CRUD).

__ABM__ quiere decir __Alta, Baja, Modificación__, que es otra forma de decir __CRUD__. La idea es que el usuario _admin_ pueda crear, modificar o borrar el listado de productos, y estos cambios se tienen que ver reflejados en el backend de MockAPI.

Para probar el ejemplo de Coder House el usuario es __admin@admin__ y el password __123__

También pueden probar entrando como _admin_ en este otro ejemplo armado por mí (usuario: __admin1__, password: __test123__):

<a href="https://vue-bakery-2.vercel.app/" target="_blank">https://vue-bakery-2.vercel.app/</a>

Luego de hacer Login como admin van a ver que en el dropdown del usuario aparecen las opciones: Mi Perfil, Pedidos, __Productos__ y Logout. Si entran a __Productos__ van a ver el __CMS de productos__ con la opción de agregar nuevos productos, editar algún dato del producto, o borrar el producto. Todos estos cambios tiene que verse reflejados en el backend de MockAPI.

*Si quieren probar la opción de borrar productos por favor haganlo con alguno que hayan creado ustedes, por favor no borren los productos listados.*

Para la vista de agregar un nuevo producto y de actualizar un producto pueden reutilizar el mismo componente, cambiando únicamente el _id_ de la ruta:

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
  this.$router.push('/admin/product/new-product')
},

goToEditProduct(product) {
  this.$router.push(`/admin/product/${product.id}`)
},
```
Y luego, en el componente __AddOrUpdateProduct.vue__, si el _id_ de la ruta es `new-product` entonces `this.product` puede ser un objeto vacío o un _placeholder_ con datos iniciales. Y si no es `new-product`, hacer un `fetch` a MockAPI para buscar los datos del producto a ser modificado haciendo uso del _id_ de la ruta (que es igual a `product.id`):

```js
async created() {
  if (this.$route.params.id === 'new-product') {
    this.product = placeholderProduct
  } else {
    this.product = await fetchService.getData(
      `/products/${this.$route.params.id}`
    )
  }
}
```

__8. Pedidos:__ La consigna dice:

__*Crear un último recurso que será el carrito, integrando GET y POST para realizar y revisar pedidos.*__

Esto también es bastante confuso, pero queda más claro al ver el ejemplo de la página 15 de la clase 1. Lo que están pidiendo es que el usuario _admin_ pueda ver un listado de __todos los pedidos realizados por todos los usuarios__ que usaron la aplicación.

Es decir, el recurso __no es para el carrito, es para pedidos.__ La información del carrito es efímera (si el usuario no completó la compra y cierra la aplicación, se borra, y si completó la compra también, porque el carrito se resetea), pero la información de pedidos completados debería ser persistida en el backend. 

Hay algunos e-commerce (Mercado Libre) que también persisten la información del carrito en el backend y luego la borran cuando el pedido fue realizado, y hay otros que la guardan en _localStorage_ (y también, luego de completado el pedido, la borran). En este caso, no es necesario que guarden el carrito pero sí los pedidos.

El problema es que el plan gratuito de MockAPI sólo permite crear 2 recursos, y ya usamos uno para usuarios y otro para productos 🤔️ 

La solución es que dentro del recurso de usuarios, en el objeto de cada usuario haya __un array de pedidos (orders)__:

```json
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
    "total": 9090,
    "products": [
     {
      "id": "17",
      "priority": 17,
      "name": "Torre de chocolate",
      "price": 240,
      "stock": 7,
      "description": "Producto elaborado con ingredientes 100% orgánicos. Reducido en calorías. Libre de TACC. Apto para celíacos. Apto para veganos. No contiene ingredientes de origen animal.",
      "imgsrc": "https://dav-leda.github.io/images-bakery/torre-chocolate.jpg",
      "qty": 3,
      "subtotal": 720
     },
     {
      "id": "18",
      "priority": 18,
      "name": "Crumble de manzana",
      "price": 420,
      "stock": 12,
      "description": "Producto elaborado con ingredientes 100% orgánicos. Reducido en calorías. Libre de TACC. Apto para celíacos. Apto para veganos. No contiene ingredientes de origen animal.",
      "imgsrc": "https://dav-leda.github.io/images-bakery/apple-pie.jpg",
      "qty": 7,
      "subtotal": 2940
     },
     {
      "id": "13",
      "priority": 13,
      "name": "Lemon Pie",
      "price": 870,
      "stock": 9,
      "description": "Producto elaborado con ingredientes 100% orgánicos. Reducido en calorías. Libre de TACC. Apto para celíacos. Apto para veganos. No contiene ingredientes de origen animal.",
      "imgsrc": "https://dav-leda.github.io/images-bakery/lemon-pie-2.jpg",
      "qty": 5,
      "subtotal": 4350
     },
     {
      "id": "16",
      "priority": 16,
      "name": "Donuts de chocolate",
      "price": 180,
      "stock": 12,
      "description": "Producto elaborado con ingredientes 100% orgánicos. Reducido en calorías. Libre de TACC. Apto para celíacos. Apto para veganos. No contiene ingredientes de origen animal.",
      "imgsrc": "https://dav-leda.github.io/images-bakery/donuts-choco.jpg",
      "qty": 6,
      "subtotal": 1080
     }
    ]
   },
   {
      "timestamp": "martes, 20 de junio de 2023, 13:25hs.",
      "total": 1850,
      "products": [
        {
          "id": "4",
          "priority": 4,
          "name": "Muffin de manzana",
          "price": 350,
          "stock": 12,
          "description": "Producto elaborado con ingredientes 100% orgánicos. Reducido en calorías. Libre de TACC. Apto para celíacos. Apto para veganos. No contiene ingredientes de origen animal.",
          "imgsrc": "https://dav-leda.github.io/images-bakery/muffin-manzana.jpg",
          "qty": 2,
          "subtotal": 700
        },
        {
          "id": "2",
          "priority": 2,
          "name": "Cheese Cake",
          "price": 280,
          "stock": 15,
          "description": "Producto elaborado con ingredientes 100% orgánicos. Reducido en calorías. Libre de TACC. Apto para celíacos. Apto para veganos. No contiene ingredientes de origen animal.",
          "imgsrc": "https://dav-leda.github.io/images-bakery/cheese-cake-frutos-rojos.jpg",
          "qty": 1,
          "subtotal": 280
        },
        {
          "id": "13",
          "priority": 13,
          "name": "Lemon Pie",
          "price": 870,
          "stock": 9,
          "description": "Producto elaborado con ingredientes 100% orgánicos. Reducido en calorías. Libre de TACC. Apto para celíacos. Apto para veganos. No contiene ingredientes de origen animal.",
          "imgsrc": "https://dav-leda.github.io/images-bakery/lemon-pie-2.jpg",
          "qty": 1,
          "subtotal": 870
        }
      ]
    }
  ]
}
```

El array __orders__ tiene 3 propiedades: __timestamp__ (el día y hora en que fue realizado el pedido), __total__ (el costo total del pedido) y __products__ (un array que contiene el detalle de cada producto comprado).

Para formatear la fecha (timestamp) pueden usar esta _helper function_:

```js
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

Como habrán visto, el password está _hasheado_. El password real es __test123__, lo que se guardó en MockAPI es un _hash_ de ese password. En general es considerado una buena práctica guardar los passwords en el backend en forma de _hash_ __para que ni siquiera el admin pueda saber cuáles son los passwords ingresados por los usuarios__ (ya que el _hashing_, a diferencia de la encriptación, es irreversible). No es necesario que lo hagan de esta forma, pero si quieren hacerlo pueden encontrar las instrucciones [acá](https://frontendlab.vercel.app/vue/simulando-un-login/#encriptacion-del-password).

__9. Login y Signup:__ La consigna dice:

__*Crear un Login y Registro de usuarios utilizando los métodos GET y POST.*__

Es decir, el Login y Signup deben usar los métodos [GET](https://frontendlab.vercel.app/vue/simulando-un-login/#fetch-service) (para chequear en MockAPI si existe un usuario con el nombre y password ingresados) y [POST](https://frontendlab.vercel.app/vue/simulando-un-signup/#usando-un-fetch-de-tipo-post) (para guardar los datos del usuario en MockAPI en el caso del Signup).

Sería bueno que para chequear si existe un usuario con el nombre (o email) y passwords ingresados por el usuario __no hagan este chequeo en el frontend con *find()*__. Esto es porque traer al frontend __todos los datos de todos los usuarios incluyendo sus passwords__ es considerado una mala práctica, porque de esta forma cualquier usuario que sepa usar las Dev Tools del browser __podría tener acceso a todos los passwords de todos los usuarios__ 🤦‍♂️️

Aunque lo que estamos haciendo no sea un e-commerce real sería bueno que lo hagan de esta forma como buena práctica. 

La única información que debería llegarle al frontend son los datos del usuario que está intentando hacer Login, __no de todos los usuarios__. Para poder pedirle a MockAPI únicamente este dato deben usar [search params](https://github.com/mockapi-io/docs/wiki/Code-examples#filtering). 

La documentación de MockAPI sobre el uso de _search params_ no es muy clara al respecto. Si les resulta más claro, pueden probar con [estas instrucciones](https://frontendlab.vercel.app/vue/simulando-un-login/#buscar-el-nombre-de-usuario).

__10. Validaciones:__ Tanto el Login como el Signup deben tener validaciones. La validación del Login es que haya un usuario con el nombre (o e-mail) ingresado y que el password coincida con el registrado. Las instrucciones sobre cómo hacer esto las pueden encontrar [acá](https://frontendlab.vercel.app/vue/simulando-un-login/) (no es obligatorio que lo hagan de esta forma, se los paso únicamente por si les sirve como guía).

En el caso del Signup, las validaciones deben ser: que el nombre no sea ni demasiado corto ni demasiado largo, que el formato de e-mail sea correcto y que el password tenga algún tipo de condición (por ejemplo: al menos una mayúscula, al menos un número o al menos un guión). En el Signup también sería bueno chequear si no hay ya un usuario previamente registrado con ese nombre (o email). Las instrucciones sobre cómo hacer esto las pueden encontrar [acá](https://frontendlab.vercel.app/vue/simulando-un-signup/#simulando-un-signup).

__11. Bloqueos de navegación:__ Sería bueno incluir alguna forma de bloquear el ingreso forzado a la aplicación. Esto ocurre cuando un usuario __que no está loggeado__ ingresa a la ruta `/admin` solamente ingresando la URL en la barra de navegación del browser:

http://localhost:5173/admin

Al hacer esto el usuario debería ser redirigido a la ruta de Login ('/login') y no permitirle entrar en forma directa a `/admin`.

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


<hr>

Cualquier duda que tengan me pueden escribir, preferentemente por Discord ya que la plataforma de CH no siempre funciona bien 🤷‍♂️️

<hr>