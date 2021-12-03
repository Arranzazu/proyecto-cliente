<template>
  <div>
    <b-navbar class="navbar" toggleable="lg" type="dark" variant="info">
      <b-navbar-brand href="http://localhost:3000/menu">HELADOS MALVARROSA</b-navbar-brand>

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav v-if="email">
          <b-nav-item> Usuario: {{ email }}.</b-nav-item>
        </b-navbar-nav>

        <!-- Right aligned nav items -->

        <b-navbar-nav class="ml-auto" v-if="email">
          <b-button variant="danger" size="lg" @click="logout" nuxt
            >Logout</b-button
          >
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
  </div>
</template>

<script>

export default {
    
  data() {
    return {
      email: '',
      token: '',
    }
  },

  created() {
    if (process.client) {
      this.email = localStorage.getItem('email')
      this.token = localStorage.getItem('token')
    } //reviso si es admin
    // this.isUser = window.localStorage.getItem('token') !== '0'
    // this.email = window.localStorage.getItem('email')
  },

  methods: {
    logout() {
      this.$store.dispatch('user/removeToken')
      // this.$router.push('/home')
       this.$router.push('/sign-in')
    },
  },
}
</script>

<style>
.navbar {
  
  top: 0;
  width: 100%;
  z-index: 100;
}
</style>
