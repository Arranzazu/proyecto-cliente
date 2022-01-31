<template>
  <div class="Almacen">
    <b-container fluid>
      <b-row class="text-center">
        <div class="saludo">
          <b-card title="Almacén" class="mb-2"></b-card>
            <b-card-text> </b-card-text>

            
              <!-- Tabla hoy -->
              <b-table
                striped
                hover
                :items="items3"
                
              ></b-table>

              <!-- tabla abajo -->
              <b-table
                striped
                hover
                :items="items2"
                :fields="fields2"
              ></b-table>
          

            <b-button
              size="sm"
              variant="outline-primary"
              class="mb-2 mr-sm-2 mb-sm-3"
              @click="volver"
            >
              Volver</b-button
            ><br />
          
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
      ventas: [],

      items3: [],
      fields3: [],

      
    }
  },

  // si no tiene token le echamos a login
  beforeMount() {
    const token = window.localStorage.getItem('token')
    if (!token) {
      this.$router.push('/sign-in')
    }
    this.creartabla()
  },
  async asyncData() {
    const url = 'http://localhost:4500/almacen/all' //listadoproductos
    const res = await fetch(url)
    const data = await res.json()

    const url2 = 'http://localhost:4500/almacen/all2' //listadoventas
    const res2 = await fetch(url2)
    const data2 = await res2.json()

    return {
      products: data.products,
      ventas: data2.venta,
      data2,
    }
  },

  methods: {
    newproduct() {
      this.$router.push('/admin/new-product')
    },
    newcategory() {
      this.$router.push('/admin/new-category')
    },
    volver() {
      this.$router.back()
    },
    creartabla() {
      for (const product of this.products) {
        const obj = {}
        obj.Producto = product.product
        for (const venta of this.ventas) {
          obj[venta.nombre] = this.obtenerventa(product._id, venta) 
        }
        obj.total= this.obttotal(obj)
        this.items3.push(obj)
      }
    },
    obtenerventa(productid, venta){
      let total = 0
      for ( const producto of venta.productos) {
        console.log(productid, producto.id)
          if (productid.toString() === producto.id.toString()) {
            total += producto.venta
          }
      }
      return total
    },
    //  sumaventas(nombre){
    //    for (const obj of this.items3) {
    //      if (obj[nombre]) return obj[nombre]
    //    }
    //    return 0
    //  }
    obttotal(obj){
      let total = 0
      const lista = Object.values(obj)
      for ( const value of lista.slice(1,lista.length)){
        total += value
      }
      return total
    }
  },
 

}
</script>

<style>
.saludo {
  margin-top: 40px !important;
  padding: 40px;
}
</style>
