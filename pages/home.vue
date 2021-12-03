<template>
  <div class="inv-home">
       <b-container fluid>
             <b-row class="text-center"  >
   
          <div class="saludo">
              <b-card title="Bienvenido a helados Malvarrosa" sub-title="Por favor, seleccione una opción">
    <b-card-text>
      
    </b-card-text>

    <b-card-text>  
  <b-button size="lg" variant="primary" router-link to="/sign-in" nuxt><b-icon icon="people">  </b-icon>Conectar Usuario</b-button></b-card-text>

       <b-card-text>  
<b-button  size="lg" router-link to="/sign-up" nuxt><b-icon icon="person-plus">  </b-icon>Nuevo Usuario</b-button></b-card-text>
  </b-card>      
      
          </div>

  </b-row>

       </b-container>
  </div>
</template>

<script>
import { BIcon, BIconPersonPlus, BIconPeople } from 'bootstrap-vue'
export default {
    components: {
    BIcon,
    BIconPersonPlus,
    BIconPeople,
  },
 

methods: {
 
 
},
}
</script>
<style>
.saludo {
  margin-top: 80px !important;
  
}

</style>
