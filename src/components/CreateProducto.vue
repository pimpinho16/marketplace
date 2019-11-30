<template>
<div id="app">
    <vue-form-generator :schema="schema"></vue-form-generator>
</div>
</template>
<script>
import VueFormGenerator from "vue-form-generator"
export default{
    name:"app",
    components: {
            "vue-form-generator": VueFormGenerator.component
        },
    data(){
        return {
           /* model:{
                precioregular: 0,
                preciofinal: precioCalculado()
            },*/
            schema:{
                fields:[
                    {
                    type: "input",
                    inputType: "text",
                    label: "Empresa",
                    model: "empresa",
                    placeholder: "Empresa",
                    featured: true,
                    required: true
                    },
                    {
                    type: "input",
                    inputType: "text",
                    label: "Producto",
                    model: "producto",
                    placeholder: "Nombre de producto",
                    featured: true,
                    required: true
                    },
                    {
                    type: "input",
                    inputType: "text",
                    label: "Descripcion",
                    model: "descripcion",
                    placeholder: "Descripcion",
                    featured: true,
                    required: true
                    },
                    {
                    type: "input",
                    inputType: "text",
                    label: "Precio Regular",
                    model: "precioregular",
                    placeholder: "Precio Regular",
                    featured: true,
                    required: true
                    },
                    {
                    type: "input",
                    inputType: "text",
                    label: "Descuento",
                    model: "descuento",
                    placeholder: "Descuento",
                    featured: true,
                    required: true
                    },
                     {
                    type: "input",
                    inputType: "text",
                    label: "Precio final",
                    model: "preciofinal",
                    placeholder: "Precio Final",
                    featured: false,
                    required: true,
                    enable: false
                    },
                ]
            }
           
         }        
}
}
</script>