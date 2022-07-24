<template> 
  <div class="container">
    <h2>Agregar nuevo producto</h2>
    <form class="form-producto" action="" @submit.prevent="onSubmit">
        <div class="row">
          
          <div class="col-12">
              <div class="array-error row" v-if="errors.length > 0">
                <p>Errores detectados:</p>
                <ul class="text-danger fw-bold" id="error">
                  <li v-for="error in errors" v-bind:key="error.index">{{ error }}</li>
                </ul>
              </div>
          </div>
          <div class="col-6 align-left">
            <div class="col-12 row">
              <label for="titulo" class="require">Nombre</label>
              <input type="text" class="form-control" id="titulo" placeholder="Ingresar nombre del producto" v-model.lazy="titulo" name="titulo">

              <div class="alert alert-success" role="alert" v-if="errorsAlert.titulo == true">Correcto</div>
              <div class="alert alert-danger" role="alert" v-if="errorsAlert.titulo ==false">Campo requerido. Min 3 caracteres.</div>
            </div>  

            <div class="col-12 row">
              <label for="descripcion" class="require">Descripción</label>
              <textarea class="form-control" id="descripcion" placeholder=" Ingresar descripción" v-model.trim="descripcion" rows="3" name="descripcion"></textarea>

              <div class="alert alert-success" role="alert" v-if="errorsAlert.descripcion == true">Correcto</div>
              <div class="alert alert-danger" role="alert" v-if="errorsAlert.descripcion == false">Campo requerido. Min 3 caracteres y Max 120 caracteres.</div>       
            </div>
            <div class="col-12 row">
              <label for="precio" class="require">Precio</label>
              <input type="number" class="form-control" id="precio" placeholder="Ingresar precio" v-model.number="precio" name="precio">
                <div class="alert alert-success" role="alert" v-if="errorsAlert.precio == true">Correcto</div>
                <div class="alert alert-danger" role="alert" v-if="errorsAlert.precio == false">Campo requerido. Solo se puede ingresar valores numericos.</div>            
            </div>             
          </div>
          <div class="col-6 align-left">
            <div class="col-12 row">
              <label for="producto" class="require" >Tipo de producto</label>
                  <select id="producto" class="form-select" name="productos" v-model="productos">
                    <option disabled value="selected">Selecione una opción</option>
                    <option 
                    v-for="producto in listaProductos" 
                    :value="producto.producto" 
                    :key="producto.id">{{producto.producto}}</option>
                  </select>
                  <div class="alert alert-success" role="alert" v-if="errorsAlert.producto == true">Correcto</div>
                  <div class="alert alert-danger" role="alert" v-if="errorsAlert.producto == false">Campo requerido. Debe seleccionar alguna opción.</div>                 
            </div>

            <div class="col-12 row" v-if="tipoTalle === 'calzado'">
              <label for="talle" class="require">Talles del producto</label>
                <div class="row">
                    <div class="form-check col-4" v-for="(talle, index) in talleZapatillas" :key="index">
                      <label class="form-check-label" :for="talle">{{talle}}</label>
                      <input :id="talle" class="form-check-input" type="checkbox" :value="talle" v-model="checksTalle">
                  </div>
                </div>
            </div>  

            <div class="col-12" v-if="tipoTalle === 'ropa'">
                <label for="talle" class="require">Talles del producto</label>
                  <div class="row">
                    <div class="form-check col-4" v-for="(talle, index) in talleRopa" :key="index">
                        <label class="form-check-label" :for="talle">{{talle}}</label>
                        <input :id="talle" class="form-check-input" type="checkbox" :value="talle" v-model="checksTalle">
                    </div> 
                </div>           
            </div>

            <div class="col-12 row margin-top">
                <div class="alert alert-success" role="alert" v-if="errorsAlert.talle == true">Correcto</div>
                <div class="alert alert-danger" role="alert" v-if="errorsAlert.talle == false">Campo requerido. Debe seleccionar al menos 1 opción.</div>
            </div>

          </div>                        
        </div>    
        <input type="submit" class="btn btn-primary" value="Cargar producto" />
    </form>
    <TablePage class="table" :lista="datosProducto" @eliminarProducto="eliminar" />
  </div>
</template>

<script>
import TablePage from './TablePage.vue'

