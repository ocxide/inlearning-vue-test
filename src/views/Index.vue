<template>
  <div class="container mx-auto p-4">
    <h2 class="text-2xl font-bold mb-4">Cursos</h2>

    <div>
      <input type="text" v-model="search">
    </div>

    <table class="min-w-full bg-white shadow-md rounded-lg overflow-hidden">
      <thead>
        <tr>
          <th class="py-2 px-4 bg-gray-200">Nombre del curso</th>
          <th class="py-2 px-4 bg-gray-200">Categoría</th>
          <th class="py-2 px-4 bg-gray-200">Descripción</th>
          <th class="py-2 px-4 bg-gray-200">Cantidad de alumnos</th>
          <th class="py-2 px-4 bg-gray-200">Fecha de inicio</th>
          <th class="py-2 px-4 bg-gray-200">Fecha de fin</th>
          <th class="py-2 px-4 bg-gray-200">Precio</th>
          <th class="py-2 px-4 bg-gray-200">Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="course in coursesfo" :key="course.id">
          <td class="border px-4 py-2">{{ course.nombre }}</td>
          <td class="border px-4 py-2">{{ typeof course.categoria === 'string' ? course.categoria :
            course.categoria.join(', ') }}</td>
          <td class="border px-4 py-2">{{ course.descripcion }}</td>
          <td class="border px-4 py-2">{{ course.cantidadAlumnos }}</td>
          <td class="border px-4 py-2">{{ new Date(course.inicio).toLocaleDateString() }}</td>
          <td class="border px-4 py-2">{{ new Date(course.fin).toLocaleDateString() }}</td>
          <td class="border px-4 py-2">{{ course.precio }}</td>
          <td class="border px-4 py-2">
            <div class="flex gap-x-2">
              <a :href="'/courses/' + course.id + '/students'" class="text-blue-500 hover:underline">
                Ver alumnos
              </a>

              <button @click="startUpdateCourse(course)" class="text-white bg-indigo-500 hover:bg-indigo-600 p-2 rounded">Editar curso</button>
            </div>
          </td>
        </tr>
      </tbody>
    </table>

    <CourseForm @submitted="saveOne" />
    <Modal v-if="updatingCourse" :isOpen="true" @close="updatingCourse = null" title="Actualizar curso">
      <div class="grid w-[50vw]">
      <CourseForm :dataCourse="updatingCourse" @submitted="updateCourse" />
      </div>
    </Modal>
  </div>
</template>

<script>
import CourseForm from '../components/Forms/CourseForm.vue';
import Modal from '../components/Modal.vue';

// type Course = {
//   id: number;
//   nombre: string;
//   categoria: string;
//   descripcion: string;
//   cantidadAlumnos: number;
//   inicio: string;
//   fin: string;
//   precio: number;
// }

export default {
  components: {
    CourseForm,
    Modal,
  },
  data() {
    return {
      allCourses: [],
      search: '',
      updatingCourse: null,
    };
  },
  methods: {
    async getCourses() {
      try {
        const response = await fetch('https://662aa18dd3f63c12f4583be5.mockapi.io/api/pt/cursos');
        if (response.ok) {
          this.allCourses = await response.json();
        } else {
          console.error('Error fetching cursos:', response.statusText);
        }
      } catch (error) {
        console.error('Error fetching cursos:', error);
      }
    },

    startUpdateCourse(course) {
      this.updatingCourse = course;
    },

    async saveOne(course) {
      try {

        const response = await fetch('https://662aa18dd3f63c12f4583be5.mockapi.io/api/pt/cursos', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(course),
        });

        if (!response.ok) {
          throw new Error('Error al crear el curso');
        }

        const saved = await response.json();
        this.allCourses.push(saved);
      } catch {
        console.error('Error al crear el curso');
      }
    },

    async updateCourse(updatedCourse) {
      try {
        const response = await fetch(`https://662aa18dd3f63c12f4583be5.mockapi.io/api/pt/cursos/${this.updatingCourse.id}`, {
          method: 'PUT',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(updatedCourse),
        });

        if (!response.ok) {
          throw new Error('Error al actualizar el curso');
        }

        const updatedData = await response.json();

        // Update the local list of courses with the updated data
        const index = this.allCourses.findIndex(course => course.id === updatedData.id);
        if (index !== -1) {
          this.allCourses.splice(index, 1, updatedData);
        }

        // Close the modal
        this.updatingCourse = null;

      } catch (error) {
        console.error('Error al actualizar el curso:', error);
      }
    },
  },
  mounted() {
    this.getCourses();
  },
  computed: {
    coursesfo: function () {
      return this.allCourses.filter((course) => {
        return course.nombre.toLowerCase().includes(this.search.toLowerCase());
      })
    }
  }
};
</script>

<style scoped>
.container {
  max-width: 1200px;
}
</style>
