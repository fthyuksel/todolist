<template>
  <card>
    <div class="w-100 h-100 d-flex flex-column justify-content-center align-items-center">
      <h1 class="text-black-50 display-6">
        To Do List
      </h1>
    </div>
    <form @submit.prevent="saveData">
      <div class="input-group mb-3">
        <input v-model="form.title" type="text" class="form-control form-control-lg" placeholder="Add something your todo List" aria-label="arialabel" aria-describedby="button-addon2">
        <div class="input-group-append">
          <button id="button-addon2" class="btn btn-success" type="submit">
            Add ToDo
          </button>
        </div>
      </div>
    </form>
  </card>
</template>

<script>
import Form from '../Form'

window.axios = require('axios');


export default {
  middleware: 'auth',

  data () {
    return {
      form: new Form({
        title: ''
      })
    }
  },
  mounted () {
  },
  methods: {
    saveData () {
      const data = new FormData()
      data.append('title', this.form.title)
      axios.post('/api/todo',data)
    }
  },

  metaInfo () {
    return { title: this.$t('home') }
  }
}
</script>
