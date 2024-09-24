<template>
    <dialog ref="dialog" class="fixed inset-0 z-50 flex items-center justify-center bg-black bg-opacity-50">
        <div class="bg-white p-6 rounded-lg shadow-lg w-full max-w-md">
            <div class="flex justify-between items-center mb-4">
                <h2 class="text-xl font-bold">{{ title }}</h2>
                <button @click="closeModal" class="text-gray-500">&times;</button>
            </div>
            <!-- Slot to render form or other content inside the modal -->
            <slot></slot>
        </div>
    </dialog>
</template>

<script>
export default {
    props: {
        isOpen: {
            type: Boolean,
            default: false,
        },
        title: {
            type: String,
            default: 'Modal',
        },
    },
    methods: {
        closeModal() {
            this.$refs.dialog.close();
            this.$emit('close');
        },
    },

    mounted() {
        this.$refs.dialog.close();
        this.$refs.dialog.showModal();
    },
};
</script>

<style scoped>
/* Add any necessary modal styling here */
</style>