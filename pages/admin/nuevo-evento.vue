<template>
  <div class="Nuevo-evento">
    <b-container fluid>
      <b-row class="text-center">
     
          <div class="saludo">
            <b-card title="Crear Nuevo Evento" class="mb-2">
              <b-card-text>
                Indica nombre del evento, fecha, número de carritos y si está o
                no activo.
              </b-card-text>
              <div>
                <b-form>
                  <b-form-input
                    class="mb-2 mr-sm-2 mb-sm-3"
                    id="name"
                    type="text"
                    placeholder="Nombre del Evento"
                    v-model="name"
                    required
                  ></b-form-input>
                  <b-form-input
                    class="mb-2 mr-sm-2 mb-sm-3"
                    id="date"
                    v-model="date"
                    type="date"
                    placeholder="Fecha del Evento MM/DD/AAAA"
                    required
                  ></b-form-input>
                  <b-form-input
                    class="mb-2 mr-sm-2 mb-sm-3"
                    id="cars"
                    v-model="cars"
                    type="text"
                    placeholder="Número de carritos activos"
                    required
                  ></b-form-input>
                  <b-form-checkbox
                    class="mb-2 mr-sm-2 mb-sm-3"
                    id="activo"
                    v-model="activo"
                    value="true"
                    unchecked-value="false"
                 
                  >
                    <b>Activo</b>
                  </b-form-checkbox>

                  <b-button
                  @click="NewEvent"
                    size="lg"
                    variant="success"
                    class="mb-2 mr-sm-2 mb-sm-3"
                    >Crear Nuevo Evento</b-button
                  >
                  <p><nuxt-link to="./eventos">Volver</nuxt-link></p>
                </b-form>
              </div>
            </b-card>
          </div>
      </b-row>
    </b-container>
  </div>
</template>


<script>
export default {
    
 data () {
    return {    
      name: '',
      date: '',
      cars: '',
      activo: '',    
    }
  },

    // si no tiene token le echamos a login
beforeMount() {
    const token = window.localStorage.getItem('token')

    if (!token) {
      this.$router.push('/sign-in')
    }

       const admin = window.localStorage.getItem('admin')
      if ( admin === 'false' ) {
      this.$router.push('../menu')  
    }
        
},
  methods: {

   
    async NewEvent() {
      console.log("Crear evento")
      if (!this.name || !this.date || !this.cars ) {
        alert('Todos los campos son necesarios')
        return
      }
      try {
        const url = 'http://localhost:4500/event/create'
        const body = JSON.stringify({
          name: this.name,
          date: this.date,
          cars: this.cars,
          activo: this.activo,
        })
        const res = await fetch(url, {
          method: 'post',
          headers: {
            'Content-Type': 'application/json',
          },
          body
        })
        const data = await res.json()
        if (data.error) {
          alert(data.error)
        } else {
          console.log({ data }) // data
          this.$router.push('./eventos')
        }
      } catch (err) {
        alert('Hubo un error al crear el evento')
      }
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
