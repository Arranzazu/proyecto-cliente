<template>
  <div class="new-product">
    <b-container fluid>
      <b-row class="text-center">
        <div class="saludo">
          <b-card title="Crear una categoría" class="mb-2">
                <b-card-text class="text-center">
                    <b>Categorías actuales:</b>
                  
                     <b-list-group><b-list-group-item v-for="category in categorys" :key="category.categoryname">
                     {{category.categoryname}}</b-list-group-item
                    ></b-list-group
                  >
                  <br>
                  
                </b-card-text>
            <b-card-text>
               <b> Nueva Categoría:</b><br>
              Introduce el nombre de la categoría que quieras crear
            </b-card-text>
            <div>
              <b-form>
                <b-form-input
                  required
                  v-model="categoryname"
                  id="inline-form-input-name"
                  class="mb-2 mr-sm-2 mb-sm-3"
                  placeholder="Nombre de la nueva categoría"
                  type="text"
                ></b-form-input>

                <b-button block variant="primary" @click="onSave"
                  >Crear Categoría</b-button
                >
              </b-form>
             
            
            </div>
          </b-card> <br /> <b-button
                size="sm"
                variant="outline-primary"
                class="mb-2 mr-sm-2 mb-sm-3"
                @click="volver"
              >
                Volver</b-button
              >
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
      category: null,
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
    if (admin === 'false') {
      this.$router.push('../menu')
    }
  },

  async asyncData() {
    const url = 'http://localhost:4500/category/all' //listado categorías
    const res = await fetch(url)
    const data = await res.json()

    return {
      categoryname: data.categoryname,
      categorys: data.categorys,
    }
  },

  methods: {
    async onSave() {
      console.log('Crear categoría')
      if (!this.categoryname) {
        alert('Todos los campos son necesarios')
        return
      }
      try {
        const url = 'http://localhost:4500/category/create'
        const body = JSON.stringify({
          categoryname: this.categoryname,
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
          alert('Categoría creada')
          this.$router.push('./productos')
        }
      } catch (err) {
        alert('Hubo un error al crear el evento')
      }
    },
    volver() {
      this.$router.back()
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
