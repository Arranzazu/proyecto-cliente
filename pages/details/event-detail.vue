<template>
  <div class="Detalles Evento">
    <b-container fluid>
      <b-row class="text-center">
        <div class="saludo">
          <b-card title="Detalles Evento" class="mb-2">
            <b-card-text> </b-card-text>

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
                  v-on:click="editar(product._id)"
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
              size="lg"
              variant="success"
              class="mb-2 mr-sm-2 mb-sm-3"
              @click="newproduct"
              >Añadir</b-button
            >

            <p><nuxt-link to="../menu">Volver al menú</nuxt-link></p>
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

  async beforeMount() {
    this.paramId = this.$route.params.id
    this.eventId = window.localStorage.getItem('eventId')

    // si no tiene token le echamos a login
    const token = window.localStorage.getItem('token')
    if (!token) {
      this.$router.push('/sign-in')
    }
  },
  //  async asyncData() {
  //   const url = `http://localhost:4500/event/${id}` //listadoproductos
  //   const res = await fetch(url)
  //   const data = await res.json()

  async asyncData() {
    const url = 'http://localhost:4500/almacen/all' //listadoproductos
    const res = await fetch(url)
    const data = await res.json()

    // async asyncData2() {
    // const url2 = 'http://localhost:4500/event/all' //listadoeventos
    // const res2 = await fetch(url2)
    // const data2 = await res2.json()

    return {
      data,
      products: data.products,
    }
  },

  methods: {
    newproduct() {
      this.$router.push('/admin/new-product')
    },
    editar(id) {
      this.$router.push(`/details/${id}`)
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
