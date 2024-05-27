<script setup>
import { ref, reactive, computed } from 'vue'
import FormModal from './components/FormModal.vue'

const showStep = ref(1)

const numberSteps = 3

const formData = reactive({
    stepOne: {
        firstName: '',
        lastName: '',
        email: '',
    },
    stepTwo: {
        languages: [],
        year: 0,
    },
    stepThree: {
        comments: '',
    },
})

const stringIsEmpty = (str) => !str?.length

const stepTwoIsActive = computed(() => {
    return (
        !stringIsEmpty(formData.stepOne.firstName) &&
        !stringIsEmpty(formData.stepOne.lastName) &&
        !stringIsEmpty(formData.stepOne.email)
    )
})

const stepThreeIsActive = computed(() => {
    return formData.stepTwo.year > 0
})

const progress = ref(100 / numberSteps)

const showModal = ref(false)
</script>

<template>
    <div class="container mt-4">
        <div class="grid mb-2">
            <button @click=";(showStep = 1), (progress = 100 / numberSteps)" :class="{ activeButton: showStep === 1 }">
                Step 1
            </button>
            <button
                @click=";(showStep = 2), (progress = (100 / numberSteps) * 2)"
                :class="{ activeButton: showStep === 2 }"
                :disabled="stepTwoIsActive === false"
            >
                Step 2
            </button>
            <button
                @click=";(showStep = 3), (progress = 100)"
                :class="{ activeButton: showStep === 3 }"
                :disabled="stepThreeIsActive === false"
            >
                Step 3
            </button>
        </div>

        <div class="progress-bar mb-2">
            <progress :value="progress" max="100" />
        </div>

        <form @submit.prevent>
            <div v-show="showStep === 1">
                <fieldset>
                    <label>
                        First name
                        <input
                            name="first_name"
                            v-model="formData.stepOne.firstName"
                            placeholder="First name"
                            autocomplete="given-name"
                            required
                        />
                    </label>
                    <label>
                        First name
                        <input
                            name="last_name"
                            v-model="formData.stepOne.lastName"
                            placeholder="Last name"
                            autocomplete="given-name"
                            required
                        />
                    </label>
                    <label>
                        Email
                        <input
                            type="email"
                            name="email"
                            v-model="formData.stepOne.email"
                            placeholder="Email"
                            autocomplete="email"
                            required
                        />
                    </label>
                </fieldset>
            </div>

            <div v-show="showStep === 2">
                <fieldset>
                    <legend>Language preferences (Select languages):</legend>
                    <label>
                        <input
                            type="checkbox"
                            name="language"
                            value="english"
                            v-model="formData.stepTwo.languages"
                            checked
                        />
                        English
                    </label>
                    <label>
                        <input type="checkbox" name="language" value="french" v-model="formData.stepTwo.languages" />
                        French
                    </label>
                    <label>
                        <input type="checkbox" name="language" value="italian" v-model="formData.stepTwo.languages" />
                        Italian
                    </label>
                    <label>
                        <input type="checkbox" name="language" value="german" v-model="formData.stepTwo.languages" />
                        German
                    </label>
                </fieldset>

                <select name="year" aria-label="Select year.." v-model="formData.stepTwo.year" required>
                    <option selected disabled>Select year...</option>
                    <option>2024</option>
                    <option>2025</option>
                    <option>2026</option>
                </select>
            </div>

            <div v-show="showStep === 3">
                <textarea
                    name="notes"
                    v-model="formData.stepThree.comments"
                    placeholder="Comments...."
                    aria-label="Comments and notes"
                ></textarea>
                <input type="submit" @click="showModal = true" value="Send" />
            </div>
        </form>
    </div>

    <FormModal :showModal="showModal" :formData="formData" @closeModal="showModal = false" />
</template>

<style scoped>
.activeButton {
    background-color: rgb(4, 116, 4);
}
</style>
