<template>
    <div class="Sign-in">
        <b-container fluid>
             <b-row class="text-center"  >

      <div class="saludo">
  <b-card
    title="Acceso Usuarios"
     class="mb-2"
  >
    <b-card-text>
      Introduce tu correo electrónico y contraseña para acceder
      
    </b-card-text>
<div>
  <b-form >
     <b-form-input
      label="email" 
      v-model="email"
      id="inline-form-input-name"
      class="mb-2 mr-sm-2 mb-sm-3"
      placeholder="usuario"
      type="email"
          ></b-form-input>

    <b-form-input
      label="password" 
      v-model="password"
      id="inline-form-input-name"
      class="mb-2 mr-sm-2 mb-sm-3"
      placeholder="Password"
      type="password"
    ></b-form-input>



    <b-button block variant="primary" @click="onLogin">Enviar</b-button>
     <p>¿No tienes usuario? <nuxt-link to="/sign-up">Regístrate</nuxt-link></p>
  </b-form>
</div>
    
  </b-card>

    </div>
    </b-row>
    
       </b-container>
    </div>
</template>

<script>
export default {
  data () {
    return {
      email: '',
      password: '',
    }
  },
  methods: {
    async onLogin () {
      const url = 'http://localhost:4500/user/login'
      const body = {
        email: this.email,
        password: this.password,
                    }
      try {
        const res = await fetch(url, {
          method: 'post',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(body),
        })
        const data = await res.json()
        if (data.error) {
          alert(data.error)
          return
        }
      
        window.localStorage.setItem('token', data.token)
        window.localStorage.setItem('userId', data.userId)
        window.localStorage.setItem('admin', data.admin)
        window.localStorage.setItem('email', data.email)
        this.$router.push('/menu')
      } catch (err) {
      }
    }
  }
}
</script>

<style>
.saludo {
  margin-top: 40px !important;
  padding: 40px;
}

</style>