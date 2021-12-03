<template>
  <div class="new-product">
    <b-container fluid>
      <b-row class="text-center">
        <div class="saludo">
          <b-card title="Añadir Producto" class="mb-2">
            <b-card-text>
              Introduce el nombre del producto, la categoría y las unidades y
              pulsa guardar
            </b-card-text>
            <div>
              <b-form>
                <b-form-input
                  required
                  v-model="product"
                  id="inline-form-input-name"
                  class="mb-2 mr-sm-2 mb-sm-3"
                  placeholder="Nombre del producto"
                  type="text"
                ></b-form-input>

                <b-form-input
                  v-model="category"
                  id="inline-form-input-name"
                  class="mb-2 mr-sm-2 mb-sm-3"
                  placeholder="Categoría"
                  type="text"
                  required
                ></b-form-input>

                <b-form-input
                  required
                  v-model="unids"
                  id="inline-form-input-name"
                  class="mb-2 mr-sm-2 mb-sm-3"
                  placeholder="Unidades"
                  type="text"
                ></b-form-input>
                <b-form-checkbox
                  class="mb-2 mr-sm-2 mb-sm-3"
                  id="activo"
                  v-model="active"
                  value="true"
                  unchecked-value="false"
                >
                  <b>Activo</b>
                </b-form-checkbox>

                <b-button block variant="primary" @click="onSave"
                  >Guardar</b-button
                > </b-form>
                <p><nuxt-link to="./almacen">Volver</nuxt-link></p>
             
            </div>
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
      product: '',
      category: '',
      unids: '',
      active: '',
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
    async onSave() {
      console.log('Crear producto')
      if (!this.product || !this.category || !this.unids) {
        alert('Todos los campos son necesarios')
        return
      }
      try {
        const url = 'http://localhost:4500/almacen/create'
        const body = JSON.stringify({
          product: this.product,
          category: this.category,
          unids: this.unids,
          active: this.active,
        })
        const res = await fetch(url, {
          method: 'post',
          headers: {
            'Content-Type': 'application/json',
          },
          body,
        })
        const data = await res.json()
        if (data.error) {
          alert(data.error)
        } else {
          console.log({ data }) // data
          alert("Producto añadido al almacén")
          this.$router.push('./almacen')
        }
      } catch (err) {
        alert('Hubo un error al crear el evento')
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
