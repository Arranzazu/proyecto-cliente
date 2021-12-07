<template>
  <div class="Evento">
    <b-container fluid>
      <b-row class="text-center">
        <div class="saludo">
          <b-card title="Asignar Producto" class="mb-2">
            <b-form>
              <b-form-select
                v-model="productos"
                size="sm"
                placeholder="seleccione producto"
                class="mt-3 mb-sm-1"
              >
                <template #first>
                  <b-form-select-option
                    v-for="product in products"
                    :key="product._id"
                    :value="product._id"
                    >{{ product.product }}</b-form-select-option
                  >
                </template> </b-form-select
              ><br />
              Unidades:
              <b-form-input
                class="mb-2 mr-sm-2 mb-sm-3"
                id="unidades"
                type="text"
                placeholder="Unidades"
                v-model="unidades"
                required
              ></b-form-input>
              <b-button @click="NewVenta" size="lg" class="mb-2 mr-sm-2 mb-sm-3"
                >Asignar</b-button
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
      productos: null,
      unidades: null,
      cons: '',
      isAdmin: '',
      isUser: '',
      myemail: '',
    }
  },

  async asyncData(ctx) {
    try {
      const id = ctx.params.id
      const url = `http://localhost:4500/carrito/get/${id}` //InfoCarrito
      const res = await fetch(url)
      const data = await res.json()

      const url3 = 'http://localhost:4500/almacen/all' //listado productos
      const res3 = await fetch(url3)
      const data3 = await res3.json()

      return {
        data,
        data3,
        products: data3.products,
        carritoId: ctx.params.id,
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

    async NewVenta() {
      console.log('Crear venta')
      if (!this.productos || !this.unidades) {
        alert('Todos los campos son necesarios')
        return
      }
      try {
       const carritoId = this.$route.params.id
       const url = 'http://localhost:4500/venta/create'
        const body = JSON.stringify({
          carritoId,
          productoId: this.productos,
          unidades: this.unidades,
        })
        const res = await fetch(url, {
          method: 'post',
          headers: {
            'Content-Type': 'application/json',
          },
          body,
        })
        const data = await res.json()
          console.log({ body })
        if (data.error) {
          alert(data.error)
        } else {
          console.log({ data }) // data
          this.$router.back()
        }
      } catch (err) {
        alert('Hubo un error al asignar el producto')
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
