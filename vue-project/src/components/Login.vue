<template>
    <form @submit.prevent="login">
      <label for="username">Username:</label>
      <input type="text" v-model="username" id="username" required>
  
      <label for="password">Password:</label>
      <input type="password" v-model="password" id="password" required>
  
      <button type="submit">Login</button>
    </form>
    <div v-if="errorMessage" class="error-message">{{ errorMessage }}</div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        username: '',
        password: '',
        errorMessage: ''
      };
    },
    methods: {
    async login() {
      try {
        // API call till endpointen för att få token
        const response = await axios.post('', {
          username: this.username,
          password: this.password
        });

        // Sparar token i local storage
        localStorage.setItem('token', response.data.token);

        // Emit the login event with the token
        this.$emit('login', { username: this.username});
      } catch (error) {
        this.errorMessage = 'Inloggningen misslyckades. Pröva på nytt.';
        console.error('Inloggningen misslyckades:', error);
      }
    }
  }
};
</script>
  
  <style scoped>
  /* Add any component-specific styles here */
  </style>