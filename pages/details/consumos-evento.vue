<template>
  <div class="Evento">
    <b-container fluid>
      <b-row class="text-center">
          <div class="saludo">
            <b-card title="Inventario del Evento" class="mb-2">
              <b-card-text>
                Detalles del evento XXX<br>
                Activo: Si<br>
                Asignado a Elena_Perez@gmail.com
              </b-card-text >
              <div class="table details">
               <b-table striped hover :items="consumos"></b-table>
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
    data() {
      return {
        consumos: [
          { producto: "Helado de Fresa", carrito_1: 4, carrito_2: 11, total: 15 },
          { producto: "Helado de Vainilla", carrito_1: 1, carrito_2: 0, total: 1 },
          { producto: "Cucurucho de Pistacho", carrito_1: 7, carrito_2: 5, total: 12 },
          { producto: "Cucurucho de Chocolate", carrito_1: 12, carrito_2: 1, total: 13 },
        { producto: "Helado de Yogurt", carrito_1: 4, carrito_2: 11, total: 15 },
          { producto: "Helado de Limon", carrito_1: 1, carrito_2: 0, total: 1 },
          { producto: "Cucurucho de Fresa", carrito_1: 7, carrito_2: 5, total: 12 },
          { producto: "Cucurucho de Nata", carrito_1: 12, carrito_2: 1, total: 13 },
        ]
      }
    }
  }
</script>

<style>
.saludo {
  margin-top: 40px !important;
  padding: 40px;
}
</style>
