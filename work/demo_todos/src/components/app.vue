<template>
  <div class="todo-container">
    <div class="todo-wrap">
      <todo-header @add="addTodo"></todo-header>
      <list :todos="todos"></list>
      <todo-footer :todos="todos" :remove-complete-todos="removeCompleteTodos" :check-all-todos="checkAllTodos"></todo-footer>
    </div>
  </div>
</template>

<script>
  import header from './header.vue'
  import list from './list.vue'
  import footer from './footer.vue'
  import storageUtils from '../util/storageUtils'

  export default {
    data () {
      return { // 如果没有返回[]
        todos: storageUtils.getTodos()
      }
    },

    methods: {
      addTodo (todo) {
        this.todos.unshift(todo)
      },

      removeCompleteTodos () {
        this.todos = this.todos.filter(function (todo) {
          return !todo.complete
        })
      },
      checkAllTodos (check) {
        this.todos.forEach(function (todo) {
          todo.complete = check
        })
      }
    },

    watch: {
      todos: {
        deep: true, // 深度监视
        /* handler: function (newValue) {
          //保存到localStorage ctrl + shift + y
          storageUtils.saveTodos(newValue)
        } */
        handler: storageUtils.saveTodos
      }
    },

    events: {//  注册自定义事件监听
      remove: function (index) {
        this.todos.splice(index, 1)
      }
    },
    components: {
      'todo-header': header,
      list,
      'todo-footer': footer
    }
  }
</script>

<style>
  .todo-container {
    width: 600px;
    margin: 0 auto;
  }
  .todo-container .todo-wrap {
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 5px;
  }
</style>
