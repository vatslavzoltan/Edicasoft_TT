<template>
    <b-modal ref="editUserModal" id="edit-user-modal" title="Edit user" hide-footer>
        <b-form @submit.prevent="onSubmit" class="w-100" v-if="!isSuccess">

            <b-form-group id="form-name-group" label="Username:" label-for="form-name-input">
                <b-form-input id="form-name-input" type="text" v-model="user.username" required placeholder="Enter name"/>
            </b-form-group>

            <b-form-group id="form-email-group" label="Email:" label-for="form-email-input">
                <b-form-input id="form-email-input" type="text" v-model="user.email" required placeholder="Enter email"/>
            </b-form-group>

            <hr>
            <h5>Address:</h5>
            <b-form-group id="form-city-group" label="City:" label-for="form-city-input">
                <b-form-input id="form-city-input" type="text" v-model="user.address.city" required placeholder="Enter city"/>
            </b-form-group>

            <b-form-group id="form-street-group" label="Street:" label-for="form-street-input">
                <b-form-input id="form-street-input" type="text" v-model="user.address.street" required placeholder="Enter street"/>
            </b-form-group>

            <b-form-group id="form-block-group" label="Suite:" label-for="form-block-input">
                <b-form-input id="form-block-input" type="text" v-model="user.address.suite" required placeholder="Enter suite"/>
            </b-form-group>

            <b-button type="submit" variant="primary">Submit</b-button>

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
    updateUser (data) {
      this.$http
        .put(`${this.$API_URL}/${data.id}`, JSON.stringify(data))
        .then(res => {
          this.getUsers()
          this.isSuccess = true
          this.successMessage = 'User was updated!'
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
      this.$refs.editUserModal.hide()
    },
    onSubmit () {
      const data = this.user
      this.updateUser(data)
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
