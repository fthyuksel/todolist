<template>
  <card>
    <div class="w-100 h-100 d-flex flex-column justify-content-center align-items-center">
      <h1 class="text-black-50 display-6">
        To Do List
      </h1>
    </div>
    <form @submit.prevent="saveData">
      <div class="input-group mb-3">
        <input v-model="form.title" :class="{'is-invalid' : form.errors.has('title')}" type="text" class="form-control form-control-lg"
               @keydown="form.errors.clear('title')" placeholder="Add something your todo List" aria-label="arialabel" aria-describedby="button-addon2">
        <div class="input-group-append">
          <button id="button-addon2" class="btn btn-success" type="submit">
            Add ToDo
          </button>
        </div>
      </div>
      <span class="text-danger pt-3 pb-3" v-if="form.errors.has('title')" v-text="form.errors.get('title')"></span>
    </form>
    <div class="w-25">
      <div v-for="todo in todos" :key="todo.id" class="w-100">{{todo.title}}</div>
    </div>
  </card>
</template>

<script>
import Form from '../Form'

window.axios = require('axios')

export default {
  middleware: 'auth',

  data () {
    return {
      todos:'',
      form: new Form({
        title: ''
      })
    }
  },
  methods: {
    getTodos(){
      axios.get('/api/todo').then((res) => {
        this.todos = res.data
      }).catch((error) => {
        console.log(error)
      })
    },
    saveData () {
      const data = new FormData()
      data.append('title', this.form.title)
      axios.post('/api/todo', data).then((res) =>{
        this.form.reset()
        this.getTodos()
      }).catch((error) => {
        this.form.errors.record(error.response.data.errors)
      })
    }
  },

  mounted() {
    this.getTodos()
  },

  metaInfo () {
    return { title: this.$t('home') }
  }
}
</script>
