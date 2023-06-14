<script>
export default {
  // ...
  computed: {
    isLoggedIn() {
      // Vérifiez si l'utilisateur est connecté en fonction de l'existence des informations d'identification dans le stockage local (localStorage) ou Vuex
      const user = JSON.parse(localStorage.getItem('user'));
      return user && user.token;
    },
  },
};
</script>

<template>
  <ion-app>
    <ion-router :use-router="isLoggedIn">
      <ion-route url="/home" component="LoginPage"></ion-route>

      <!-- Page admin -->
      <ion-route-redirect from="/" to="/admin" :if="isLoggedIn && $route.path === '/'" />
      <ion-route url="/admin" component="admin-page" :if="isLoggedIn && $route.path === '/admin'" />

      <!-- Page client -->
      <ion-route-redirect from="/" to="/client" :if="isLoggedIn && $route.path === '/'" />
      <ion-route url="/client" component="client-page" :if="isLoggedIn && $route.path === '/client'" />

      <!-- ... Autres routes ... -->
    </ion-router>

    <ion-nav :root="isLoggedIn ? 'admin-page' : 'login-page'"></ion-nav>
  </ion-app>
</template>
