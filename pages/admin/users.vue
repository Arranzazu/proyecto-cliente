<template>
  <div class="Evento">
    <b-container fluid>
      <b-row class="text-center">
      
          <div class="saludo">
            <b-card title="Listado de Usuarios" class="mb-2">
              <b-card-text>
                Listado de usuarios registrados en la App
              </b-card-text >
              <div>
                <b-form  > 
                  <b-list-group>
                    <b-list-group-item v-for="user in users" :key="user._id">
                      <b v-if="user.admin === false">{{ user.email }}, es Usuario  </b>
                      <b v-if="user.admin === true">{{ user.email }}, es Administrador  </b>
                      
                 
                      <b-button
                       v-if="user.admin === false"
                        variant="info"
                        size="sg"
                        class="mb-2 mr-sm-2 mb-sm-1"
                        @click="AdminYes(user._id)"
                        >Hacer Admin</b-button
                      >

                      <b-button
                            v-if="user.admin === true"
                        variant="warning"
                        size="sg"
                        class="mb-2 mr-sm-2 mb-sm-1"
                        @click="AdminNo(user._id)"
                        >Quitar de Admin</b-button
                      >

                      <b-button
                        variant="danger"
                        size="sg"
                        class="mb-2 mr-sm-2 mb-sm-1"
                        @click="removeUser(user._id)"
                        >Eliminar</b-button
                      >
                      
                      </b-list-group-item
                    ></b-list-group
                  >

            
<br>
                 
               
                   <br>
            <b-button size="sm" variant="outline-primary" class="mb-2 mr-sm-2 mb-sm-3" @click="volver">
              Volver</b-button
            ><br />
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
    layout: 'lists',
  data() {
    return {
           users: [],
           
    }
  },

//   async asyncData() {
//     try{
//      const res = await fetch('http://localhost:4500/user/all') //listado users
    
//     const data = await res.json()
//      console.log({ data })

// if (data.error) {
//           alert(data.error)
//           return this.$router.push('/menu')
//         }              
     
//          const users = data.users //creo la constante users que esta dentro de la data.users. Ahora solo lo llamaré como users
//         for (let i = 0; i < users.length; i++) {
//           this.users.push(users[i])
            
//    }
//       } catch (err) {
      
//       }
//   },

    async asyncData() {
    const url = 'http://localhost:4500/user/all' //listado users
    const res = await fetch(url)
    const data = await res.json()

    return {
      users: data.users,
           
    }
  },

  // si no tiene token le echamos a login
  beforeMount() {    

    const token = window.localStorage.getItem('token')
    if (!token) {
      this.$router.push('/sign-in')
       }


     const admin = window.localStorage.getItem('admin')
      if ( admin === 'false' ) {
      this.$router.push('../menu')  
    }
         
     

  },
  methods: {
    async removeUser(id) {
      const url = `http://localhost:4500/user/suprime/${id}`
      var answer =confirm('Estás seguro que deseas eliminar el usuario?', );
      if (answer){
      await fetch(url, { method: 'delete' })
      alert('usuarioSuprimido')
      await this.updateData()}
      else{        this.$router.push('/admin/users') }
    },

      async AdminYes(id) {
      const url = `http://localhost:4500/user/AdminYes/${id}`
      var answer =confirm('Vas a hacer Admin al usuario' );
      if (answer){
      await fetch(url, { method: 'post' })
      alert('El usuario ya es Admin')
      await this.updateData()}
      else{        this.$router.push('/admin/users') }
    },

         async AdminNo(id) {
      const url = `http://localhost:4500/user/AdminNo/${id}`
      var answer =confirm('El usuario dejará de ser Admin' );
      if (answer){
      await fetch(url, { method: 'post' })
      alert('El usuario ya no es Admin')
      await this.updateData()}
      else{        this.$router.push('/admin/users') }
    },

    async updateData() {
      const actualize = 'http://localhost:4500/user/all'
      const res = await fetch(actualize)
      const data = await res.json()
      this.users = data.users
    },
  volver() {
      this.$router.back()
 },
  },
}
</script>
<style>
.saludo {
  margin-top: 40px !important;
  padding: 40px;
}
</style>
