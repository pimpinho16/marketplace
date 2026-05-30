<template>
  <div class="marketplace-dashboard">
    <header class="dashboard-header">
      <div class="header-content">
        <span class="badge">Marketplace Portal</span>
        <h1>Gestión de Productos</h1>
        <p>Añada, cotice y administre su catálogo de productos en tiempo real con cálculos automáticos.</p>
      </div>
    </header>

    <div class="dashboard-content">
      <!-- Left side: Form -->
      <div class="card form-card">
        <div class="card-header">
          <div class="card-icon">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <rect x="3" y="3" width="18" height="18" rx="2" ry="2"></rect>
              <line x1="12" y1="8" x2="12" y2="16"></line>
              <line x1="8" y1="12" x2="16" y2="12"></line>
            </svg>
          </div>
          <h2>Registrar Producto</h2>
        </div>
        <VueFormGenerator 
          :schema="schema" 
          v-model:model="model" 
          @submit="saveProduct" 
        />
      </div>

      <!-- Right side: Live List -->
      <div class="card list-card">
        <div class="card-header">
          <div class="card-icon list-icon">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <line x1="8" y1="6" x2="21" y2="6"></line>
              <line x1="8" y1="12" x2="21" y2="12"></line>
              <line x1="8" y1="18" x2="21" y2="18"></line>
              <line x1="3" y1="6" x2="3.01" y2="6"></line>
              <line x1="3" y1="12" x2="3.01" y2="12"></line>
              <line x1="3" y1="18" x2="3.01" y2="18"></line>
            </svg>
          </div>
          <h2>Catálogo Activo</h2>
        </div>
        <ListProducto :products="productsList" />
      </div>
    </div>
    
    <!-- Toast notification -->
    <Transition name="toast">
      <div v-if="showToast" class="toast-notification">
        <div class="toast-indicator"></div>
        <div class="toast-icon">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="20" height="20" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
            <path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"></path>
            <polyline points="22 4 12 14.01 9 11.01"></polyline>
          </svg>
        </div>
        <div class="toast-content">
          <h4>¡Producto Guardado!</h4>
          <p>El producto ha sido añadido con éxito al catálogo.</p>
        </div>
      </div>
    </Transition>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'
import VueFormGenerator from './VueFormGenerator.vue'
import ListProducto from './ListProducto.vue'

const model = reactive({
  empresa: '',
  producto: '',
  descripcion: '',
  precioregular: '',
  descuento: '',
  preciofinal: ''
})

const productsList = ref([
  { 
    name: 'Nintendo 64', 
    precioregular: 100.0, 
    descuento: 10, 
    iniciovigencia: '10-Nov-2019', 
    finvigencia: '31-Dec-2019', 
    empresa: 'Radio Shark' 
  }
])

const showToast = ref(false)

const saveProduct = (formData) => {
  const newProduct = {
    name: formData.producto || 'Producto sin nombre',
    precioregular: parseFloat(formData.precioregular) || 0,
    descuento: parseFloat(formData.descuento) || 0,
    iniciovigencia: new Date().toLocaleDateString('es-ES', { day: 'numeric', month: 'short', year: 'numeric' }),
    finvigencia: new Date(Date.now() + 30 * 24 * 60 * 60 * 1000).toLocaleDateString('es-ES', { day: 'numeric', month: 'short', year: 'numeric' }),
    empresa: formData.empresa || 'General'
  }
  
  productsList.value.push(newProduct)
  
  // Reset form model
  model.empresa = ''
  model.producto = ''
  model.descripcion = ''
  model.precioregular = ''
  model.descuento = ''
  model.preciofinal = ''
  
  // Show toast
  showToast.value = true
  setTimeout(() => {
    showToast.value = false
  }, 4000)
}

