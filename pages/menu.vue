<template>
  <div class="menu">
    <b-container fluid>
      <b-row>
        <div class="saludo text-center">
          <p CLASS="h2 mb-sm-5"><b-icon icon="list">  </b-icon>  MENÚ PRINCIPAL</p>

          <b-button
            v-if="isAdmin"
            size="lg"
            class="mb-2 mr-sm-2 mb-sm-3"
            @click="almacen"
            ><b-icon icon="house">  </b-icon> Almacén</b-button
          ><br />
          <b-button
            v-if="isAdmin"
            size="lg"
            class="mb-2 mr-sm-2 mb-sm-3"
            router-link
            to="/admin/eventos"
            > <b-icon icon="calendar-event">  </b-icon> Eventos</b-button
          ><br />
            <b-button
            v-if="isAdmin"
            size="lg"
            class="mb-2 mr-sm-2 mb-sm-3"
            router-link
            to="/admin/users"
            ><b-icon icon="people">  </b-icon> Usuarios</b-button
          ><br />

        <b-form-select
                  v-model="eventos"
                  size="sm"
                  placeholder="seleccione usuario"
                  class="mt-3 mb-sm-4"
                >
                  <template #first>
                    <b-form-select-option
                      v-for="event in events"
                      :key="event._id"
                      :value={event}                
                      >{{ event.name }}, {{ event.date }}</b-form-select-option
                    >
                  </template><br /></b-form-select
                >
        
          <b-button size="lg" class="mb-2 mr-sm-2 mb-sm-3"  @click="detailsEvent(id)"
            ><b-icon icon="calendar-event">  </b-icon> Ir a Detalles Evento</b-button
          > </div>
      </b-row>
    </b-container>
  </div>
</template>
<script>
import { BIcon, BIconList, BIconPeople, BIconHouse, BIconCalendarEvent } from 'bootstrap-vue'
export default {
    components: {
    BIcon,
    BIconList,
    BIconPeople,
    BIconHouse,
    BIconCalendarEvent
  },
  data() {
    return {
      users: [],
      user: '',
      isAdmin: '',
      email: undefined,
      eventos: null,
    }
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
  async beforeMount() {
    const token = window.localStorage.getItem('token')

    if (!token) {
      this.$router.push('/sign-in')
    }

    //reviso si es admin
    this.isAdmin = window.localStorage.getItem('admin') === 'true'
    this.email = window.localStorage.getItem('email')
  },
  methods: {
    almacen() {
      this.$router.push('/admin/almacen')
    },
  },
   detailsEvent(id) {
      console.log('update: '+ id)
      this.$router.push(`/details/${id}`)
    },
  
}
</script>
<style>
.saludo {
  padding-top: 100px !important;
}
</style>
