<template>
  <div class="Evento">
    <b-container fluid>
      <b-row class="text-center">
        <div class="saludo">
          <b-card title="Listado de eventos y opciones" class="mb-2">
            <b-card-text>
              Busca un evento y selecciona las opciones o bien crea uno nuevo
            </b-card-text>
            <div>
              <b-form>


 <div class="table details">
            <table class="table">
              <tr>
                <th>Nombre</th>
                <th  sortable: true>Fecha</th>
                <th>Activo</th>
                  <th>Detalles</th>
                <th>Cambiar Estado</th>
                <!-- <th>Eliminar</th> -->
              </tr>
              <tr  v-for="event in events"           
                    v-bind:key="event._id"
                    >

                <td v-text="event.name" v-bind:style="event.activo === false ? 'color:red' : ''"></td>
                <td v-text="event.date.substring(0, 10)" v-bind:style="event.activo === false ? 'color:red' : ''"></td>
                <td v-if="event.activo === false" v-bind:style="event.activo === false ? 'color:red' : ''"> No </td>
                <td v-if="event.activo === true"> Si </td>
               
               <td>
                 <b-button
                  size="lg"
                  class="mb-2 mr-sm-2 mb-sm-3"
                  @click="detail(event._id)"
                  >Detalles</b-button
                ></td>

                <td>
                  <b-button
                    v-if="event.activo === false"
                    variant="info"
                    size="sg"
                    class="mb-2 mr-sm-2 mb-sm-1"
                    @click="ActiveYes(event._id)"
                    >Activar</b-button
                  >

                  <b-button
                    v-if="event.activo === true"
                    variant="warning"
                    size="sg"
                    class="mb-2 mr-sm-2 mb-sm-1"
                    @click="ActiveNo(event._id)"
                    >Desactivar</b-button
                  >
                </td>
                <!-- <td>
                  <b-button
                    variant="danger"
                    size="sg"
                    class="mb-2 mr-sm-2 mb-sm-1"
                    @click="removeEvent(event._id)"
                    >Eliminar</b-button
                  >
                </td> -->
              </tr>
            </table>
          </div>


<hr><hr>
                <br />
                <b-button
                  router-link
                  to="/admin/nuevo-evento"
                  size="lg"
                  variant="success"
                  class="mb-2 mr-sm-2 mb-sm-3"
                  >Crear Nuevo Evento</b-button
                >

                
              </b-form>
            </div>
          </b-card><br />
                <b-button
                  size="sm"
                  variant="outline-primary"
                  class="mb-2 mr-sm-2 mb-sm-3"
                  @click="volver"
                >
                  Volver</b-button
                ><br />
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
    var moment = require('moment') // require
    console.log(moment().format('HH:mm:ss'))
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
      var answer = confirm('Estás seguro que deseas eliminar el registro?')
      if (answer) {
        await fetch(url, { method: 'delete' })
        alert('Evento Suprimido')
        await this.updateData()
      } else {
        this.$router.push('/admin/eventos')
      }
    },

      async updateData() {
      const actualize = 'http://localhost:4500/event/all'
      const res = await fetch(actualize)
      const data = await res.json()
      this.events = data.events
    },

      async ActiveYes(eventId) {
      const url = `http://localhost:4500/event/ActiveYes/${eventId}`
      var answer = confirm('Vas a actiar el evento')
      if (answer) {
        await fetch(url, { method: 'post' })
        alert('Evento Activo')
        await   this.updateData()
      } else {
        this.$router.push('/admin/eventos')
      }
    },

    async ActiveNo(eventId) {
      const url = `http://localhost:4500/event/ActiveNo/${eventId}`
      var answer = confirm('El evento dejará de estar activo')
      if (answer) {
        await fetch(url, { method: 'post' })
        alert('Evento Desactivado')
        await   this.updateData()
      } else {
        this.$router.push('/admin/eventos')
      }
    },

    detail(eventId) {
      this.$router.push(`/details/${eventId}`)
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
