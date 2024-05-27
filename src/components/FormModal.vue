<script setup>
const props = defineProps({
    showModal: Boolean,
    formData: Object,
})

const emits = defineEmits(['closeModal'])

const capitalizeFirstLetter = (str) => {
    return str.charAt(0).toUpperCase() + str.slice(1)
}
</script>

<template>
    <Teleport to="body">
        <div class="modal-container" v-show="showModal" @click="$emit('closeModal')">
            <div class="modal">
                <h2>Your Data</h2>
                <ul>
                    <li>First Name: {{ formData.stepOne.firstName }}</li>
                    <li>Last Name: {{ formData.stepOne.lastName }}</li>
                    <li>Email: {{ formData.stepOne.email }}</li>
                    <li>Languages:</li>
                    <ol>
                        <li v-for="language in formData.stepTwo.languages">{{ capitalizeFirstLetter(language) }}</li>
                    </ol>
                    <li>Year: {{ formData.stepTwo.year }}</li>
                    <li>Comments: {{ formData.stepThree.comments }}</li>
                </ul>
            </div>
        </div>
    </Teleport>
</template>

<style scoped>
.modal-container {
    position: absolute;
    height: 100vh;
    width: 100%;
    top: 0;
    left: 0;
    background-color: rgba(0, 0, 0, 0.6);
    display: grid;
    place-items: center;
}

.modal {
    width: 60%;
    background-color: rgb(152, 101, 6);
    border-radius: 12px;
    padding: 2rem 4rem;
}
</style>
