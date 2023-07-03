# Curso Vue.js 💻️ 🛠️
## Tutor: David Leda
### Tercera Pre-entrega del Proyecto Final
---

Hola 🙋‍♂️️ Acá les paso algunas recomendaciones para la __Pre-entrega 3 del Proyecto Final__:

__1. GitHub:__ Por favor hagan la entrega con __un link al repositorio en GitHub__, no en un zip ni Dropbox 

__2. Errores en consola:__ Antes de hacer la entrega __por favor chequeen si no les está dando algún error por consola__.

__3. Vuex__: La consigna consiste en integrar Vuex al proyecto, aunque no queda muy claro para cuál de los estados de la aplicación hay que usarlo. Si se fijan en el ejemplo de la página 15 de la clase 1, en el archivo __router.js__, los estados a los que se refieren son los de __carrito__ y __usuario__ (aunque no deberían estar ambos en un mismo _store_ como en ese ejemplo sino separados en módulos, como vimos en clase).

Si para la entrega anterior usaron una _store_ simple para el carrito (_cartStore_) o para el usuario (_userStore_) recuerden que en esta entrega deben reemplazarlos por Vuex, no deben usarse los dos tipos de _store_ al mismo tiempo para un mismo estado. De todas formas, como la estructura es bastante similar, pueden usar las _stores_ simples como guía para las _stores_ de Vuex.

__4. Vuex 4__: Si están usando __Vue 2__, la versión de Vuex que deben instalar es la __3__, y si están usando __Vue 3__, la versión de Vuex es la __4__. Para instalar Vuex 3 pueden hacerlo como se vio en clase. Para registrar __Vuex 4__ en __Vue 3__ es así:

En __main.js__:

```js
import { createApp } from 'vue'
import App from './App.vue'

import router from './router'
import { store } from './stores'

import './assets/css/main.css'

createApp(App)
  .use(router)
  .use(store)
  .mount('#app')
```

__5. Módulos de Vuex:__ Para crear los dos módulos (carrito y usuario) pueden hacerlo así:

Dentro de __/src__ crean un directorio llamado __/stores__ y dentro de éste un archivo __index.js__:

```js
// src/stores/index.js
// Vuex 4:
import { createStore } from 'vuex'

// Esto es igual para cualquier versión de Vuex:
import { cart } from './modules/cart'
import { user } from './modules/user'

// El método createStore es para Vuex 4:
export const store = createStore({
  
  modules: {
    cart, user
  }  
})
```
Esta forma de importar Vuex (`import { createStore } from 'vuex'`) es solamente para Vuex 4. Si están usando Vuex 3 con Vue 2 es como vimos en clase. Todo lo demás es igual para ambas versiones de Vuex.

Luego, dentro de __/src__, un directorio llamado __/modules__, y dentro de __/modules__ dos archivos: __cart.js__ y __user.js__. Y en cada uno de estos archivos, un objeto con 5 propiedades: __namespaced, state, getters, mutations__ y __actions:__

```js
// src/stores/modules/user.js

import { 
  saveInStorage, 
  getFromStorage, 
  removeFromStorage
} from '@/utils/helperFunctions'


export const user = {

  namespaced: true,

  state: {
  },

  getters: {
  },

  mutations: {
  },

  actions: {
  }
}
```
La propiedad __namespaced__ es para que los _getters_ y las _actions_ de cada módulo puedan ser referenciados por el nombre del módulo (en este caso __user__).


__6. Módulo del usuario:__ El módulo del usuario puede ser así (no es necesario que sea exactamente igual, depende de cómo hayan implementado la lógica del usuario en el proyecto):

```js
// src/stores/modules/user.js

// helpers para guardar data en localStorage:
import { 
  saveInStorage, 
  getFromStorage, 
  removeFromStorage
} from '@/utils/helperFunctions'


export const user = {

  namespaced: true,

  state: {
    user: null
  },

  getters: {
    user: state => state.user,
    loggedIn: state => !!state.user,
    isAmdin: state => state.user?.isAmdin || false
  },

  mutations: {
    setUser: (state, user) => state.user = user   
  },

  actions: {

    getUser: ({ commit }) => {
      const user = getFromStorage('user') || null;
      commit('setUser', user)
    },

    setUser: ({ commit }, user) => {

      // Antes de guardar el usuario en localStorage
      // es una buena práctica evitar que el password quede expuesto en el storage:
      delete user.password

      saveInStorage('user', user)
      commit('setUser', user)
    },

    logoutUser: ({ commit }) => {
      removeFromStorage('user')
      commit('setUser', null)
    }
  }
}
```

