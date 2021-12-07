<template>
  <div class="Nuevo-evento">
    <b-container fluid>
      <b-row class="text-center">
        <div class="saludo">
          <b-card title="Crear Nuevo Carrito" class="mb-2">
            <b-card-text>
              Selecciona el evento y el número de carrito.
            </b-card-text>
            <div>
              <b-form>
                <b-form-select
                  v-model="eventos"
                  size="sm"
                  placeholder=">-- Seleccione un evento --<"
                  class="mt-3 mb-sm-3"
                >
                  <template #first>
                    <b-form-select-option
                      v-for="event in events"
                      :key="event._id"
                      :value="event._id"
                      v-bind:style="event.activo === false ? 'color:red' : ''"
                      >{{ event.name }}, {{ event.date }}
                    </b-form-select-option>
                  </template>
                </b-form-select>
                Usuario:
                <b-form-select
                  v-model="usuarios"
                  size="sm"
                  placeholder="seleccione usuario"
                  class="mt-3 mb-sm-4"
                >
                  <template #first>
                    <b-form-select-option
                      v-for="user in users"
                      :key="user._id"
                      :value="user._id"
                      >{{ user.email }}</b-form-select-option
                    >
                  </template> </b-form-select
                ><br />Número del Carrito:
                <b-form-input
                  class="mb-2 mr-sm-2 mb-sm-3"
                  id="carrito"
                  type="text"
                  placeholder="Número del carrito"
                  v-model="carrito"
                  required
                ></b-form-input>

                <b-button
                  @click="NewCarrito"
                  size="lg"
                  variant="success"
                  class="mb-2 mr-sm-2 mb-sm-3"
                  >Crear</b-button
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
                <div class="mt-2">
                  Seleccionado:
                  <strong>{{ eventos }} {{ usuarios }} {{ carrito }}</strong>
                </div>
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
  data() {
    return {
      //   name: '',
      //   date: '',
      //   cars: '',
      //   activo: '',
      eventos: null,
      usuarios: null,
      carrito: '',
    }
  },

  // si no tiene token le echamos a login
  beforeMount() {
    const token = window.localStorage.getItem('token')

    if (!token) {
      this.$router.push('/sign-in')
    }

    const admin = window.localStorage.getItem('admin')
    if (admin === 'false') {
      this.$router.push('../menu')
    }
  },

  async asyncData() {
    var moment = require('moment') // require
    console.log(moment().format('HH:mm:ss'))
    const url = 'http://localhost:4500/event/all' //listadoeventos
    const res = await fetch(url)
    const data = await res.json()
    const url2 = 'http://localhost:4500/user/all' //listadousuarios
    const res2 = await fetch(url2)
    const data2 = await res2.json()

    return {
      events: data.events,
      users: data2.users,
    }
  },
  methods: {
    async updateData() {
      const actualize = 'http://localhost:4500/event/all'
      const res = await fetch(actualize)
      const data = await res.json()
      this.events = data.events
    },

    async NewCarrito() {
      console.log('Crear carrito')
      if (!this.eventos || !this.carrito) {
        alert('Selecciona el evento y asígnmale un número de carrito')
        return
      }
      try {
        const url = 'http://localhost:4500/carrito/create'
        const body = JSON.stringify({
          eventId: this.eventos,
          numero: this.carrito,
          userId: this.usuarios,
        })
        const res = await fetch(url, {
          method: 'post',
          headers: {
            'Content-Type': 'application/json',
          },
          body,
        })
        const data = await res.json()
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
