<template>
  <div class="Evento">
    <b-container fluid>
      <b-row class="text-center">
        <div class="saludo">
          <b-card title="Detalles del Carrito" class="mb-2">
            <b-card-text>
              <b>{{ data.carrito.evento.name }}, </b>carrito número<b>
                {{ data.carrito.numero }}</b
              >
              <br />
              Fecha:<b> {{ data.carrito.evento.date }}</b
              ><br />
              Evento activo:<b> {{ data.carrito.evento.activo }}</b
              ><br />
              <div v-if="isUser">
                Asignado a: <b>{{ myemail }}</b>
              </div>
              <div v-if="isAdmin">
                <b-row
                  ><b-col align="right">Asignado a: </b-col>
                  <b-col
                    ><b-form-select
                      v-model="userId"
                      size="sm"
                      placeholder="seleccione usuario"
                    >
                      <template #first>
                        <b-form-select-option
                          v-for="user in users"
                          :key="user._id"
                          v-bind:value="user._id"
                          >{{ user.email }}</b-form-select-option
                        >
                      </template>
                    </b-form-select> </b-col
                  ><b-col align="left">
                    <b-button @click="Reasignar">Reasignar</b-button></b-col
                  >
                </b-row>
              </div>
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
                  v-for="venta in ventas"
                  v-bind:key="venta._id"
                  v-on:click="editar(ventas._id)"
                >
                  <td
                    v-text="venta.producto"
                    v-if="venta.carrito === carritoId"
                  ></td>

                  <td
                    v-text="venta.unidades"
                    v-if="venta.carrito === carritoId"
                  ></td>
                  <td v-if="venta.carrito === carritoId">
                    <b-form-input
                      v-model="cons"
                      placeholder="Unids."
                    ></b-form-input>
                  </td>
                  <td
                    v-text="venta.unidades - cons"
                    v-if="venta.carrito === carritoId"
                  ></td>
                </tr>
              </table>
            </div>

            <div v-if="isAdmin">
              <b-button
                @click="venta(carritoId)"
                size="lg"
                variant="success"
                class="mb-2 mr-sm-2 mb-sm-3"
                >Asignar Producto</b-button
              >
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

      const url2 = 'http://localhost:4500/event/all' //listado eventos
      const res2 = await fetch(url2)
      const data2 = await res2.json()

      const url3 = 'http://localhost:4500/almacen/all' //listado productos
      const res3 = await fetch(url3)
      const data3 = await res3.json()

      const url4 = 'http://localhost:4500/user/all' //listado usuarios
      const res4 = await fetch(url4)
      const data4 = await res4.json()

      const url5 = 'http://localhost:4500/venta/all' //listado ventas
      const res5 = await fetch(url5)
      const data5 = await res5.json()

      return {
        data,
        data3,
        data2,
        data4,
        data5,
        events: data2.events,
        products: data3.products,
        users: data4.users,
        ventas: data5.ventas,
        userId: data.carrito.usuario._id,
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
    venta(carritoId) {
      console.log('venta')
      this.$router.push(`/venta/${carritoId}`)
    },
    async Reasignar() {
      const url = `http://localhost:4500/carrito/${this.carritoId}`
      const body = JSON.stringify({
        userId: this.userId,
      })
      try {
        const res = await fetch(url, {
          method: 'put',
          headers: {
            'Content-Type': 'application/json',
          },
          body,
        })
        const data = await res.json()
        if (data.error) {
          alert(data.error)
          return
        }
        alert('Carrito Reasignado')
        this.$router.back()
        // await this.updateData()
      } catch (err) {
        alert('Hubo un problema al actualizar el usuario')
      }
    },
    async updateData() {
      const actualize = `http://localhost:4500/carrito/${id}`
      const res = await fetch(actualize)
      const data = await res.json()
      this.carritos = data.carritos
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
