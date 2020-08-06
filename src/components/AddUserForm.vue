<template>
    <b-modal ref="addUserModal" id="add-user-modal" title="Add a new user" hide-footer>
        <b-form @submit.prevent="onSubmit" @reset.prevent="onReset" class="w-100" v-if="!isSuccess">

            <b-form-group id="form-name-group" label="Username:" label-for="form-name-input">
                <b-form-input id="form-name-input" type="text" v-model="addUserForm.username"  placeholder="Enter name"/>
            </b-form-group>
            <span class="error">{{errors.username}}</span>

            <b-form-group id="form-email-group" label="Email:" label-for="form-email-input">
                <b-form-input id="form-email-input" type="text" v-model="addUserForm.email"  placeholder="Enter email"/>
            </b-form-group>
            <span class="error">{{errors.email}}</span>

            <hr>
            <h5>Address:</h5>
            <b-form-group id="form-city-group" label="City:" label-for="form-city-input">
                <b-form-input id="form-city-input" type="text" v-model="addUserForm.address.city"  placeholder="Enter city"/>
            </b-form-group>

            <b-form-group id="form-street-group" label="Street:" label-for="form-street-input">
                <b-form-input id="form-street-input" type="text" v-model="addUserForm.address.street"  placeholder="Enter street"/>
            </b-form-group>

            <b-form-group id="form-block-group" label="Suite:" label-for="form-block-input">
                <b-form-input id="form-block-input" type="text" v-model="addUserForm.address.suite"  placeholder="Enter suite"/>
            </b-form-group>

            <b-button type="submit" variant="primary" >Submit</b-button>
            <b-button type="reset" variant="danger">Reset</b-button>

        </b-form>
        <div v-else>
            <h4 class="success_header">{{successMessage}}</h4>
        </div>
    </b-modal>
</template>
<script>
export default {
  props: {
    getUsers: {
      type: Function
    }
  },
  data () {
    return {
      isSuccess: false,
      errors: {
        email: '',
        username: ''
      },
      successMessage: '',
      addUserForm: {
        username: '',
        email: '',
        address: {
          city: '',
          street: '',
          suite: ''
        }
      }
    }
  },
  methods: {
    addUser (data) {
      this.$http
        .post(this.$API_URL, JSON.stringify(data))
        .then(res => {
          this.getUsers()
          this.isSuccess = true
          this.successMessage = 'User was created!'
          setTimeout(() => {
            this.resetForm()
          }, 1000)
        })
        .catch(error => {
          console.log(error)
        })
    },
    resetForm () {
      // Reset form field
      this.addUserForm.username = ''
      this.addUserForm.email = ''
      this.addUserForm.address = {
        city: '',
        street: '',
        suite: ''
      }
      // reset Success message and state
      this.isSuccess = false
      this.successMessage = ''
      // Hide form
      this.$refs.addUserModal.hide()
    },
    onSubmit () {
      const data = this.addUserForm
      if (this.checkForm()) {
        this.addUser(data)
      }
    },
    onReset () {
      this.resetForm()
    },
    isEmailCorrect (email) {
      // validation using regex
      const re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
      return re.test(email)
    },
    checkForm () {
      // if email not empty and valid
      if (this.addUserForm.email && this.addUserForm.username && this.isEmailCorrect(this.addUserForm.email)) {
        this.errors = []
        return true
      }
      // if email empty
      if (!this.addUserForm.email) {
        this.errors = []
        this.errors.email = 'The field cannot be empty.'
        return false
      }
      // if name empty
      if (!this.addUserForm.username) {
        this.errors = []
        this.errors.username = 'The field cannot be empty.'
        return false
      }
      // if email not valid
      if (!this.isEmailCorrect(this.addUserForm.email)) {
        this.errors = []
        this.errors.email = 'Please enter a valid email address'
        return false
      }
    }
  }

}
</script>
<style lang="css">
    .success_header{
        color: #28A745;
        text-align: center;
    }
    .error{
      font-size: 12px;
      color: #C82333;
    }
</style>
