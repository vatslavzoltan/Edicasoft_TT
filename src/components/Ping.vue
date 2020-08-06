<template>
  <div class="container">
    <button type="button" class="btn btn-primary">{{ msg }}</button>
  </div>
</template>

<script>
export default {
  name: 'Ping',
  data () {
    return {
      msg: 'Hello!'
    }
  },
  methods: {
    Users () {
      this.$http
        .get(this.$API_URL)
        .then(response => {
          console.log('getMessage -> response', response)
        })
        .catch(error => {
          console.log(error)
        })
    }
  },
  created () {
    this.Users()
  }
}
</script>
