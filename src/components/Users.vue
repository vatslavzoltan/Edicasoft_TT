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
                <button type="button" @click.prevent='setUserEdit(user)' v-b-modal.edit-user-modal class="btn btn-warning btn-sm"><b-icon-pencil style="color: #fff;"/></button>
                <button type="button" class="btn btn-danger btn-sm"><b-icon-trash/></button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <AddUserForm :getUsers='getUsers' />

    <EditUserForm :user='userToEdit' :getUsers='getUsers' />

  </div>
</template>
<script>
import { BIconPencil, BIconTrash } from 'bootstrap-vue'
import AddUserForm from './AddUserForm'
import EditUserForm from './EditUserForm'
export default {
  data () {
    return {
      users: [],
      userToEdit: {
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
    BIconTrash,
    AddUserForm,
    EditUserForm
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
    setUserEdit (user) {
      this.userToEdit = user
    }
  },
  created () {
    this.getUsers()
  }
}
</script>
