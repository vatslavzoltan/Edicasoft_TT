<template>
    <b-modal ref="deleteUserModal" id="delete-user-modal" title="Delete user" hide-footer>
        <b-form @submit.prevent="deleteUser" class="w-100" v-if="!isSuccess">

            <h5>Delete user: {{user.username}} ?</h5>
            <br>
            <b-button type="submit" variant="danger">Delete</b-button>
            <b-button @click.prevent="resetForm" variant="primary">Cancel</b-button>

        </b-form>
        <div v-else>
            <h4 class="success_header">{{successMessage}}</h4>
        </div>
    </b-modal>
</template>
<script>
export default {
  props: {
    user: {
      type: Object
    },
    getUsers: {
      type: Function
    }
  },
  data () {
    return {
      isSuccess: false,
      successMessage: ''
    }
  },
  methods: {
    deleteUser () {
      this.$http
        .delete(`${this.$API_URL}/${this.user.id}`)
        .then(res => {
          this.getUsers()
          this.isSuccess = true
          this.successMessage = 'User was deleted!'
          setTimeout(() => {
            this.resetForm()
          }, 1000)
        })
        .catch(error => {
          console.log(error)
        })
    },
    resetForm () {
      // reset Success message and state
      this.isSuccess = false
      this.successMessage = ''
      // Hide form
      this.$refs.deleteUserModal.hide()
    }
  }

}
</script>
<style lang="css">
    .success_header{
        color: #28A745;
        text-align: center;
    }
</style>
