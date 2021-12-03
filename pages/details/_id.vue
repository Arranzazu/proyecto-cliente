<template>
  <div class="Evento">
    <b-container fluid>
      <b-row class="text-center">
        <div class="saludo">
          <b-card title="Detalles del Evento" class="mb-2">
            <b-card-text>
              Detalles del evento<b> {{ data.event.name }} </b><br />
              Activo:<b> {{ data.event.activo }}</b><br />
              Asignado a <b>Elena_Perez@gmail.com</b>
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
            ><p><nuxt-link to="../menu">Volver al menú</nuxt-link></p>
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
        // eventId: ctx.params.id,
      }
    } catch (_) {
      console.log('Hubo un problema!')
      // ctx.redirect('/home')
    }
  },
}
</script>

<style>
.saludo {
  margin-top: 40px !important;
  padding: 40px;
}
</style>
