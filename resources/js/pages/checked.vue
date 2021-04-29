<template>


  <card>


    <div class="w-100 h-100 d-flex flex-column justify-content-center align-items-center">
      <h1 class="text-black-50 display-6">
        Checked List
      </h1>
    </div>

    <div class="bg-danger w-100 mb-3 justify-content-center align-items-center">
      <span class="text-white pt-3 pb-3 mt-3 mb-3 ml-3 display-4" v-if="form.errors.has('title')"
            v-text="form.errors.get('title')"></span>
    </div>


    <div class="w-100 todo">
      <div v-for="todo in todos" :key="todo.id" v-if="todo.completed == true"
           class="w-100 d-flex align-items-center p3 bg-white border-bottom">
        <span class="mr-2">
          <svg v-on:click="toogleTodo(todo)" v-if="todo.completed == false" xmlns="http://www.w3.org/2000/svg"
               class="icon icon-tabler icon-tabler-circle-check" width="36" height="36" viewBox="0 0 24 24"
               stroke-width="1.5" stroke="#c9de00" fill="none" stroke-linecap="round" stroke-linejoin="round">
          <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
          <circle cx="12" cy="12" r="9"/>
          <path d="M9 12l2 2l4 -4"/>
        </svg>
          <svg v-if="todo.completed == true" v-on:click="toogleTodo(todo)" xmlns="http://www.w3.org/2000/svg"
               class="icon icon-tabler icon-tabler-arrow-up-left-circle" width="36" height="36" viewBox="0 0 24 24"
               stroke-width="1.5" stroke="#00abfb" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
            <circle cx="12" cy="12" r="9"/>
            <line x1="9" y1="9" x2="15" y2="15"/>
            <polyline points="15 9 9 9 9 15"/>
          </svg>
        </span>

        <div class="font-weight-bolder w-100"><span
          v-if="editmode == false || editmode != todo.id">{{todo.title}}</span>
          <input @keydown="form.errors.clear('title')" class="w-100" v-if="editmode == todo.id" v-model="todo.title"
                 type="text">
        </div>

        <div class="ml-auto mr-2 d-flex align-items-center">

          <span>
            <svg v-on:click="deleteTodo(todo)" xmlns="http://www.w3.org/2000/svg"
                 class="icon icon-tabler icon-tabler-trash" width="36" height="36" viewBox="0 0 24 24"
                 stroke-width="1.5" stroke="#597e8d" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
            <line x1="4" y1="7" x2="20" y2="7"/>
            <line x1="10" y1="11" x2="10" y2="17"/>
            <line x1="14" y1="11" x2="14" y2="17"/>
            <path d="M5 7l1 12a2 2 0 0 0 2 2h8a2 2 0 0 0 2 -2l1 -12"/>
            <path d="M9 7v-3a1 1 0 0 1 1 -1h4a1 1 0 0 1 1 1v3"/>
            </svg>
          </span>
        </div>
      </div>
    </div>
  </card>
</template>

<script>
  import Form from '../Form'

  window.axios = require('axios')

  export default {
    middleware: 'auth',

    data() {
      return {
        editmode: false,
        todos: '',
        form: new Form({
          title: ''
        })
      }
    },
    methods: {
      deleteTodo(e) {
        const data = new FormData();
        data.append('_method', 'DELETE')
        axios.post('/api/todo/' + e.id, data).then((res) => {
          this.todos = res.data
        }).catch((error) => {
          this.form.errors.record(error.response.data.errors)
        })
      },
      toogleTodo(e) {
        e.completed = !e.completed
        const data = new FormData();
        data.append('_method', 'PATCH')
        data.append('title', e.title)
        if (e.completed == true) {
          data.append('completed', 1);
        }
        if (e.completed == false) {
          data.append('completed', 0);
        }
        axios.post('/api/todo/' + e.id, data)
      },
      getTodos() {
        axios.get('/api/todo').then((res) => {
          this.todos = res.data
        }).catch((error) => {
          console.log(error)
        })
      }
    },

    mounted() {
      this.getTodos()
    },

    metaInfo() {
      return {title: this.$t('Checked List')}
    }
  }
</script>
