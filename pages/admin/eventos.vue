<template>
  <div class="Evento">
    <b-container fluid>
      <b-row class="text-center">
        <div class="saludo">
          <b-card title="Listado de eventos y opciones" class="mb-2">
            <b-card-text>
              Busca un evento y elecciona las opciones
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
                      :value=(event._id)
                      v-bind:style="event.activo === false ? 'color:red' : ''"
                      >{{ event.name }}, {{ event.date }}
                    </b-form-select-option>
                  </template>
                </b-form-select>

                <b-button size="sg" class="mb-2 mr-sm-2 mb-sm-3"
                  @click="detail(eventos)"
                  >Detalles</b-button
                >
              
                <b-button size="sg" class="mb-2 mr-sm-2 mb-sm-3"
                router-link to="../details/event-detail"
                  >Consumos</b-button
                >
                <b-button
                  :v-for="eventos"
                  variant="danger"
                  size="sg"
                  class="mb-2 mr-sm-2 mb-sm-3"
                  @click="removeEvent(eventos)"
                  >Eliminar</b-button
                >
                <!-- <div class="mt-2">
                  Seleccionado: <strong>{{ eventos }} </strong>
                </div> -->
                <br />
                <b-button
                  router-link
                  to="/admin/nuevo-evento"
                  size="lg"
                  variant="success"
                  class="mb-2 mr-sm-2 mb-sm-3"
                  >Crear Nuevo</b-button
                >

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
  data() {
    return {
      eventos: [],
    }
  },

  async asyncData() {
    var moment = require('moment'); // require
    console.log(moment().format('HH:mm:ss'));
    const url = 'http://localhost:4500/event/all' //listadoeventos
    const res = await fetch(url)
    const data = await res.json()

    return {
      events: data.events,
    }
  },

  // si no tiene token le echamos a login
  beforeMount() {
    const token = window.localStorage.getItem('token')
    if (!token) {
      this.$router.push('/sign-in')
    }
  },
  methods: {
    async removeEvent(id) {
      const url = `http://localhost:4500/event/suprime/${id}`
      var answer =confirm('Estás seguro que deseas eliminar el registro?');
      if (answer){
      await fetch(url, { method: 'delete' })
      alert('Evento Suprimido')
      await this.updateData()}
      else{        this.$router.push('/admin/eventos') }
    },

    async updateData() {
      const actualize = 'http://localhost:4500/event/all'
      const res = await fetch(actualize)
      const data = await res.json()
      this.events = data.events
    },
   detail(id) {
    this.$router.push(`/details/${id}`)
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
