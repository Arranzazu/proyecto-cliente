<template>
  <div class="Evento">
    <b-container fluid>
      <b-row class="text-center">
        <div class="saludo">
          <b-card title="Detalles del Evento" class="mb-2">
            <b-card-text>
              Detalles del evento<b> {{ data.event.name }} </b><br />
              Activo:<b> {{ data.event.activo }}</b
              ><br />
              Fecha:<b> {{ data.event.date }}</b
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
              variant="outline-success"
              class="mb-2 mr-sm-2"
              >Añadir Carrito</b-button
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

      const url3 = 'http://localhost:4500/almacen/all' //listadoproductos
      const res3 = await fetch(url3)
      const data3 = await res3.json()

      // console.log({ data })

      return {
        data,
        data3,
        data2,
        carritos: data2.carritos,
        products: data3.products,
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
  },
}
</script>

<style>
.saludo {
  margin-top: 40px !important;
  padding: 40px;
}
</style>
