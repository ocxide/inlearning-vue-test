<template>
    <div class="container mx-auto p-4" v-if="course">
        <h1 class="text-2xl font-bold mb-4">Lista de Alumnos - Curso: {{ course.nombre }}</h1>
        <table class="min-w-full bg-white shadow-md rounded-lg overflow-hidden">
            <thead>
                <tr>
                    <th class="py-2 px-4 bg-gray-200">Nombres</th>
                    <th class="py-2 px-4 bg-gray-200">Apellidos</th>
                    <th class="py-2 px-4 bg-gray-200">Email</th>
                    <th class="py-2 px-4 bg-gray-200">Código del alumno</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="student in course.alumnos" :key="student.codigo">
                    <td class="border px-4 py-2">{{ student.nombres }}</td>
                    <td class="border px-4 py-2">{{ student.apellidos }}</td>
                    <td class="border px-4 py-2">{{ student.correo }}</td>
                    <td class="border px-4 py-2">{{ student.codigo }}</td>
                </tr>
                <tr v-if="course.alumnos.length === 0">
                    <td colspan="4" class="border px-4 py-2 text-center">No hay alumnos inscritos en este curso.</td>
                </tr>
            </tbody>
        </table>

        <div class="w-full grid gap-4">
            <StudentList @student-selected="addExisting" />
            <StudentForm @appsubmit="saveUser" />
        </div>
    </div>
</template>

<script>
import StudentForm from '../components/Forms/StudentForm.vue';
import StudentList from '../components/StudentList.vue';

export default {
    components: {
        StudentForm,
        StudentList,
    },
    data() {
        return {
            course: null,
        };
    },
    methods: {
        async fetchCurso() {
            const courseId = this.$route.params.id; // Obtener el ID del curso de la ruta
            try {
                const response = await fetch(`https://662aa18dd3f63c12f4583be5.mockapi.io/api/pt/cursos/${courseId}`);
                if (response.ok) {
                    this.course = await response.json();
                } else {
                    console.error('Error fetching curso:', response.statusText);
                }
            } catch (error) {
                console.error('Error fetching curso:', error);
            }
        },
        async saveUser(user) {
            try {
                const response = await fetch('https://662aa18dd3f63c12f4583be5.mockapi.io/api/pt/alumnos', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                    },
                    body: JSON.stringify(user),
                });

                if (!response.ok) {
                    throw new Error('Error en el envío de datos');
                }

                const saved = await response.json();
                this.course.alumnos.push(saved);
            } catch (error) {
                console.error('Error submitting user data:', error);
                // Handle error appropriately (e.g., show an error message)
            }
        },
        addExisting(student) {
            this.course.alumnos.push(student);
        }
    },
    mounted() {
        this.fetchCurso();
    },
};
</script>

<style scoped>
.container {
    max-width: 800px;
}
</style>