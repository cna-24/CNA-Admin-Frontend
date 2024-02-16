<template>
    <div>
        <form @submit.prevent="submitForm">
            <label for="name">Namn:</label>
            <input v-model="user.name" type="text" id="name" required>

            <label for="email">Email:</label>
            <input v-model="user.email" type="email" id="email" required>

            <button type="submit">Updatera Användare</button>
        </form>
    </div>
</template>
  
<script>
import axios from 'axios';

export default {
    props: {
        user: {
            type: Object,
            required: true
        }
    },
    methods: {
        async submitForm() {

            //call api
            axios.put('')
                .then(response => {
                    console.log('User updated successfully', response.data);
                })
                .catch(error => {
                    console.error('Error updating user', error);
                });
        }
    }
};
</script>