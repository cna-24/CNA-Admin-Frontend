<!-- App.vue -->
<template>
  <div id="app">

    <header>Login</header>
    <Login @login="handleLogin" v-if="!loggedIn && !loading" />
    <div v-else>
      <!-- Visar att det händerr något-->
      <div v-if="loading">Loading...</div>
      <!-- Visar logout och namn då man är inloggad-->
      <div v-else>
        <p>Välkommen, {{ username }}!</p>
        <button @click="logout">Logga ut</button>

        <!-- Visar products när man är inloggad -->
        <Products v-if="loggedIn" />

        <!-- Visar users när man är inloggad -->
        <Users v-if="loggedIn" />
      </div>
    </div>

  </div>
</template>

<script>
import Login from './components/Login.vue';
import Products from './components/Products.vue';
import Users from './components/Users.vue';
import axios from 'axios';

// Constant for API endpoint
const LOGIN_ENDPOINT = '';

export default {
  components: {
    Login,
    Products,
    Users,
  },
  data() {
    return {
      loggedIn: false,
      username: '',
      loading: false // Add a loading state
    };
  },
  methods: {


    // Hantera inloggning
    async handleLogin(credentials) {
      // Anta att du har en metod för att validera token
      this.loading = true;
      if (await this.validateToken()) {
        this.loggedIn = true;
        this.username = credentials.username;
      }
      this.loading = false;
    },
    // Logga ut användaren
    logout() {
      this.loggedIn = false;
      this.username = '';
      // Rensa token från local storage vid utloggning
      localStorage.removeItem('token');
    },
    // Validera JWT-token
    async validateToken() {
      const token = localStorage.getItem('token');

      if (token) {
        try {
          // Gör en begäran till en skyddad resurs för att kontrollera om token är giltig
          const response = await axios.get(LOGIN_ENDPOINT, {
            headers: {
              'Authorization': `Bearer ${token}`
            }
          });
          console.log('Token är giltig');
          return true;
        } catch (error) {
          console.error('Token-validering misslyckades:', error);
          // Hantera ogiltig token
          return false;
        }
      } else {
        // Token saknas i local storage
        return false;
      }
    }
  }
};
</script>

