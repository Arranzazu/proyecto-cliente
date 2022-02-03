<template>
  <div class="Almacen">
    <b-container fluid>
      <b-row class="text-center">
        <div class="saludo">
          <b-card title="Gestión de Productos" class="mb-2"></b-card>
          <b-card-text>
            <b-img
              align="left"
              src="./../../static/Helados-malvarrosa-horiz2.png"
            ></b-img
            ><br
          /></b-card-text><br>
          <b-button
            size="lg"
            variant="success"
            class="mb-2 mr-sm-2 mb-sm-3"
            @click="newproduct"
            >Crear nuevo Producto</b-button
          >
          <b-button
            size="lg"
            variant="warning"
            class="mb-2 mr-sm-2 mb-sm-3"
            @click="newcategory"
            >Crear Categoría</b-button
          >
          <div class="table details">
            <table class="table">
              <tr>
                <th>Producto</th>
                <th  sortable: true>Categoría</th>
                <th>Activo</th>
                <th>Cambiar Estado</th>
                <th>Eliminar</th>
              </tr>
              <tr v-for="product in products" v-bind:key="product._id">
                <td v-text="product.product"></td>

                <td v-text="product.category"></td>
                <td v-if="product.active === false"> No </td>
              <td v-if="product.active === true"> Si </td>
               
                <td>
                  <b-button
                    v-if="product.active === false"
                    variant="info"
                    size="sg"
                    class="mb-2 mr-sm-2 mb-sm-1"
                    @click="ActiveYes(product._id)"
                    >Activar</b-button
                  >

                  <b-button
                    v-if="product.active === true"
                    variant="warning"
                    size="sg"
                    class="mb-2 mr-sm-2 mb-sm-1"
                    @click="ActiveNo(product._id)"
                    >Desactivar</b-button
                  >
                </td>
                <td>
                  <b-button
                    variant="danger"
                    size="sg"
                    class="mb-2 mr-sm-2 mb-sm-1"
                    @click="removeProduct(product._id)"
                    >Eliminar</b-button
                  >
                </td>
              </tr>
            </table>
          </div>
         
          <br />
          <b-button
            size="sm"
            variant="outline-primary"
            class="mb-2 mr-sm-2 mb-sm-3"
            @click="volver"
          >
            Volver</b-button
          ><br />
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
      data,
      products: data.products,
    }
  },

  methods: {
    newproduct() {
      this.$router.push('/admin/new-product')
    },
    newcategory() {
      this.$router.push('./new-category')
    },
    async removeProduct(id) {
      const url = `http://localhost:4500/almacen/suprime/${id}`
      var answer = confirm('Estás seguro que deseas eliminar el producto?')
      if (answer) {
        await fetch(url, { method: 'delete' })
        alert('Producto Suprimido')
        await this.updateData()
      } else {
        this.$router.push('/admin/productos')
      }
    },

    async ActiveYes(id) {
      const url = `http://localhost:4500/almacen/ActiveYes/${id}`
      var answer = confirm('Vas a actiar el producto')
      if (answer) {
        await fetch(url, { method: 'post' })
        alert('Producto Activo')
        await this.updateData()
      } else {
        this.$router.push('/admin/productos')
      }
    },

    async ActiveNo(id) {
      const url = `http://localhost:4500/almacen/ActiveNo/${id}`
      var answer = confirm('El producto dejará de estar activo')
      if (answer) {
        await fetch(url, { method: 'post' })
        alert('Producto Desactivado')
        await this.updateData()
      } else {
        this.$router.push('/admin/productos')
      }
    },

    async updateData() {
      const actualize = 'http://localhost:4500/almacen/all'
      const res = await fetch(actualize)
      const data = await res.json()
      this.products = data.products
    },
    volver() {
      this.$router.back()
    },
  },
}
</script>

<style>
.saludo {
  margin-top: 0px !important;
  padding: 0px;
}
</style>
