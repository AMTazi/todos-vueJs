<template>
   <section class="todoapp">
       <header class="header">
           <h1>todos</h1>
           <label for="task_input"></label>
           <input id="task_input" type="text" class="new-todo" v-model="new_task" @keyup.enter="addValue">
       </header>
       <section class="main">
         <input type="checkbox" class="toggle-all" v-model="toggle_all">
           <ul class="todo-list">
               <li class="todo" v-for="task in select" :class="{completed:task.checked,editing: task === editing}">
                   <div class="view">
                       <input type="checkbox" class="toggle" v-model="task.checked">
                       <label @dblclick="edit(task)">{{ task.name }}</label>
                       <button class="destroy" @click.prevent="deleteTask(task)"></button>
                   </div>
                   <input type="text" class="edit" v-model="task.name" @keyup.enter="editThis(task.name)" @blur="editThis(task.name)" @keyup.esc="cancelEdit()" v-focus="task === editing">
               </li>
           </ul> 
       </section>
       <footer class="footer" v-if="hide > 0">
           <span class="todo-count"><strong>{{ count }}</strong> item left</span>
           <ul class="filters">
               <li>
                   <a href="#all"  :class="{selected: filter === '#all'}" @click.prevent="filter = '#all'">All</a>
               </li>
               <li>
                   <a href="#active" :class="{selected: filter === '#active'}" @click.prevent="filter = '#active'">Active</a>
               </li>
               <li>
                   <a href="#completed" :class="{selected: filter === '#completed'}" @click.prevent="filter = '#completed'">Completed</a>
               </li>
           </ul>
           <button class="clear-completed" @click.prevent="clearCompleted">Clear completed</button>
       </footer>
   </section>
</template>
<!--
- Step 1
- get value from input
- store it in array (tasks)
- display tasks[] items
- Step 2
.
.


-->
<script>
import Vue from 'vue'
export default {
  name: 'todos',
  data () {
    return {
      msg: 'hello world',
      tasks: [],
      new_task: '',
      filter: window.location.hash ? window.location.hash : '#all',
      editing: null,
      oldtodo: ''
    }
  },
  methods: {
    addValue () {
      if (this.new_task !== '') {
        this.tasks.push({name: this.new_task, checked: false})
        this.new_task = ''
      }
    },
    deleteTask (task) {
      this.tasks = this.tasks.filter(function (t) {
        return t !== task
      })
    },
    clearCompleted () {
      this.tasks = this.tasks.filter(function (task) {
        return !task.checked
      })
    },
    edit (task) {
      this.editing = task
      this.oldtodo = task.name
    },
    editThis (task) {
      this.editing = null
      return this.tasks.filter(function (t) {
        t.name = task
      })
    },
    cancelEdit () {
      this.editing.name = this.oldtodo
      this.editing = null
    }
  },
  computed: {
    count () {
      return this.tasks.filter(function (task) {
        return !task.checked
      }).length
    },
    hide () {
      return this.tasks.filter(function (task) {
        return task
      }).length
    },
    toggle_all: {
      get () {
        return this.count === 0
      },
      set (value) {
        this.tasks.forEach(task => { task.checked = value })
      }
    },
    select () {
      if (this.filter === '#active') {
        return this.tasks.filter(function (task) {
          return !task.checked
        })
      } else if (this.filter === '#completed') {
        return this.tasks.filter(function (task) {
          return task.checked
        })
      }
      return this.tasks
    }
  },
  directives: {
    focus (el, val) {
      if (val) {
        Vue.nextTick(_ => {
          el.focus()
        })
      }
    }
  }
}

</script>

<style src="../assets/style.css"></style>
