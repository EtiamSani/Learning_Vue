Source : https://vuejs.org/tutorial

# Declarative Rendering 

Le rendu côté client se fait grâce à l'export default, les élements qu'on souhaite rendre est définit dans un objet qu'on retourne : 
```javaScript
export default { 
    data() {
        return {
            message: "Hello Wordl!"
        }
    }
}
 ```

 ```message``` peut être par la suite être affiché dynamiquement dans des élements HTML grâce a cette synthaxe : 

 ```html
<h1>{{message }}</h2>
 ```
On a la possibilité d'utiliser des expression propre a javaScript dans ces doubles accolades 

Exemple : 

```javaScript
<script>
export default {
  data() {
    return {
      message: 'Hello World!',
      counter: {
        count: 0
      }
    }
  }
}
</script>

<template>
  <h1>{{ message }}</h1>
  <p>Count is: {{ counter.count }}</p>
</template>
```

# Attribute Bindings 

Les doubles accolades sont utilisé uniquement pour l'implémentation de texts. Pour y arribué un valeur dynamique on utilise la directive ```v-bind``` 
```<div v-bind:id="dynamicId"></div>```
raccourcie utilisé ```:``` ```<div :id="dynamicId"></div>```

exemple : 

```javaScript
<script>
export default {
  data() {
    return {
      titleClass: 'title'
    }
  }
}
</script>

<template>
  <h1 :class="titleClass">Make me red</h1>
</template>

<style>
.title {
  color: red;
}
</style>
```

L'élément ``<h1>`` aura la classe CSS "title", ce qui le fera apparaître en rouge en raison de la règle de style définie dans la section ``<style>``. La valeur initiale de titleClass peut être modifiée dynamiquement en fonction de la logique de l'application.

# Event Listerners 

On peut ajouter des ecouteurs d'évenements en utilisant la directive ``v-on``

```javaScript
<button v-on:click="increment">{{ count }}</button>

raccouri : 

<button @click="increment">{{ count }}</button>
```

exemple : 

```javaScript 
<script>
export default {
  data() {
    return {
      count: 0
    }
  },
  methods: {
    increment() {
      this.count++
    }
  }
}
</script>

<template>
  <button @click="increment">count is: {{ count }}</button>
</template>
```
# Form Bindings 

Ils est possible d'utiliser les ``v-bond`` et les ``v-on`` ensemble : 

```html
<input :value="text" @input="onInput">
```

Le directive v-model permet de faire les deux à la fois, mais le v-model ne fonctionne uniquement sur des inputs.

```html
<input v-model="text">
```

# Conditional Rendering 

le directive ``v-if`` et ``v-else`` permet de faire des rendu enfonction des conditions. 

par exemple : 

```html
<h1 v-if="awesome">Vue is awesome!</h1>
<h1 v-else>Oh no 😢</h1>
```

Ici, si awesome est vrai, alors on affiche "Vue is awsome"
sinon "Oh no"

# List Rendering (Boucles)

``v-for`` est un directive utilisé pour rendre une liste d'élements basé sur des données issue d'un array. 

```javaScript
<ul>
  <li v-for="todo in todos" :key="todo.id">
    {{ todo.text }}
  </li>
</ul>
```

# Computed Property 

Avec ``computed property`` nous pouvons déclarer une propriété calculée de manière réactive à partir d'autres propriétés en utilisant l'option ``computed`` 
Une propriété calculée suit d'autres état  utilisé dans computation en tant que dépendances. Le résultat est mis en cach et mis a jours de façon automatique quand les dépendances changent.

```javaScript
export default {
  // ...
  computed: {
    filteredTodos() {
      // return filtered todos based on `this.hideCompleted`
    }
  }
}
```
Généralement on peut boucler dessus afin d'avoir les mis-à-jours affiché lors de changement de l'état

# Lifecycle and Template Refs 

On peut cibler un élement dans un template (le DOM) en utilisant l'attribut spécial ``ref``.

```html
<p ref="pElementRef">hello</p>
```

Mais on ne peut avoir accès qu'après que le composant est monté. C'est ce qu'on appel un *lifecycle hook* = ça donne la possiblité, d'enregistrer un callback afin qu'il soit appeller a un certains moment dans la cycle de vie du composant.

# Watchers 

Parfois nous avons besion de voir les effets d'un changement, on peut faire ça grâce aux *watchers*

```javaScript
watch: {
    count(newCount) {
      // yes, console.log() is a side effect
      console.log(`new count is: ${newCount}`)
    }
  }
```

# Components 

A l'image de react 

```javaScript
import ChildComp from './ChildComp.vue'

export default {
  components: {
    ChildComp
  }
}

<ChildComp />
```

# Props 

```javaScript
<script>
import ChildComp from './ChildComp.vue'

export default {
  components: {
    ChildComp
  },
  data() {
    return {
      greeting: 'Hello from parent'
    }
  }
}
</script>

<template>
  <ChildComp :msg="greeting" />
</template>
```
# Emits

Un composant enfant peut également emettre des evenement a son parent 
On met ca dans l'enfant 
```javaScript
export default {
  // declare emitted events
  emits: ['response'],
  created() {
    // emit with argument
    this.$emit('response', 'hello from child')
  }
}
```

Dans le parent : 
```javaScript
<ChildComp @response="(msg) => childMsg = msg" />
```

# Slots

Le parent peut également passer des fragmement de template a l'enfant via des *slots*

Dans le composant mère :

```javaScript
<script>
import ChildComp from './ChildComp.vue'

export default {
  components: {
    ChildComp
  },
  data() {
    return {
      msg: 'from parent'
    }
  }
}
</script>

<template>
  <ChildComp>Hello worldo ! 'On passe le fragmement ici'</ChildComp>
</template>
```

Dans le composant enfant : 

```javaScript
<slot>Fallback content</slot>
```