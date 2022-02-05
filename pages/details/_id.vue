<template>
  <div class="Evento">
    <b-container fluid>
      <b-row class="text-center">
        <div class="saludo">
          <b-card title="Detalles del Evento" class="mb-2">
            <b-card-text>
              Detalles del evento<b> {{ data.event.name }} </b><br />
              Activo:<b v-if="data.event.activo === false"> No </b>
              <b v-if="data.event.activo === true"> Si </b>
              <br />
              Fecha:<b> {{ data.event.date.substring(0, 10) }}</b
              ><br />
            </b-card-text>
            <p>Carritos:</p>
            <div
              class="carritos"
              v-for="carrito in carritos"
              :key="carrito._id"
            >
              <b-button
                v-if="carrito.evento._id === eventId"
                @click="onClick(carrito._id)"
                size="lg"
                variant="outline-primary"
                class="mb-2 mr-sm-2"
                >Carrito {{ carrito.numero }}</b-button
              >
            </div>

            <br />

            <b-button
              @click="newCar(eventId)"
              size="lg"
              variant="success"
              class="mb-2 mr-sm-2"
              >Añadir Carrito</b-button
            >

             <b-button
                    v-if="data.event.activo === false"
                    variant="info"
                    size="lg"
                    class="mb-2 mr-sm-2 mb-sm-1"
                    @click="ActiveYes(eventId)"
                    >Activar Evento</b-button
                  >

                  <b-button
                    v-if="data.event.activo === true"
                    variant="warning"
                    size="lg"
                    class="mb-2 mr-sm-2 mb-sm-1"
                    @click="ActiveNo(eventId)"
                    >Desactivar Evento</b-button
                  >

            <br />
           
          </b-card> <br /><b-button
              size="sm"
              variant="outline-primary"
              class="mb-2 mr-sm-2 mb-sm-3"
              @click="volver"
            >
              Volver</b-button
            >
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
      cons: '',
      eventId: undefined,
    }
  },

  //  async beforeMount() {
  //     this.paramId = this.$route.params.id
  //     this.eventId = window.localStorage.getItem('eventId')

  //     // si no tiene token le echamos a login
  //     const token = window.localStorage.getItem('token')
  //     if (!token) {
  //       this.$router.push('/sign-in')
  //   }
  //   },

  async asyncData(ctx) {
    try {
      const id = ctx.params.id
      const url = `http://localhost:4500/event/get/${id}` //InfoEvent
      const res = await fetch(url)
      const data = await res.json()

      const url2 = 'http://localhost:4500/carrito/all' //listado  carritos
      const res2 = await fetch(url2)
      const data2 = await res2.json()

    
      return {
        data,
        data2,
        carritos: data2.carritos,
        eventId: ctx.params.id,
      }
    } catch (_) {
      console.log('Hubo un problema!')
      // ctx.redirect('/home')
    }
  },
  methods: {
    volver() {
      this.$router.back()
    },
    onClick(carritoid) {
      this.$router.push(`/carrito/${carritoid}`)
    },
    newCar(eventId) {
      console.log('venta')
      this.$router.push(`/nuevo-carrito/${eventId}`)
    },
    sortCarritos(carritos) {
      return carritos, 'numero'
    },

    async ActiveYes(eventId) {
      const url = `http://localhost:4500/event/ActiveYes/${eventId}`
      var answer = confirm('Vas a actiar el evento')
      if (answer) {
        await fetch(url, { method: 'post' })
        alert('Evento Activo')
        await   this.$router.back()
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
        await   this.$router.back()
      } else {
        this.$router.push('/admin/eventos')
      }
    },
     async updateData() {
    
      const actualize = `http://localhost:4500/event/get/${id}`
      const res = await fetch(actualize)
      const data = await res.json()
      this.event = data.event
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