export default {
  name: 'FormularioWeb',
  components: {
    TablePage
  },      
  data() {
      return{
        titulo:"",
        descripcion:"",
        precio: 0,
        listaProductos: [
          {id: 0 ,producto: "Zapatilla"},
          {id: 1 ,producto: "Remeras"},
          {id: 2 ,producto: "Pantalones"},
          {id: 3 ,producto: "Buzos y camperas"}
        ],
        productos: "selected",
        talleZapatillas: ["35","36","37","38","39","40","41","42","43","44","45","46"],
        talleRopa: ["S", "M", "L", "XL", "XXL"],        
        tipoTalle: "",
        checksTalle: [],
        errorsAlert:{titulo: null, descripcion: null, producto: null, talle: null, precio: null},
        errors: [],
        RegexTitulo: /[\w\D]{3,}/, 
        RegexDescripcion: /[\w\D]{3,120}/,
        RegexNumerico: /[0-9]+/,
        datosProducto: []
      }
  },
  methods:{
    onSubmit: function () {
      this.errors =[];
      let rts = [];

      //Validacion Titulo
      if(this.titulo == "" || !this.RegexTitulo.test(this.titulo)){
        this.errors.push('Campo Titulo requerido. Min 3 caracteres.');
        rts['titulo'] = false;
      }else{
        rts['titulo'] = true;
      }   

      //Validacion Descripcion
      if(this.descripcion == "" || !this.RegexDescripcion.test(this.descripcion)){
        this.errors.push('Campo Descripcion requerido. Min 3 caracteres y Max 120 caracteres.');
        rts['descripcion'] = false;
      }else{
          rts['descripcion'] = true;
      } 

      //Validacion Producto
      if(this.productos == "selected"){ 
        this.errors.push('Campo Producto requerido. Debe seleccionar alguna opción.');
        rts['productos'] = false;
      }else{
          rts['productos'] = true;
      }    

      //Validacion Talle
      if(this.checksTalle == "" || this.checksTalle.length < 0){ 
          this.errors.push('Campo Talle requerido. Debe seleccionar al menos 1 opción.');
          rts['talle'] = false;
      }else{
          rts['talle'] = true;
      }

      //Validacion Precio
      if(!this.RegexNumerico.test(this.precio) || this.precio == 0){ 
        this.errors.push('Campo Precio requerido. Solo se puede ingresar valores numericos.');
        rts['precio'] = false;
      }else{
          rts['precio'] = true;
      }
      
      //Objeto para la tabla de producto
      if(rts['titulo'] && rts['descripcion'] && rts['productos'] && rts['precio'] && rts['talle']){
        this.datosProducto =[...this.datosProducto, {titulo: this.titulo, descripcion: this.descripcion, producto: this.productos, talle: this.checksTalle,precio: this.precio}]

        const parsed = JSON.stringify(this.datosProducto);
        localStorage.setItem('Productos', parsed); 
      }     
    },
    vTitulo(value){
      if(this.RegexTitulo.test(value)){
          this.errorsAlert.titulo = true;
      }else{
        this.errorsAlert.titulo = false;
      }
    },
    vDescricion(value){
      if(this.RegexDescripcion.test(value)){
          this.errorsAlert.descripcion = true;
      }else{
          this.errorsAlert.descripcion = false;
      }
    },
    vProductos(value){
      if(this.productos !== "selected"){
          if(value == "Zapatilla"){
            this.tipoTalle = "calzado";
          }else{
            this.tipoTalle = "ropa";
          }
      
          this.errorsAlert.producto = true;
      }else{
          this.errorsAlert.producto = false;
      }
    },
    vTalle(value){
      if(value.length > 0){      
          this.errorsAlert.talle = true;
      }else{
          this.errorsAlert.talle = false;
      }
    },
    vPrecio(value){
      if(this.RegexNumerico.test(value) || value != 0){      
          this.errorsAlert.precio = true;
      }else{
          this.errorsAlert.precio = false;
      }
    },
    eliminar(id){
        this.datosProducto.splice(id, 1);
    }
  },
  watch: {
    titulo(value){
      this.titulo = value
      this.vTitulo(value)
    },
    descripcion(value){
      this.descripcion = value
      this.vDescricion(value)
    },
    productos(value){
      this.productos = value
      this.vProductos(value)
    },
    checksTalle(value){
      this.checksTalle = value
      this.vTalle(value)
    },    
    precio(value){
      this.precio = value
      this.vPrecio(value)
    }    
  },
}
</script>

<style scoped>
.form-producto{
  margin-bottom: 40px;

}
h2{
    text-transform: capitalize;
    margin-bottom: 40px;
}
.require:after{
  content: " *";
  color: rgb(237, 107, 117);
}
.align-left{
  text-align: left;
}
input, textarea, select{
  margin:10px 0 20px;
}
.form-check{
  text-align: left;
}
label{
  padding-left: 0;
}
.form-check input{ 
  margin-top: 0.25em;
  margin-bottom:0;
}

.margin-top{
  margin-top: 20px;

}

.array-error{
  border:1px solid #ccc;
  border-radius: 10px;
  padding: 20px;
  margin:20px auto;
}
.array-error p{
  font-weight: 600;
  text-decoration: underline;
}
.array-error ul{
  list-style: none;
}
.array-error ul li{
  margin-bottom: 5px;

}
</style>
