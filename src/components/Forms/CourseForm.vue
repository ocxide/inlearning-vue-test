<template>
    <div class="container mx-auto p-4">
      <h1 class="text-2xl font-bold mb-4">Formulario de Curso</h1>
      <form @submit.prevent="onSubmit">
        <div class="mb-4">
          <label class="block text-gray-700">Nombre del curso</label>
          <input
            v-model="course.nombre"
            type="text"
            class="border rounded w-full py-2 px-3"
            :class="{'border-red-500': errors.nombre}"
            placeholder="Ingrese el nombre del curso"
          />
          <span v-if="errors.nombre" class="text-red-500">{{ errors.nombre }}</span>
        </div>
  
        <div class="mb-4">
          <label class="block text-gray-700">Categoría</label>
          <select
            v-model="course.categoria"
            multiple
            class="border rounded w-full py-2 px-3"
          >
            <option value="programacion">Programación</option>
            <option value="diseño">Diseño</option>
            <option value="marketing">Marketing</option>
            <option value="negocios">Negocios</option>
          </select>
          <span v-if="errors.categoria" class="text-red-500">{{ errors.categoria }}</span>
        </div>
  
        <div class="mb-4">
          <label class="block text-gray-700">Descripción</label>
          <textarea
            v-model="course.descripcion"
            class="border rounded w-full py-2 px-3"
            :class="{'border-red-500': errors.descripcion}"
            placeholder="Ingrese una descripción detallada del curso"
          ></textarea>
          <span v-if="errors.descripcion" class="text-red-500">{{ errors.descripcion }}</span>
        </div>
  
        <div class="mb-4">
          <label class="block text-gray-700">Cantidad de alumnos</label>
          <input
            v-model.number="course.capacidad"
            type="number"
            class="border rounded w-full py-2 px-3"
            :class="{'border-red-500': errors.capacidad}"
            placeholder="Ingrese la capacidad de alumnos"
          />
          <span v-if="errors.capacidad" class="text-red-500">{{ errors.capacidad }}</span>
        </div>
  
        <div class="mb-4">
          <label class="block text-gray-700">Fecha de inicio</label>
          <input
            v-model="course.inicio"
            type="date"
            class="border rounded w-full py-2 px-3"
            :class="{'border-red-500': errors.inicio}"
          />
          <span v-if="errors.inicio" class="text-red-500">{{ errors.inicio }}</span>
        </div>
  
        <div class="mb-4">
          <label class="block text-gray-700">Fecha de fin</label>
          <input
            v-model="course.fin"
            type="date"
            class="border rounded w-full py-2 px-3"
            :class="{'border-red-500': errors.fin}"
          />
          <span v-if="errors.fin" class="text-red-500">{{ errors.fin }}</span>
        </div>
  
        <div class="mb-4">
          <label class="block text-gray-700">Precio</label>
          <input
            v-model.number="course.precio"
            type="number"
            class="border rounded w-full py-2 px-3"
            :class="{'border-red-500': errors.precio}"
            placeholder="Ingrese el precio del curso"
          />
          <span v-if="errors.precio" class="text-red-500">{{ errors.precio }}</span>
        </div>
  
        <div class="flex justify-between">
          <button
            type="submit"
            class="bg-indigo-500 text-white py-2 px-4 rounded"
          >
            Crear Curso
          </button>
          <button
            type="button"
            @click="resetForm"
            class="bg-gray-500 text-white py-2 px-4 rounded"
          >
            Resetear
          </button>
        </div>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        course: {
          nombre: '',
          categoria: [],
          descripcion: '',
          capacidad: null,
          inicio: '',
          fin: '',
          precio: null,
        },
        errors: {
          nombre: '',
          categoria: '',
          descripcion: '',
          capacidad: '',
          inicio: '',
          fin: '',
          precio: '',
        },
      };
    },
    methods: {
      validate() {
        // Reset errors
        this.errors = {
          nombre: '',
          categoria: '',
          descripcion: '',
          capacidad: '',
          inicio: '',
          fin: '',
          precio: '',
        };
        let isValid = true;
  
        // Validate course name
        if (!this.course.nombre) {
          this.errors.nombre = 'El nombre del curso es requerido.';
          isValid = false;
        }
  
        // Validate category
        if (this.course.categoria.length === 0) {
          this.errors.categoria = 'Debe seleccionar al menos una categoría.';
          isValid = false;
        }
  
        // Validate description
        if (!this.course.descripcion) {
          this.errors.descripcion = 'La descripción es requerida.';
          isValid = false;
        }
  
        // Validate capacity
        if (this.course.capacidad === null || this.course.capacidad <= 0) {
          this.errors.capacidad = 'La capacidad debe ser un número positivo.';
          isValid = false;
        }
  
        // Validate start date
        if (!this.course.inicio) {
          this.errors.inicio = 'La fecha de inicio es requerida.';
          isValid = false;
        }
  
        // Validate end date
        if (!this.course.fin) {
          this.errors.fin = 'La fecha de fin es requerida.';
          isValid = false;
        }
  
        // Validate price
        if (this.course.precio === null || this.course.precio < 0) {
          this.errors.precio = 'El precio debe ser un número positivo.';
          isValid = false;
        }
  
        return isValid;
      },
      async onSubmit() {
        if (this.validate()) {
          try {
            const response = await fetch('https://662aa18dd3f63c12f4583be5.mockapi.io/api/pt/cursos', {
              method: 'POST',
              headers: {
                'Content-Type': 'application/json',
              },
              body: JSON.stringify(this.course),
            });
  
            if (!response.ok) {
              throw new Error('Error al crear el curso');
            }
  
            const saved = await response.json();
            this.$emit('saved', saved);
            this.resetForm();
          } catch (error) {
            console.error('Error al crear el curso:', error);
            // Manejar el error apropiadamente (ejemplo: mostrar un mensaje de error)
          }
        }
      },
      resetForm() {
        // Reset course data
        this.course = {
          nombre: '',
          categoria: [],
          descripcion: '',
          capacidad: null,
          inicio: '',
          fin: '',
          precio: null,
        };
        // Reset errors
        this.errors = {
          nombre: '',
          categoria: '',
          descripcion: '',
          capacidad: '',
          inicio: '',
          fin: '',
          precio: '',
        };
      },
    },
  };
  </script>
  
  <style scoped>
  .container {
    max-width: 600px;
  }
  </style>
  