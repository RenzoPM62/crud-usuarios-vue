<template>
  <div class="container">
    <h1>CRUD de Usuarios</h1>

    <UserForm :form="form" @submit="saveUser" />

    <Loader v-if="loading" />

    <UserTable
      v-else
      :users="users"
      @edit="editUser"
      @delete="deleteUser"
    />
  </div>
</template>

<script>
import Loader from './components/Loader.vue'
import UserTable from './components/UserTable.vue'
import UserForm from './components/UserForm.vue'

export default {
  components: { Loader, UserTable, UserForm },
  data() {
    return {
      users: [],
      loading: true,
      form: {
        id: null,
        name: '',
        username: '',
        email: '',
        phone: ''
      }
    }
  },
  mounted() {
    fetch('https://jsonplaceholder.typicode.com/users')
      .then(res => res.json())
      .then(data => {
        this.users = data
        this.loading = false
      })
  },
  methods: {
    saveUser(user) {
      if (user.id) {
        const index = this.users.findIndex(u => u.id === user.id)
        this.users[index] = user
      } else {
        const newId = Math.max(...this.users.map(u => u.id)) + 1
        this.users.push({ ...user, id: newId })
      }
      this.resetForm()
    },
    editUser(user) {
      this.form = { ...user }
    },
    deleteUser(id) {
      if (confirm('¿Eliminar usuario?')) {
        this.users = this.users.filter(u => u.id !== id)
      }
    },
    resetForm() {
      this.form = { id: null, name: '', username: '', email: '', phone: '' }
    }
  }
}
</script>

