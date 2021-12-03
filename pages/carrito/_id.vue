<template>
  <div class="Evento">
    <b-container fluid>
      <b-row class="text-center">
          <div class="saludo">
            <b-card title="Detalles del Carrito" class="mb-2">
              <b-card-text>
           <b> {{ data.carrito.nombrecar }}</b> <br>
                Evento activo: {{ data.carrito.evento.activo }}<br>
                <!-- Asignado a {{carrito.user.email}} -->
              </b-card-text >
              <div class="table details">
               <b-table>
                 
               </b-table>
              </div>
                <b-button
                    router-link
                    to="/admin/asignar-producto"
                    size="lg"
                    variant="success"
                    class="mb-2 mr-sm-2 mb-sm-3"
                    >Asignar Producto</b-button
                  >
            </b-card>
          </div>
      </b-row>
    </b-container>
  </div>
</template>

<script>
  export default {
    async asyncData () {
    const url = 'http://localhost:4500/user/all'//listado usuarios
    const res = await fetch(url)
    const data = await res.json()
    const url2 = 'http://localhost:4500/carrito/all'//listado  carritos
    const res2 = await fetch(url2)
    const data2 = await res2.json()
    return {
      users: data.users,
      carritos: data2.carritos,
    }
    },
  async asyncData (ctx) {
      try {
    const id = ctx.params.id
    const url = `http://localhost:4500/carrito/get/${id}` //InfoEvent
    const res = await fetch(url)
    const data = await res.json()

          console.log({ data })
            
    return {
     
      data,
      // eventId: ctx.params.id,
      }
   
    } catch (_) {
      console.log('Hubo un problema!')
      // ctx.redirect('/home')
    }
  }}
</script>

<style>
.saludo {
  margin-top: 40px !important;
  padding: 40px;
}
</style>
