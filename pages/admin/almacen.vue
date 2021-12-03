<template>
  <div class="Almacen">
    <b-container fluid>
      <b-row class="text-center">
        <div class="saludo">
          <b-card title="Almacén" class="mb-2">
            <b-card-text> </b-card-text>

            <div class="table details">
              <table class="table">
                <tr>
                  <th>Nombre</th>
                  <th>Categoría</th>
                  <th>Unids.</th>
                </tr>
                <tr
                  v-for="product in products"
                  v-bind:key="product._id"
                  v-bind:style="product.unids <= 10 ? 'color:red' : ''"
                >
                  <td v-text="product.product"></td>
                  <td v-text="product.category"></td>
                  <td v-text="product.unids"></td>
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
    }
  },

  // si no tiene token le echamos a login
  beforeMount() {
    const token = window.localStorage.getItem('token')
    if (!token) {
      this.$router.push('/sign-in')
    }
  },
  async asyncData() {
    const url = 'http://localhost:4500/almacen/all' //listadoproductos
    const res = await fetch(url)
    const data = await res.json()

    return {
      products: data.products,
    }
  },

  methods: {
    newproduct() {
      this.$router.push('/admin/new-product')
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
