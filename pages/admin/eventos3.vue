<template>
  <div class="Evento">
    <b-container fluid>
      <b-row class="text-center">
      
          <div class="saludo">
            <b-card title="Listado de eventos" class="mb-2">
              <b-card-text>
                Busca un evento y elecciona las opciones
              </b-card-text >
              <div>
                <b-form  > 
                  <b-list-group>
                    <b-list-group-item v-for="event in events" :key="event._id" :sort="event.date">
                      <b>{{ event.name }}, {{ event.date }}</b>
                      
                      <b-button size="sg" class="mb-2 mr-sm-2 mb-sm-1" router-link to="../details/event-detail"
                        >Detalles</b-button
                      >
                       <b-button size="sg" class="mb-2 mr-sm-2 mb-sm-1" router-link to="/admin/asignar"
              >Asignar Evento</b-button
            >
                      <b-button size="sg" class="mb-2 mr-sm-2 mb-sm-1" router-link to="../details/consumos-evento"
                        >Consumos</b-button
                      >
                      <b-button
                        variant="danger"
                        size="sg"
                        class="mb-2 mr-sm-2 mb-sm-1"
                        @click="removeEvent(event._id)"
                        >Eliminar</b-button
                      >
                      
                      </b-list-group-item
                    ></b-list-group
                  >

                    

                  <!-- <b-form-select
                    v-model="selected"
                    :options="listevents"
                    size="sm"
                    placeholder="seleccione evento"
                    class="mt-3 mb-sm-3"
                  > -->
                  <!-- <template #first>
        <b-form-select-option :value="null" disabled>-- Please select an option --</b-form-select-option>
        <b-form-select-option v-for="event in events" :key="event._id"  value="C">{{ event.name }}, {{ event.date }}</b-form-select-option>
      </template> -->
                  <!-- <br />    </b-form-select
                  > -->
<br>
                  <b-button
                    router-link
                    to="/admin/nuevo-evento"
                    size="lg"
                    variant="success"
                    class="mb-2 mr-sm-2 mb-sm-3"
                    >Crear Nuevo</b-button
                  >

               
                    <p><nuxt-link to="../menu">Volver al menú</nuxt-link></p>
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
    return {}
  },

  async asyncData() {
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
    //   const admin = window.localStorage.getItem('admin')
  
    // if ( admin == true ) {
    //   this.$router.push('/menu')  
      
     
    // }
   //reviso si es admin
    // this.isAdmin = window.localStorage.getItem('admin') === 'true'
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

  },
}
</script>
<style>
.saludo {
  margin-top: 40px !important;
  padding: 40px;
}
</style>
