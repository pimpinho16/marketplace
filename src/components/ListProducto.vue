<template>
  <div id="app">
    <h3>Listado de productos</h3>
   <table style="center">
     <thead>
     <th v-bind:key="key" v-for="key in columns">{{key}}</th>
     </thead>
     <tbody >
      <tr v-bind:key="prod" v-for="prod in products"  >
        <td>{{prod.name}}</td>
        <td>{{prod.precioregular}}</td>
        <td>{{prod.descuento}}</td>
        <td>{{prod.iniciovigencia}}</td>
        <td>{{prod.finvigencia}}</td>
        <td>{{prod.empresa}}</td>
      </tr>
     </tbody>
   </table>
  </div>
</template>

<script>


export default {
  name: 'app',
  data(){
    return{
      products:[
        {name:'Nintendo 64',precioregular:100.0,descuento:10,iniciovigencia:"10-Nov-2019",finvigencia:"31-Dec-2019",empresa:"Radio Shark" }

      ],
      columns:[
        "Producto",
        "Precio Regular",
        "Descuento",
        "Inicio de vigencia descuento",
        "Fin de vigencia descuento",
        "Empresa"
      ]
    }
  }
  
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
table {
  border: 2px solid #42b983;
  border-radius: 3px;
  background-color: #fff;
}

th {
  background-color: #42b983;
  color: rgba(255,255,255,0.66);
  cursor: pointer;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

td {
  background-color: #f9f9f9;
}
</style>
