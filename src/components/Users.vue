<template>
  <div class="container">
    <div class="row">
      <div class="col-sm-12">
        <h1>Users</h1>
        <hr><br><br>
        <button type="button" class="btn btn-success btn-sm" v-b-modal.add-user-modal>Add New</button>
        <br><br>
        <table class="table table-hover">
          <thead>
            <tr>
              <th scope="col">Name</th>
              <th scope="col">Username</th>
              <th scope="col">Email</th>
              <th></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(user, index) in users" :key="index">
              <td>{{ user.name }}</td>
              <td>{{ user.username }}</td>
              <td> <a :href="`mailto:${user.email}`">{{ user.email }}</a> </td>
              <td>
                <button type="button" class="btn btn-warning btn-sm"><b-icon-pencil style="color: #fff;"/></button>
                <button type="button" class="btn btn-danger btn-sm"><b-icon-trash/></button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <b-modal ref="addUserModal" id="add-user-modal" title="Add a new user" hide-footer>
        <b-form @submit.prevent="onSubmit" @reset.prevent="onReset" class="w-100" v-if="!isSuccess">

            <b-form-group id="form-name-group" label="Username:" label-for="form-name-input">
                <b-form-input id="form-name-input" type="text" v-model="addUserForm.username" required placeholder="Enter name"/>
            </b-form-group>

            <b-form-group id="form-email-group" label="Email:" label-for="form-email-input">
                <b-form-input id="form-email-input" type="text" v-model="addUserForm.email" required placeholder="Enter email"/>
            </b-form-group>

            <hr>
            <h5>Address:</h5>
            <b-form-group id="form-city-group" label="City:" label-for="form-city-input">
                <b-form-input id="form-city-input" type="text" v-model="addUserForm.address.city" required placeholder="Enter city"/>
            </b-form-group>

            <b-form-group id="form-street-group" label="Street:" label-for="form-street-input">
                <b-form-input id="form-street-input" type="text" v-model="addUserForm.address.street" required placeholder="Enter street"/>
            </b-form-group>

            <b-form-group id="form-block-group" label="Block:" label-for="form-block-input">
                <b-form-input id="form-block-input" type="text" v-model="addUserForm.address.block" required placeholder="Enter block"/>
            </b-form-group>

            <b-button type="submit" variant="primary">Submit</b-button>
            <b-button type="reset" variant="danger">Reset</b-button>

        </b-form>
        <div v-else>
            <h4 class="success_header">{{successMessage}}</h4>
        </div>
    </b-modal>
  </div>
</template>
<script>
import { BIconPencil, BIconTrash } from 'bootstrap-vue'
export default {
  data () {
    return {
      users: [],
      isSuccess: false,
      successMessage: '',
      addUserForm: {
        username: '',
        email: '',
        address: {
          city: '',
          street: '',
          block: ''
        }
      }
    }
  },
  components: {
    BIconPencil,
    BIconTrash
  },
  methods: {
    getUsers () {
      this.$http
        .get(this.$API_URL)
        .then(res => {
          this.users = res.data
        })
        .catch(error => {
          console.log(error)
        })
    },
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
        block: ''
      }
      // reset Success message and state
      this.isSuccess = false
      this.successMessage = ''
      // Hide form
      this.$refs.addUserModal.hide()
    },
    onSubmit () {
      const data = this.addUserForm
      this.addUser(data)
    },
    onReset () {
      this.resetForm()
    }
  },
  created () {
    this.getUsers()
  }
}
</script>
<style lang="css">
    .success_header{
        color: #28A745;
        text-align: center;
    }
</style>
