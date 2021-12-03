<template>
    <div class="Sign-in">
        <b-container fluid>
             <b-row class="text-center"  >
   
      <div class="saludo">
  <b-card
    title="Alta nuevo Usuario"
     class="mb-2"
  >
    <b-card-text>
      Introduce un correo electrónico y una contraseña para acceder al sistema
      
    </b-card-text>
<div>
  <b-form >
     <b-form-input
      id="inline-form-input-name"
      class="mb-2 mr-sm-2 mb-sm-3"
      placeholder="usuario"
      type="email"
      label-for="email"
      v-model="email"
      required
          ></b-form-input>

    <b-form-input
      id="inline-form-input-name"
      class="mb-2 mr-sm-2 mb-sm-3"
      placeholder="Password"
      type="password"
      label-for="password1"
      v-model="password1"
      required
    ></b-form-input>

  <b-form-input
      id="inline-form-input-name"
      class="mb-2 mr-sm-2 mb-sm-3"
      placeholder="Repite Password"
      type="password"
      label-for="password2"
      v-model="password2"
      required
    ></b-form-input>


    <b-button block variant="primary"  @click="onClick">Enviar</b-button>
     <p>¿Ya tienes usuario? <nuxt-link to="/sign-in">Ir a Login</nuxt-link></p>
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
      password1: '',
      password2: '',
    
    }
  },
  methods: {
    async onClick () {
      console.log("SignUP")
      if (!this.email || !this.password1 || !this.password2 || this.password1 !== this.password2) {
        alert('Error 222en email o password')
        return
      }
      try {
        const url = 'http://localhost:4500/user/signUp'
        const body = {
          email: this.email,
          password1: this.password1,
          password2: this.password2,
        }
        const res = await fetch(url, {
          method: 'post',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(body)
        })
        const data = await res.json()
        if (data.error) {
          alert(data.error)
        } else {
          console.log({ data }) // data
          this.$router.push('/sign-in')
        }
      } catch (err) {
        alert('Hubo un error al crear tu usuario')
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