Para las __actions__, en lugar de `context.commit` se puede usar `({ commit })`, tal como indica [la documentación de Vuex](https://v3.vuex.vuejs.org/guide/actions.html#dispatching-actions).

Recuerden que las __actions__ son llamadas desde los componentes, y con __commit__ ejecutan las __mutations__, que a su vez modifican el estado de la _store_.

Y los __getters__ obtienen algún valor a partir del estado, pero no modifican el estado.

Como ven, las __actions__ son similares a los __methods__ y los __getters__ son similares a las __computed__, mientras que __state__ es similar a __data__.

__7. Vuex en los componentes:__ Ni el __state__ ni las __mutations__ de Vuex pueden ser accedidas en forma directa desde los componentes, sino a través de los __getters__ y las __actions__, usando los métodos __mapGetters__ y __mapActions__. Estos métodos deben ser importados en el componente donde sean usados.

__mapGetters__ inserta los __getters__ de Vuex dentro de las __computed__ del componente y __mapActions__ inserta las __actions__ dentro de los __methods__:

```js

import { mapGetters, mapActions } from 'vuex'

export default {

  props: {
    product: {
      id: String,
      name: String,
      price: Number,
      stock: Number,
      imgsrc: String
    }
  },

  computed: {
    // El primer param de mapGetters es el nombre del módulo
    ...mapGetters('cart', ['findById']),

    inCart() {
      // Usar el getter de Vuex dentro de una computed:
      return this.findById(this.product.id)
    },
    color() {
      return this.inCart ? 'verde' : 'azul'
    }
  },

  methods: {
    // Cambiarle el nombre a la action para evitar
    // que se superponga con el nombre del method
    ...mapActions('cart', { addToCartAction: 'addToCart' }),

    // No pueden llamarse igual
    addToCart() {
      this.addToCartAction(this.product)
    }
  }
}
```
En el caso de que el nombre de una __action__ coincida con el de un __method__ deben declararlo con un nombre distinto (usando llaves en lugar de corchetes) para evitar que se superpongan (y lo mismo si tienen un __getter__ con el mismo nombre de una __computed__).

__6. Módulo del carrito:__ Toda la lógica del carrito debería ser manejada por Vuex, no por los componentes, de lo contrario es posible que el carrito no les funcione bien (por ejemplo, si el estado del carrito es mutado en formas distintas desde cada componente).

Una forma posible de hacerlo es así (no es necesario que sea exactamente igual, depende de cómo hayan armado la lógica del carrito en el proyecto):

```js
export const cart = {

  namespaced: true,
  
  state: {
    cart: []
  },

  getters: {
    cart: state => state.cart,
    findById: state => id => state.cart.find(p => p.id === id),
    findIndexById: state => id => state.cart.findIndex(p => p.id === id),
    cartTotalQty: state => state.cart.length,
    cartTotalPrice: state => state.cart.reduce((total, p) => total + p.subtotal, 0)
  },

  mutations: {
    addToCart: (state, newProduct) => {
      state.cart.push(newProduct)
    },
    removeFromCart: (state, index) => {
      state.cart.splice(index, 1)
    },
    emptyCart: (state) => {
      state.cart = []
    }
  },
  
  actions: {
    addToCart: ({ commit }, product) => {
      const newProduct = {
        ...product,
        qty: 1,
        subtotal: product.price
      }
      commit('addToCart', newProduct)
    },
    removeFromCart: ({ commit, getters }, id) => {
      const index = getters.findIndexById(id)
      commit('removeFromCart', index)
    },
    emptyCart: ({ commit }) => {
      commit('emptyCart')
    }
  }
}
```

__7. Getters:__  Como ven los __getters__ de Vuex son [funciones que retornan otras funciones](https://yeisondaza.com/currying-en-javascript-funciones-con-superpoderes), por eso se declaran con __arrow functions__ encadenadas. De todas formas, para invocarlos pueden hacerlo como con cualquier otro método:

En el componente:

```js
this.findById(this.product.id)
```

Y al usarlos dentro de una __action__ del mismo módulo, anteponiendo `getters.`, no `this.`:

```js
removeFromCart: ({ commit, getters }, id) => {
  const index = getters.findIndexById(id)
  commit('removeFromCart', index)
},
```

__8. Vuex en Vue Router:__ Para acceder a los __getters__ de Vuex dentro de Vue Router pueden hacerlo así:

```js
import { store } from '../stores'

router.beforeEach((to, from, next) => {

  if (
    to.path.includes('admin') && 
    !store.getters['user/loggedIn'] && 
    !store.getters['user/isAdmin']
  ) { 
    next({name: 'login'})

  } else if (
    to.path.includes('order') && 
    !store.getters['user/loggedIn']
  ) { 
    next({name: 'login'})
        
  } else {
    next()
  }
})
```

<hr>

Cualquier duda que tengan me pueden escribir, preferentemente por Discord ya que la plataforma de CH no siempre funciona bien.

<hr>