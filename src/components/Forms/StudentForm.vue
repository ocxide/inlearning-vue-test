<template>
    <div class="container mx-auto p-4">
        <h1 class="text-2xl font-bold mb-4">Formulario de Usuario</h1>
        <form @submit.prevent="onSubmit">
            <div class="mb-4">
                <label class="block text-gray-700">Nombres</label>
                <input v-model="user.nombres" type="text" class="border rounded w-full py-2 px-3"
                    :class="{ 'border-red-500': errors.nombres }" placeholder="Ingrese sus nombres" />
                <span v-if="errors.nombres" class="text-red-500">{{ errors.nombres }}</span>
            </div>

            <div class="mb-4">
                <label class="block text-gray-700">Apellidos</label>
                <input v-model="user.apellidos" type="text" class="border rounded w-full py-2 px-3"
                    :class="{ 'border-red-500': errors.apellidos }" placeholder="Ingrese sus apellidos" />
                <span v-if="errors.apellidos" class="text-red-500">{{ errors.apellidos }}</span>
            </div>

            <div class="mb-4">
                <label class="block text-gray-700">Email</label>
                <input v-model="user.email" type="email" class="border rounded w-full py-2 px-3"
                    :class="{ 'border-red-500': errors.email }" placeholder="Ingrese su email" />
                <span v-if="errors.email" class="text-red-500">{{ errors.email }}</span>
            </div>

            <div class="mb-4">
                <label class="block text-gray-700">Código del alumno</label>
                <input v-model="user.codigo" type="text" class="border rounded w-full py-2 px-3"
                    :class="{ 'border-red-500': errors.codigo }" placeholder="Ingrese el código del alumno" />
                <span v-if="errors.codigo" class="text-red-500">{{ errors.codigo }}</span>
            </div>

            <button type="submit" class="bg-indigo-500 text-white py-2 px-4 rounded">
                Enviar
            </button>
        </form>
    </div>
</template>

<script>
export default {
    data() {
        return {
            user: {
                nombres: '',
                apellidos: '',
                email: '',
                codigo: '',
            },
            errors: {
                nombres: '',
                apellidos: '',
                email: '',
                codigo: '',
            },
        };
    },
    methods: {
        validate() {
            // Reset errors
            this.errors = {
                nombres: '',
                apellidos: '',
                email: '',
                codigo: '',
            };
            let isValid = true;

            // Validate names
            if (!this.user.nombres) {
                this.errors.nombres = 'El nombre es requerido.';
                isValid = false;
            }

            // Validate last names
            if (!this.user.apellidos) {
                this.errors.apellidos = 'Los apellidos son requeridos.';
                isValid = false;
            }

            // Validate email
            const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
            if (!this.user.email) {
                this.errors.email = 'El email es requerido.';
                isValid = false;
            } else if (!emailPattern.test(this.user.email)) {
                this.errors.email = 'Formato de email inválido.';
                isValid = false;
            }
            else if (!(this.user.email.endsWith('@gmail.com') || this.user.email.endsWith('@outlook.com'))) {
                this.errors.email = 'El email debe ser de Gmail o Outlook.';
                isValid = false;
            }

            // Validate student code
            if (!this.user.codigo) {
                this.errors.codigo = 'El código del alumno es requerido.';
                isValid = false;
            }

            return isValid;
        },
        onSubmit() {
            if (this.validate()) {
                this.$emit('appsubmit', { ...this.user });
                this.resetForm();
            }
        },
        resetForm() {
            // Reset user data
            this.user = {
                nombres: '',
                apellidos: '',
                email: '',
                codigo: '',
            };
            // Reset errors
            this.errors = {
                nombres: '',
                apellidos: '',
                email: '',
                codigo: '',
            };
        },
    }
};
</script>

<style scoped>
.container {
    max-width: 600px;
}
</style>