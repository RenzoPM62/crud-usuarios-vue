<template>
  <div class="container mt-4">
    <h1 class="mb-4 text-center">CRUD de Usuarios</h1>

    <button class="btn btn-primary mb-3" @click="openNewUser">
      Agregar Usuario
    </button>

    <!-- Modal -->
    <div class="modal fade" id="userModal" tabindex="-1">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">
              {{ form.id ? 'Editar Usuario' : 'Agregar Usuario' }}
            </h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
          </div>

          <div class="modal-body">
            <UserForm :form="form" @submit="saveUser" />
          </div>
        </div>
      </div>
    </div>

    <Loader v-if="loading" />

    <UserTable v-else :users="users" @edit="editUser" @delete="deleteUser" />
  </div>
</template>

<script>
import { Modal } from 'bootstrap'
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
    openNewUser() {
      this.resetForm()
      this.showModal()
    },
    editUser(user) {
      this.form = { ...user }
      this.showModal()
    },
    showModal() {
      const modal = new Modal(document.getElementById('userModal'))
      modal.show()
    },
    hideModal() {
      const modalEl = document.getElementById('userModal')
      const modal = Modal.getInstance(modalEl)
      modal.hide()
    },
    saveUser(user) {
      if (user.id) {
        const index = this.users.findIndex(u => u.id === user.id)
        this.users[index] = user
      } else {
        const newId = Math.max(...this.users.map(u => u.id)) + 1
        this.users.push({ ...user, id: newId })
      }
      this.resetForm()
      this.hideModal()
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
