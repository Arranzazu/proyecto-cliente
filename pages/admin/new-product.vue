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

                <b-form-select
                  v-model="category"
                  size="sm"
                  placeholder="seleccione usuario"
                >
                  <template #first>
                    <b-form-select-option
                      v-for="category in categorys"
                      :key="category._id"
                      v-bind:value="category.categoryname"
                      >{{ category.categoryname }}</b-form-select-option
                    >
                  </template>
                </b-form-select>
                <br /><br />

                <!-- <b-form-input
                  v-model="category"
                  id="inline-form-input-name"
                  class="mb-2 mr-sm-2 mb-sm-3"
                  placeholder="Categoría"
                  type="text"
                  required
                ></b-form-input> -->

                <!-- <b-form-input
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
                </b-form-checkbox> -->

                <b-button block variant="primary" @click="onSave"
                  >Guardar</b-button
                >
              </b-form>
            </div>
          </b-card>
          <br /><b-button
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
      categoryId: data.categoryname,
      categorys: data.categorys,
    }
  },

  methods: {
    async onSave() {
      console.log('Crear producto')
      if (!this.product || !this.category) {
        alert('Todos los campos son necesarios')
        return
      }
      try {
        const url = 'http://localhost:4500/almacen/create'
        const body = JSON.stringify({
          product: this.product,
          category: this.category,

          active: true,
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
          alert('Producto añadido al almacén')
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
