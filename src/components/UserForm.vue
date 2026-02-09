<template>
    <div class="mb-2">
        <input v-model="localForm.name" class="form-control" placeholder="Nombre" required />
    </div>
    <div class="mb-2">
        <input v-model="localForm.username" class="form-control" placeholder="Usuario" required />
    </div>
    <div class="mb-2">
        <input v-model="localForm.email" type="email" class="form-control" placeholder="Email" required />
    </div>
    <div class="mb-2">
        <input v-model="localForm.phone" class="form-control" placeholder="Teléfono" required />
    </div>

    <button type="submit" class="btn btn-success w-100" @click="submitForm">
        {{ localForm.id ? 'Actualizar' : 'Guardar' }}
    </button>
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
