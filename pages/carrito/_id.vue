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
              Fecha:<b> {{ data.carrito.evento.date.substring(0,10)}} </b
              ><br />
              Evento activo:<b v-if="data.carrito.evento.activo === false"> No </b>
              <b v-if="data.carrito.evento.activo === true"> Si </b>
              <br /> 
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
             <br>
              A continuación escriba los consumos en cada producto <b>siempre con número positivo</b> (sin el -)
            </b-card-text>

            <div class="table details">
              <table class="table">
                <tr>
                  <th>Producto</th>
                  <th>Unids.</th>
                  <th>Consumos</th>
                  <th>Devolución</th>
                </tr>
                <tr v-for="asignado in asignados" v-bind:key="asignado._id">
                  <!-- <tr
                  v-for="asignado, index in asignados"
                  v-bind:key="asignado._id"
            
                > -->
                  <!-- <td>{{ unidscons[index] }}</td> -->
                  <td
                    v-text="asignado.producto.product"
                    v-if="asignado.carrito === carritoId"
                  ></td>

                  <td
                    v-text="asignado.unidades"
                    id="unidades1"
                    v-if="asignado.carrito === carritoId"
                  ></td>

                  <td v-if="asignado.carrito === carritoId">
                    <b-form-input         
                      id="unidscons"
                      :key="asignado._id"
                      v-model="asignado.venta"
                      >{{ asignado.venta }}</b-form-input
                    >
                  </td>

                
                  <td
                    v-text="asignado.unidades + asignado.venta"
                    v-if="asignado.carrito === carritoId"
                  ></td>
                </tr>
              </table>
            </div>

            <div v-if="isAdmin">
              <b-button
                @click="asignado(carritoId)"
                size="lg"
                variant="success"
                class="mb-2 mr-sm-2 mb-sm-3"
                >Asignar Producto</b-button
              >
            </div>
            <b-button
              @click="enviarprod()"
              size="lg"
              variant="warning"
              class="mb-2 mr-sm-2 mb-sm-3"
              >Validar consumos</b-button
            >
            <!-- <b-button
                @click="editarconsumo()"
                size="lg"
                variant="warning"
                class="mb-2 mr-sm-2 mb-sm-3"
                >Validar consumos</b-button
              > -->

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
      unidscons: {},
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

      const url5 = 'http://localhost:4500/asignado/all' //listado asignados
      const res5 = await fetch(url5)
      const data5 = await res5.json()

      const url6 = 'http://localhost:4500/consumo/all' //listado consumoss
      const res6 = await fetch(url6)
      const data6 = await res6.json()


      return {
        data,
        data3,
        data2,
        data4,
        data5,
        data6,
        events: data2.events,
        products: data3.products,
        users: data4.users,
        asignados: data5.asignados,
        consumos: data6.consumos,
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
    consumo() {
      console.log('consumo')
    },

    asignado(carritoId) {
      console.log('asignado')
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


    async enviarprod() {
      console.log('asignando enviarprod')
      let index = 0
      for (const asignado of this.asignados) {
        if (asignado.carrito === this.carritoId) {
          await this.NewCons(asignado.producto._id, index)

          console.log('asignando', asignado)
        }
        index++ 
      }
    },

    async NewCons(_id, index) {
      console.log(
        'Editar consumo ',
        this.asignados[index].venta,
        index,
        this.carritoId
      )

      try {
        const carritoId = this.$route.params.id
        const url = 'http://localhost:4500/consumo/'+this.asignados[index]._id
        const body = JSON.stringify({
          //   carritoId,
          //  productoId:_id,
          // unidades: this.unidscons[index],
          venta: (this.asignados[index].venta)*-1,
        
        })
        const res = await fetch(url, {
          method: 'put',
          headers: {
            'Content-Type': 'application/json',
          },
          body,
        })
        const data = await res.json()
        //  alert('Consumos reasignados. Puede volver a modificarlos en caso de que sea necesario hasta que se desactive el evento')
         await this.$router.home
         console.log({ body })
        if (data.error) {
          alert(data.error)
        } else {
          console.log({ data }) // data
         
        }
      } catch (err) {
        alert('Hubo un error al asignar el producto')
      }
    },

    //     async enviarprod(){
    //       console.log("asignando")
    //       let index = 0;
    //         for (const asignado of this.asignados)
    //         {
    //           if (asignado.carrito === this.carritoId) {
    //                  await this.NewCons(asignado.producto._id, index)

    //                  console.log('asignando', asignado)
    //           }
    //       index++
    //           }

    //     },

    // async NewCons(_id, index) {
    //       console.log('Crear consumo ', this.unidscons[index], index, this.carritoId )

    //       try {
    //        const carritoId = this.$route.params.id
    //        const url = 'http://localhost:4500/consumo/create'
    //         const body = JSON.stringify({
    //           carritoId,
    //          productoId:_id,
    //           unidades: this.unidscons[index],
    //         })
    //         const res = await fetch(url, {
    //           method: 'post',
    //           headers: {
    //             'Content-Type': 'application/json',
    //           },
    //           body,
    //         })
    //         const data = await res.json()
    //           console.log({ body })
    //         if (data.error) {
    //           alert(data.error)
    //         } else {
    //           console.log({ data }) // data
    //           this.$router.back()
    //         }
    //       } catch (err) {
    //         alert('Hubo un error al asignar el producto')
    //       }
    //     },

    async updateData(id) {
      const actualize = `http://localhost:4500/carrito/${id}`
      const res = await fetch(actualize)
      const data = await res.json()
      this.carritoId = data.carritoId
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
