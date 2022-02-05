<template>
  <div class="menu">
    <b-container fluid>
      <b-row>
        <div class="saludo text-center">
          <p CLASS="h2 mb-sm-5">
            <b-icon icon="list"> </b-icon> MENÚ PRINCIPAL
          </p>

          <b-button
            v-if="isAdmin"
            size="lg"
            class="mb-2 mr-sm-2 mb-sm-3"
            @click="almacen"
            ><b-icon icon="house"> </b-icon> Almacén</b-button
          ><br />
          <b-button
            v-if="isAdmin"
            size="lg"
            class="mb-2 mr-sm-2 mb-sm-3"
            router-link
            to="/admin/eventos"
          >
            <b-icon icon="calendar-event"> </b-icon> Eventos</b-button
          ><br />
          <b-button
            v-if="isAdmin"
            size="lg"
            class="mb-2 mr-sm-2 mb-sm-3"
            router-link
            to="/admin/users"
            ><b-icon icon="people"> </b-icon> Usuarios</b-button
          ><br />
<br>
          <p v-if="isUser" align="center"><b>Bienvenid@ usuario de Helados Malvarrosa. </b><br> A continuación se le mostrarán (si los tuviera) los eventos que le han sido asignados</p>
          <h3> <p v-if="isAdmin" align="center"><u>Carritos con asignación pendiente:</u></p></h3>

          <div class="carritos" v-for="carrito in carritos" :key="carrito._id"> 
            <!-- {{carrito.evento ? carrito.evento.name : carrito}} -->
            <b-button
              v-if="carrito.usuario === userId && carrito.evento.activo=== false "
              @click="alcarrito(carrito._id)"
              size="lg"
              variant="outline-danger"
              class="mb-2 mr-sm-2"
              disabled
                            >
              <b>NO ACTIVO</b> {{ carrito.evento.name }}, carrito número
              {{ carrito.numero }}</b-button
            >
             <b-button
              v-if="carrito.usuario === userId && carrito.evento.activo=== true "
              @click="alcarrito(carrito._id)"
              size="lg"
              variant="outline-success"
              class="mb-2 mr-sm-2"
              v-bind:style="carrito.evento.activo === false ? '' : ''"
              >
              <b>ACTIVO</b> {{ carrito.evento.name }}, carrito número
              {{ carrito.numero }}</b-button
            >
          </div>
        </div>
      </b-row>
    </b-container>
  </div>
</template>
<script>
import {
  BIcon,
  BIconList,
  BIconPeople,
  BIconHouse,
  BIconCalendarEvent,
} from 'bootstrap-vue'
export default {
  components: {
    BIcon,
    BIconList,
    BIconPeople,
    BIconHouse,
    BIconCalendarEvent,
  },
  data() {
    return {
      users: [],
      user: '',
      userId: '',
      isAdmin: '',
      isUser: '',
      email: undefined,
      eventos: null,
    }
  },

  async asyncData() {
    const url = 'http://localhost:4500/carrito/all' //listado carritos
    const res = await fetch(url)
    const data = await res.json()

    const url2 = 'http://localhost:4500/event/all' //listado eventos
    const res2 = await fetch(url2)
    const data2 = await res2.json()

    return {
      data,
      data2,
      carritos: data.carritos,
      events: data2.events,
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
    this.isUser = window.localStorage.getItem('admin') === 'false'
    this.email = window.localStorage.getItem('email')
    this.userId = window.localStorage.getItem('userId')
  },

  methods: { 
    
   alcarrito(carritoid) {
      this.$router.push(`/carrito/${carritoid}`)
  },
    almacen() {
      this.$router.push('/admin/almacen')
    },
  },
  detailsEvent(id) {
    console.log('update: ' + id)
    this.$router.push(`/details/${id}`)
  },
 
}
</script>
<style>
.saludo {
  padding-top: 100px !important;
}
</style>
