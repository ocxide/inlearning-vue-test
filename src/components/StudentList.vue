<template>
    <div class="container mx-auto p-4">
      <h1 class="text-2xl font-bold mb-4">Lista de Estudiantes</h1>
      <div class="mb-4">
        <label for="student-select" class="block mb-2 font-bold">Selecciona un Estudiante:</label>
        <select
          id="student-select"
          v-model="selectedStudentId"
          class="border rounded p-2 w-full"
        >
          <option value="" disabled>-- Selecciona un estudiante --</option>
          <option v-for="student in students" :key="student.id" :value="student.id">
            {{ student.nombres }} {{ student.apellidos }} ({{ student.codigo }})
          </option>
        </select>
      </div>
      <button
        @click="addSelectedStudent"
        class="mt-4 bg-indigo-500 text-white py-2 px-4 rounded"
      >
        Agregar Estudiante Seleccionado
      </button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        students: [],
        selectedStudentId: null,
      };
    },
    methods: {
      async fetchStudents() {
        try {
          const response = await fetch('https://662aa18dd3f63c12f4583be5.mockapi.io/api/pt/alumnos');
          if (!response.ok) {
            throw new Error('Error al obtener los estudiantes');
          }
          this.students = await response.json();
        } catch (error) {
          console.error('Error al obtener estudiantes:', error);
        }
      },
      addSelectedStudent() {
        const selectedStudent = this.students.find(student => student.id === this.selectedStudentId);
        if (selectedStudent) {
          this.$emit('student-selected', selectedStudent);
          // Optional: reset the selected student
          this.selectedStudentId = null;
        } else {
          alert('Por favor, selecciona un estudiante.');
        }
      },
    },
    mounted() {
      this.fetchStudents();
    },
  };
  </script>
  
  <style scoped>
  .container {
    max-width: 800px;
  }
  </style>