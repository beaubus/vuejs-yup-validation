<script setup>
    import {ref, reactive} from 'vue'
    import * as yup from 'yup'

    let name = ref('Andrew')
    let email = ref('info@beaubus.com')
    let validation_errors = reactive([])
    const schema = yup.object().shape({
        name: yup.string().required("Name is required"),
        email: yup.string().required("Email is required").email("Invalid email format")
    })

    function validateForm()
    {
        validation_errors.length = 0
        schema.validate({name: name.value, email: email.value}, { abortEarly: false })
        .then(() => {
            console.log('form is valid do the thing...');
        }).catch(errors => errors.inner.forEach(err => validation_errors.push(err)))
    }

    function em(path) // error message
    {
        let error = validation_errors.filter(err => err.path.includes(path))[0]
        return error ? error.message : ''
    }
</script>


<template>
    <form @submit.prevent="validateForm">
        <label>
            Name: <input type="text" v-model="name" :class="{'is-error': em('name')}">
        </label>
        <span v-text="em('name')"></span>

        <label>
            Email: <input type="text" v-model="email" :class="{'is-error': em('email')}">
        </label>
        <span v-text="em('email')"></span>

        <button>Submit</button>
    </form>
</template>


<style scoped>
.is-error { border: 1px solid red }
</style>
