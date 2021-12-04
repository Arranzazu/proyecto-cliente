<template>
  <div class="Evento">
    <b-container fluid>
      <b-row class="text-center">
        <div class="saludo">
          <b-card title="Detalles del Carrito" class="mb-2">
            <b-card-text>
              <b> {{ data.carrito.nombrecar }}</b> <br />
              Evento activo: {{ data.carrito.evento.activo }}<br />
              <!-- Asignado a {{carrito.user.email}} -->
            </b-card-text>
            <div class="table details">
              <table class="table">
                <tr>
                  <th>Producto</th>
                  <th>Unids.</th>
                  <th>Devolución</th>
                  <th>Consumos</th>
                </tr>
                <tr
                  v-for="product in products"
                  v-bind:key="product._id"
                  v-on:click="editar(products._id)"
                  v-bind:style="product.unids <= 10 ? 'color:red' : ''"
                >
                  <td v-text="product.product"></td>

                  <td v-text="product.unids"></td>
                  <td>
                    <b-form-input
                      v-model="cons"
                      placeholder="Unids."
                    ></b-form-input>
                  </td>
                  <td v-text="product.unids - cons"></td>
                </tr>
              </table>
            </div>
            <b-button
              router-link
              to="/admin/asignar-producto"
              size="lg"
              variant="success"
              class="mb-2 mr-sm-2 mb-sm-3"
              >Asignar Producto</b-button
            ><br>
            <b-button size="sm" variant="outline-primary" class="mb-2 mr-sm-2 mb-sm-3" @click="volver">
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

    }
  },

  async asyncData (ctx) {
      try {
    const id = ctx.params.id
    const url = `http://localhost:4500/carrito/get/${id}` //InfoEvent
    const res = await fetch(url)
    const data = await res.json()

        const url2 = 'http://localhost:4500/event/all' //listado  carritos
    const res2 = await fetch(url2)
    const data2 = await res2.json()

        const url3 = 'http://localhost:4500/almacen/all' //listadoproductos
    const res3 = await fetch(url3)
    const data3 = await res3.json()


          console.log({ data })

    return {

      data,
        data3,
        data2,
         events: data2.events,
         products: data3.products,
      // eventId: ctx.params.id,
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
 }
  }

</script>

<style>
.saludo {
  margin-top: 40px !important;
  padding: 40px;
}
</style>
