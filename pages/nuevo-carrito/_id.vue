<template>
  <div class="Evento">
    <b-container fluid>
      <b-row class="text-center">
        <div class="saludo">
          <b-card title="Crear Carrtio" class="mb-2">
              Evento <b>{{ data.event.name }}</b><br><br>
            <b-form>
              Usuario:
                <b-form-select
                  v-model="usuarios"
                  size="sm"
                  placeholder="seleccione usuario"
                  class="mb-2 mr-sm-2 mb-sm-3"
                >
                  <template #first>
                    <b-form-select-option
                      v-for="user in users"
                      :key="user._id"
                      :value="user._id"
                      >{{ user.email }}</b-form-select-option
                    >
                  </template> </b-form-select
                ><br />
              Número del carrito:
              <b-form-input
                class="mb-2 mr-sm-2 mb-sm-3"
                id="unidades"
                type="text"
                placeholder="Número"
                v-model="numero"
                required
              ></b-form-input>
              <b-button @click="NewCar" variant="success" size="lg" class="mb-2 mr-sm-2 mb-sm-3"
                >Crear Carrito</b-button
              >

              <br />
              <b-button
                size="sm"
                variant="outline-primary"
                class="mb-2 mr-sm-2 mb-sm-3"
                @click="volver"
              >
                Volver</b-button
              ><br />
            </b-form>
          </b-card>
        </div>
      </b-row>
    </b-container>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: null,
      productos: null,
      unidades: null,
      numero: '',
      isAdmin: '',
      isUser: '',
      myemail: '',
      usuarios:"",
    }
  },

  async asyncData(ctx) {
    try {
      const id = ctx.params.id
      const url = `http://localhost:4500/event/get/${id}` //InfoCarrito
      const res = await fetch(url)
      const data = await res.json()

      const url3 = 'http://localhost:4500/user/all' //listado productos
      const res3 = await fetch(url3)
      const data3 = await res3.json()

      return {
        data,
        data3,
        users: data3.users,
        eventId: ctx.params.id,
      }
    } catch (_) {
      console.log('Hubo un problema!')
      // ctx.redirect('/home')
    }
  },
  async beforeMount() {
    this.isAdmin = window.localStorage.getItem('admin') === 'true'
    this.isUser = window.localStorage.getItem('admin') === 'false'
    this.myemail = window.localStorage.getItem('email')
    
  },

  methods: {
    volver() {
      this.$router.back()
    },

    async NewCar() {
      console.log('Crear venta')
      if (!this.numero ) {
        alert('Debe indicar el número del carrito')
        return
      }
      try {
       const eventId = this.$route.params.id
       const url = 'http://localhost:4500/carrito/create'
        const body = JSON.stringify({
          eventId,
          userId: this.usuarios,
          numero: this.numero,
        })
        const res = await fetch(url, {
          method: 'post',
          headers: {
            'Content-Type': 'application/json',
          },
          body,
        })
        const data = await res.json()
          console.log({ body })
        if (data.error) {
          alert(data.error)
        } else {
          console.log({ data }) // data
          this.$router.back()
        }
      } catch (err) {
        alert('Hubo un error al crear el carrito')
      }
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
