# Introduction

## vue

### ¿Qué es vue?

Vue (/vjuː/) es un framework para construir interfaces de usuario (UI).

### Instalación

Para utilizar vue necesitamos:

- incluir la libreria en nuestra página
```html
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.17/dist/vue.js"></script>
```

- definir el elemento de carga
```html
<div id="app"></div>
```

- crear la instancia de vue.
```html
<script>
  new Vue({
    el: "#app"
  })
</script>
```

### Hello vue

Indicamos a Vue el texto a renderizar a través de los _mustaches_ \{\{  \}\}

> Se emplea como entorno de pruebas https://jsbin.com/?html,output

> CDN de vue https://www.jsdelivr.com/package/npm/vue

```html
<html>
  <head>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.17/dist/vue.js"></script>
  </head>
  <body>
    <div id="app">
      <p>{{ message }}</p>
    </div>
    <script>
      var app = new Vue({
        el: "#app",
        data: {
          message: "hello vue!"
        }
      })
    </script>
  </body>
</html>
```

### Vincular atributos, directiva `v-bind` (:)

Una directiva vue proporciona herramientas para controlar el DOM de forma reactiva. Comienzan con el prefijo `v-`, indicando que pertenecen a vue.

Por ejemplo `v-bind` vincula cualquier atributo del elemento con el valor definido.

```html
<html>
  <head>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.17/dist/vue.js"></script>
  </head>
  <body>
    <div id="app">
      <p>{{ message }}</p>
      <span v-bind:title="when">Hover your mouse over here</span>
    </div>
    <script>
      var app = new Vue({
        el: "#app",
        data: {
          message: "hello vue!",
          when: `You loaded this page on ${new Date().toLocaleString()}`
        }
      })
    </script>
  </body>
</html>
```

### Condicionales, directiva `v-if`

Vue puede controlar la estructura del DOM, mostrando elemento de forma condicional.

```html
<html>
  <head>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.17/dist/vue.js"></script>
  </head>
  <body>
    <div id="app">
      <p>{{ message }}</p>
      <span v-if="seen">Now you see me</span>
    </div>
    <script>
      new Vue({
        el: "#app",
        data: {
          message: "hello vue!",
          seen: true
        }
      })
    </script>
  </body>
</html>
```

consola

```javaScript
app.seen = false
```

### Eventos, directiva `v-on` (@)

Vue es capaz de capturar los eventos generados por los elementos.

```html
<html>
  <head>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.17/dist/vue.js"></script>
  </head>
  <body>
    <div id="app">
      <p>{{ message }}</p>
      <span v-if="seen">Now you see me</span>
      <button v-on:click="onClick">show/hide</button>
    </div>
    <script>
      var app = new Vue({
        el: "#app",
        data: {
          message: "hello vue!",
          seen: true
        },
        methods: {
          onClick () {
            this.seen = !this.seen
          }
        }
      })
    </script>
  </body>
</html>
```

Reemplazar `v-on:` por `@`

### Two-way binding, directiva `v-model`

Vue permite también la vinculación en sentido de lectura y escritura.

```html
<html>
  <head>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.17/dist/vue.js"></script>
  </head>
  <body>
    <div id="app">
      <p>{{ message }}</p>
      <input v-model="name"></input>
    </div>
    <script>
      var app = new Vue({
        el: "#app",
        data: {
          name: 'vue'
        },
        computed: {
          message: {
            get () {
              if (!this.name) return `Hello stranger!`
              return `Hello ${this.name}!`
            }
          }
        }
      })
    </script>
  </body>
</html>
```

### Bucles, directiva `v-for`

También podemos recorrer un array y mostar sus elementos.

```html
<html>
  <head>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.17/dist/vue.js"></script>
  </head>
  <body>
    <div id="app">
      <ol>
        <li v-for="todo in todos">
          {{ todo.text }}
        </li>
      </ol>
    </div>
    <script>
      var app = new Vue({
        el: "#app",
        data: {
          todos: [
            { text: 'Learn JavaScript' },
            { text: 'Learn Vue' },
            { text: 'Build something awesome' }
          ]
        }
      })
    </script>
  </body>
</html>
```

### Componentes

Un componente vue permite abstraer funcionalidades y facilitar la gestión de los proyectos, por ejemplo permitiendo la reusabilidad.

```html
<html>
  <head>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.17/dist/vue.js"></script>
  </head>
  <body>
    <div id="app">
      <ol>
        <todo-item v-for="(todo, index) in todos" :todo="todo" :key="index">
        </todo-item>
      </ol>
    </div>
    <script>
      Vue.component('todo-item', {
        props: ['todo'],
        template: '<li>{{ todo.text }}</li>'
      })
      var app = new Vue({
        el: "#app",
        data: {
          todos: [
            { text: 'Learn JavaScript' },
            { text: 'Learn Vue' },
            { text: 'Build something awesome' }
          ]
        }
      })
    </script>
  </body>
</html>
```

![lifecycle](img/lifecycle.png)

## vuex

Vuex permite gestionar el estado de la aplicación, también conocido como store.

- Es reactivo, ante cambios en el estado actualiza automáticamente los componentes.
- Define un flujo unidireccional, a través de mutaciones, para cambiar el estado.

-- buscar imagen con flujo

```javascript
const store = new Vuex.Store({
  state: {
    count: 0
  }
})
```

![vuex](img/vuex.png)

### mutations

Las mutaciones modifican el estado de la aplicación.

```javascript
const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})
```

```javascript
store.commit('increment')
```

### actions

Las acciones son procesos asíncronos que ejecutan mutaciones.

```javascript
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  },
  actions: {
    increment ({ commit }) {
      commit('increment')
    }
  }
})
```

```javascript
store.dispatch('increment')
```

### getters

Permiten obtener valores calculados a partir del estado del store.

```javascript
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos: state => {
      return state.todos.filter(todo => todo.done)
    }
  }
})
```

```javascript
store.getters.doneTodos
```

## vue router

Vue router facilita la creacción de SPA (Single Page Applications).

```javascript
const router = new VueRouter({
  routes: [
    // the following object is a route record
    { path: '/foo', component: Foo,
      children: [
        // this is also a route record
        { path: 'bar', component: Bar }
      ]
    }
  ]
})
```

### router-view

El componente `<router-view>` renderiza el componente asignado a la ruta.


### router-link

El componente `<router-link>` permite la navegación dentro de la aplicación.

```html
<router-link to="/foo">
  <a>/foo</a>
</router-link>
```
