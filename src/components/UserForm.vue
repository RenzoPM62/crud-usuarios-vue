<template>
  <form @submit.prevent="submitForm">
    <input v-model="localForm.name" placeholder="Nombre" required />
    <input v-model="localForm.username" placeholder="Usuario" required />
    <input v-model="localForm.email" type="email" placeholder="Email" required />
    <input v-model="localForm.phone" placeholder="Teléfono" required />

    <button type="submit">
      {{ localForm.id ? 'Actualizar' : 'Guardar' }}
    </button>
  </form>
</template>

<script>
export default {
  props: {
    form: Object
  },
  data() {
    return {
      localForm: { ...this.form }
    }
  },
  watch: {
    form(newValue) {
      this.localForm = { ...newValue }
    }
  },
  methods: {
    submitForm() {
      this.$emit('submit', this.localForm)
    }
  }
}
</script>