const schema = {
  fields: [
    {
      type: "input",
      inputType: "text",
      label: "Empresa",
      model: "empresa",
      placeholder: "Ingrese el nombre de la empresa",
      featured: true,
      required: true
    },
    {
      type: "input",
      inputType: "text",
      label: "Producto",
      model: "producto",
      placeholder: "Ingrese el nombre del producto",
      featured: true,
      required: true
    },
    {
      type: "input",
      inputType: "text",
      label: "Descripción",
      model: "descripcion",
      placeholder: "Describa brevemente el producto",
      featured: true,
      required: true
    },
    {
      type: "input",
      inputType: "number",
      label: "Precio Regular ($)",
      model: "precioregular",
      placeholder: "0.00",
      featured: true,
      required: true
    },
    {
      type: "input",
      inputType: "number",
      label: "Descuento (%)",
      model: "descuento",
      placeholder: "0",
      featured: true,
      required: true
    },
    {
      type: "input",
      inputType: "text",
      label: "Precio Final Calculado ($)",
      model: "preciofinal",
      placeholder: "0.00",
      featured: false,
      required: true,
      enable: false
    }
  ]
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;500;600;700&display=swap');

body {
  margin: 0;
  padding: 0;
  background-color: #f4f7f6;
  font-family: 'Outfit', sans-serif;
  color: #1f2937;
  min-height: 100vh;
}

.marketplace-dashboard {
  max-width: 1300px;
  margin: 0 auto;
  padding: 40px 20px;
}

.dashboard-header {
  margin-bottom: 40px;
  text-align: left;
}

.badge {
  background: rgba(66, 185, 131, 0.12);
  color: #3aa372;
  padding: 6px 14px;
  border-radius: 30px;
  font-size: 0.85rem;
  font-weight: 600;
  letter-spacing: 0.5px;
  display: inline-block;
  margin-bottom: 12px;
  border: 1px solid rgba(66, 185, 131, 0.2);
}

.dashboard-header h1 {
  font-size: 2.5rem;
  font-weight: 700;
  color: #111827;
  margin: 0 0 10px 0;
  letter-spacing: -0.5px;
}

.dashboard-header p {
  color: #6b7280;
  font-size: 1.1rem;
  margin: 0;
  max-width: 600px;
}

.dashboard-content {
  display: grid;
  grid-template-columns: 1.1fr 1.9fr;
  gap: 30px;
  align-items: start;
}

@media (max-width: 1024px) {
  .dashboard-content {
    grid-template-columns: 1fr;
  }
}

.card {
  background: white;
  border-radius: 20px;
  border: 1px solid #e5e7eb;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05), 0 2px 4px -1px rgba(0, 0, 0, 0.03);
  overflow: hidden;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.card:hover {
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.08), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
}

.card-header {
  padding: 24px 30px;
  border-bottom: 1px solid #f3f4f6;
  display: flex;
  align-items: center;
  gap: 14px;
}

.card-icon {
  background: #ecfdf5;
  color: #10b981;
  padding: 10px;
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.list-icon {
  background: #eff6ff;
  color: #3b82f6;
}

.card-header h2 {
  font-size: 1.25rem;
  font-weight: 600;
  color: #111827;
  margin: 0;
}

/* Toast styling */
.toast-notification {
  position: fixed;
  bottom: 30px;
  right: 30px;
  background: white;
  border-radius: 16px;
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.15), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
  padding: 16px 24px;
  display: flex;
  align-items: center;
  gap: 16px;
  z-index: 1000;
  border: 1px solid #e5e7eb;
  overflow: hidden;
  max-width: 380px;
}

.toast-indicator {
  position: absolute;
  left: 0;
  top: 0;
  bottom: 0;
  width: 6px;
  background: #10b981;
}

.toast-icon {
  color: #10b981;
  background: #ecfdf5;
  padding: 8px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.toast-content h4 {
  margin: 0 0 4px 0;
  color: #111827;
  font-weight: 600;
}

.toast-content p {
  margin: 0;
  color: #6b7280;
  font-size: 0.9rem;
}

/* Toast transitions */
.toast-enter-active,
.toast-leave-active {
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.toast-enter-from {
  transform: translateY(100px) scale(0.9);
  opacity: 0;
}

.toast-leave-to {
  transform: scale(0.9);
  opacity: 0;
}
</style>