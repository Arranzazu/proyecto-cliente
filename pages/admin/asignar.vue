<template>
  <div class="Sign-in">
    <b-container fluid>
      <b-row class="text-center">
        <div class="saludo">
          <b-card title="Asignar Evento" class="mb-2">
            <b-card-text>
              Selecciona evento, carrito, usuario y pulsa "Asignar"
            </b-card-text>
            <div>
              <b-form
                >Evento:
                <b-form-select
                  v-model="eventos"
                  size="sm"
                  placeholder="seleccione evento"
                  class="mt-3 mb-sm-1"
                 
                >
                  <template #first>
                    <b-form-select-option
                      v-for="event in events"
                      :key="event._id"
                      :value=(event._id)
                      >{{ event.name }}, {{ event.date }}</b-form-select-option
                    >
                  </template> </b-form-select
                ><br />

                <!-- <b-form-select
              v-model="selected"
              :options="listcarritos"
              size="sm"
              placeholder="seleccione evento"
              class="mt-3 mb-sm-1"
            ></b-form-select
            ><br /> -->
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
                      :value=(user._id)
                      >{{ user.email }}</b-form-select-option
                    >
                  </template> </b-form-select
                ><br />Carrito Asignado:
                <b-form-input
                  class="mb-2 mr-sm-2 mb-sm-3"
                  id="carrito"
                  type="text"
                  placeholder="Número del carrito"
                  v-model="carrito"
                  required
                ></b-form-input>
                <b-button size="lg" class="mb-2 mr-sm-2 mb-sm-3"
                  >Asignar</b-button
                >
                  <div class="mt-2">Seleccionado: <strong>{{ eventos }} {{ usuarios }}</strong></div>
                <p><nuxt-link to="../menu">Volver</nuxt-link></p>
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
      eventos: null,
      usuarios: null,
      carrito: '',
      // listevents: [
      //   { value: null, text: 'seleccione uno' },
      //   { value: 'a', text: 'Evento 1' },
      //   { value: 'b', text: 'Evento 2' },
      //   { value: { C: '3PO' }, text: 'This is an option with object value' },
      //   { value: 'd', text: 'This one is disabled', disabled: true },
      // ],
      listcarritos: [
        { value: null, text: 'seleccione carrito' },
        { value: 'a', text: 'Carrito 1' },
        { value: 'b', text: 'Carrito 2' },
        { value: { C: '3PO' }, text: 'This is an option with object value' },
        { value: 'd', text: 'This one is disabled', disabled: true },
      ],
      // userslist: [
      //   { value: null, text: 'seleccione usuario' },
      //   { value: 'a', text: 'Usuario 1' },
      //   { value: 'b', text: 'Usuario 2' },
      //   { value: { C: '3PO' }, text: 'Usuario 3' },
      //   { value: 'd', text: 'This one is disabled', disabled: true },
      // ],
    }
  },
  async asyncData() {
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
// si no tiene token le echamos a login
  beforeMount() {
    const token = window.localStorage.getItem('token')
    if (!token) {
      this.$router.push('/sign-in')
    }
    //   const admin = window.localStorage.getItem('admin')
    // if ( admin !== true ) {
    //   this.$router.push('/menu')
    // }
    
  },
}
</script>
<style>
.saludo {
  padding-top: 80px !important;
}
</style>
