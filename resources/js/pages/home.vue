<template>
  <card>

    <div class="w-100 h-100 d-flex flex-column justify-content-center align-items-center">
      <h1 class="text-black-50 display-6">To Do List</h1>
    </div>
    <form>
      <div class="input-group mb-3">
        <input type="text" class="form-control form-control-lg" placeholder="Add something your todo List" aria-label="arialabel" aria-describedby="button-addon2">
        <div class="input-group-append">
          <button class="btn btn-success" type="submit" id="button-addon2">Add ToDo</button>
        </div>
      </div>
    </form>

  </card>
</template>

<script>
export default {
  middleware: 'auth',

  data(){
    return{

    }
  },
  mounted() {
  },

  metaInfo () {
    return { title: this.$t('home') }
  }
}
</script>
