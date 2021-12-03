<template>
  <div class="Sign-in">
    <b-container fluid>
      <b-row class="text-center">
        <div class="saludo">
          <b-card title="Asignar Producto" class="mb-2">
            <b-card-text>
              Selecciona producto, unidades y carrito y pulsa "Asignar"
            </b-card-text>
            <div>
              <b-form
                >Producto:
                <b-form-select
                  v-model="eventos"
                  size="sm"
                  placeholder="seleccione evento"
                  class="mt-3 mb-sm-1"
                 
                >
                  <template #first>
                    <b-form-select-option
                      v-for="event in events"
                      :key="event._id"
                       :value="{event}"
                      >{{ event.name }}, {{ event.date }}</b-form-select-option
                    >
                  </template> </b-form-select
                ><br />
                Unidades:
                <b-form-input
                  class="mb-2 mr-sm-2 mb-sm-3"
                  id="carrito"
                  type="text"
                  placeholder="Unidades Asignadas"
                  v-model="carrito"
                  required
                ></b-form-input>
          Carrito Asignado:
                <b-form-input
                  class="mb-2 mr-sm-2 mb-sm-3"
                  id="carrito"
                  type="text"
                  placeholder="Número del carrito"
                  v-model="carrito"
                  required
                ></b-form-input>
                <b-button size="lg" class="mb-2 mr-sm-2 mb-sm-3" nuxt-link to="../details/consumos-evento"
                  >Asignar</b-button
                >
                  <div class="mt-2">Seleccionado: <strong>{{ eventos }} {{ usuarios }}</strong></div>
                <p><nuxt-link to="../details/consumos-evento">Volver</nuxt-link></p>
              </b-form>
            </div>
          </b-card>
        </div>
      </b-row>
    </b-container>
  </div>
</template>
<script>



</script>


<style>
.saludo {
  margin-top: 40px !important;
  padding: 40px;
}
</style>
