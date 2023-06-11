<template>
    <ion-page>
      <ion-header>
        <ion-toolbar>
          <ion-title>Page de connexion</ion-title>
        </ion-toolbar>
      </ion-header>
  
      <ion-content class="ion-padding">
        <ion-input v-model="email" type="text" placeholder="Utilisateur"></ion-input>
        <ion-input v-model="password" type="password" placeholder="Mot de passe"></ion-input>
        <ion-button @click="login">Se connecter</ion-button>
  
        <p v-if="errorMessage" class="error-message">{{ errorMessage }}</p>
      </ion-content>
    </ion-page>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        email: '',
        password: '',
        errorMessage: '',
      };
    },
    methods: {
      async login() {
        try {
          const response = await axios.post('/api/user/login', {
            login: this.email,
            password: this.password,
          });
  
          const { role, id, login, token } = response.data;
  
          // Enregistrement des informations d'identification et du rôle dans le stockage local (localStorage) ou dans Vuex si vous l'utilisez
          localStorage.setItem('user', JSON.stringify({ role, id, login, token }));
  
          // Rediriger l'utilisateur vers la page appropriée en fonction du rôle
          if (role === 'admin') {
            this.$router.push('/admin');
          } else if (role === 'client') {
            this.$router.push('/client');
          }
        } catch (error) {
          if (error.response && error.response.status === 404) {
            this.errorMessage = 'Utilisateur inexistant';
          } else if (error.response && error.response.status === 400) {
            this.errorMessage = 'Mot de passe incorrect';
          } else {
            this.errorMessage = 'Erreur de connexion';
          }
        }
      },
    },
  };
  </script>
  
  <style scoped>
  .error-message {
    color: red;
  }
  </style>