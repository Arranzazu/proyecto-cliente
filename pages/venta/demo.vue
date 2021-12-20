<template>
  <div class="Evento">
    <b-container fluid>
      <b-row class="text-center">
        <div class="saludo">
          <b-card title="Demo" class="mb-2">
            Producto y evento<b></b><br /><br />
            <b-form>
              <b-form-input
                class="mb-2 mr-sm-2 mb-sm-3"
                id="unidades"
                type="text"
                placeholder="ProductId"
                v-model="productId"
                required
              ></b-form-input
              >
              <b-form-input
                class="mb-2 mr-sm-2 mb-sm-3"
                id="unidades"
                type="text"
                placeholder="EventId"
                v-model="eventId"
                required
              ></b-form-input>
              <b-button
                @click="demo"
                variant="success"
                size="lg"
                class="mb-2 mr-sm-2 mb-sm-3"
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
            {{data2}}
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
      productId: null,
      eventId: null,
      numero: '',
      isAdmin: '',
      isUser: '',
      myemail: '',
      usuarios: '',
    }
  },

  async asyncData() {
    try {
      
      const url = 'http://localhost:4500/event/all' //InfoCarrito
      const res = await fetch(url)
      const data = await res.json()

      const url2 = 'http://localhost:4500/venta/all' //listado productos
      const res2 = await fetch(url2)
      const data2 = await res2.json()

      const url3 = 'http://localhost:4500/carrito/all' //listado productos
      const res3 = await fetch(url3)
      const data3 = await res3.json()

      return {
        data: data.events,
        data2: data2.ventas,
        data3,
        events: data.events,
        ventas: data2.ventas,
        users: data3.users,
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

    async demo() {
      console.log('demo')
      if (!this.eventId) {
        alert('Debe indicar el número del carrito')
        return
      }
      try {
        
        const url = 'http://localhost:4500/prueba/cantidad'
        const body = JSON.stringify({
          
          eventId: this.eventId,
          productId: this.productId,
        })
        const res = await fetch(url, {
          method: 'post',
          headers: {
            'Content-Type': 'application/json',
          },
          body,
        })
        const data4 = await res.json()
        console.log({ body })
        if (data4.error) {
          alert(data.error)
        } else {
          console.log({ data4 }) // data
         return {
        data4: data4.products,}
        }
      } catch (err) {
        alert('Hubo un error al hacer la Demo')
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
