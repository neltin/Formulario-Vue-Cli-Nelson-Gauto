<template>
    <div class="col-12 f-table">
            <h2>Tabla de Producto</h2>
            <table class="table table-warning">
                <thead>
                    <tr>
                        <th :style="style">Titulo</th>
                        <th :style="style">Descripcion</th>
                        <th :style="style">Producto</th>
                        <th :style="style">Talles</th>
                        <th :style="style">Precio</th>
                        <th :style="style">Eliminar</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(data, key) in lista" :key="key"> 
                        <td>{{data.titulo}}</td>
                        <td>{{data.descripcion}}</td>
                        <td>{{data.producto}}</td>
                        <td><span v-for="(data2, key2) in data.talle" :key="key2">{{data2}} </span></td>
                        <td>${{data.precio}}</td>
                        <td><a class="eliminar" @click="eliminar(key)">Eliminar</a></td>
                    </tr>
                </tbody>
            </table>
        </div>
</template>

<script>
export default {
  name: 'TablePage',
  props: {
    lista: Array,
  },
  data(){
    return{
        style: { 'background-color': '#000','color': '#f2e7c3'}
    }
  },
  methods:{
    eliminar(id){
        this.$emit('eliminarProducto', id);

    }
  }
}
</script>

<style scoped>
    h2{
        text-transform: capitalize;
        margin-bottom: 40px;
    }

    .f-table{
        margin-bottom: 25px;
    }
    .eliminar{
        cursor: pointer;
    }
</style>