<template>
  <div class="list-container">
    <div class="table-responsive" v-if="localProducts.length > 0">
      <table class="premium-table">
        <thead>
          <tr>
            <th v-for="key in columns" :key="key">{{ key }}</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(prod, index) in localProducts" :key="index" class="table-row">
            <td class="prod-name-cell">
              <div class="prod-avatar">{{ prod.name.charAt(0) }}</div>
              <span>{{ prod.name }}</span>
            </td>
            <td class="numeric-cell">${{ formatCurrency(prod.precioregular) }}</td>
            <td>
              <span class="discount-badge" :class="{ 'no-discount': !prod.descuento }">
                {{ prod.descuento ? `${prod.descuento}% OFF` : 'Sin descuento' }}
              </span>
            </td>
            <td class="date-cell">{{ prod.iniciovigencia }}</td>
            <td class="date-cell">{{ prod.finvigencia }}</td>
            <td class="empresa-cell">
              <span class="empresa-badge">{{ prod.empresa }}</span>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    
    <div v-else class="empty-state">
      <div class="empty-icon">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="48" height="48" fill="none" stroke="currentColor" stroke-width="1.5">
          <circle cx="12" cy="12" r="10"></circle>
          <path d="M8 12h8"></path>
        </svg>
      </div>
      <h3>No hay productos disponibles</h3>
      <p>Registre su primer producto usando el formulario de la izquierda.</p>
    </div>
  </div>
</template>

<script setup>
import { computed, defineProps } from 'vue'

const props = defineProps({
  products: {
    type: Array,
    default: () => [
      { 
        name: 'Nintendo 64', 
        precioregular: 100.0, 
        descuento: 10, 
        iniciovigencia: "10-Nov-2019", 
        finvigencia: "31-Dec-2019", 
        empresa: "Radio Shark" 
      }
    ]
  }
})

const columns = [
  "Producto",
  "Precio Regular",
  "Descuento",
  "Inicio Vigencia",
  "Fin Vigencia",
  "Empresa"
]

const localProducts = computed(() => props.products)

const formatCurrency = (val) => {
  const num = parseFloat(val)
  return isNaN(num) ? '0.00' : num.toFixed(2)
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;500;600;700&display=swap');

.list-container {
  font-family: 'Outfit', sans-serif;
  padding: 8px;
}

.table-responsive {
  width: 100%;
  overflow-x: auto;
  border-radius: 14px;
}

.premium-table {
  width: 100%;
  border-collapse: separate;
  border-spacing: 0;
  text-align: left;
}

.premium-table th {
  background: #f8fafc;
  color: #475569;
  font-size: 0.85rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  padding: 16px 20px;
  border-bottom: 1.5px solid #e2e8f0;
}

.premium-table td {
  padding: 16px 20px;
  border-bottom: 1px solid #f1f5f9;
  color: #334155;
  font-size: 0.95rem;
  vertical-align: middle;
}

.table-row {
  transition: all 0.2s ease;
}

.table-row:hover {
  background-color: #f8fafc;
}

.table-row:last-child td {
  border-bottom: none;
}

/* Custom styled column items */
.prod-name-cell {
  display: flex;
  align-items: center;
  gap: 12px;
  font-weight: 500;
  color: #0f172a;
}

.prod-avatar {
  width: 32px;
  height: 32px;
  background: linear-gradient(135deg, #e0f2fe 0%, #bae6fd 100%);
  color: #0369a1;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 600;
  font-size: 0.9rem;
}

.numeric-cell {
  font-variant-numeric: tabular-nums;
  font-weight: 500;
}

.discount-badge {
  background: #dcfce7;
  color: #15803d;
  padding: 4px 10px;
  border-radius: 9999px;
  font-size: 0.75rem;
  font-weight: 600;
  display: inline-block;
}

.discount-badge.no-discount {
  background: #f1f5f9;
  color: #64748b;
}

.date-cell {
  color: #64748b;
  font-size: 0.85rem;
}

.empresa-badge {
  background: #f1f5f9;
  color: #475569;
  padding: 4px 8px;
  border-radius: 6px;
  font-size: 0.8rem;
  font-weight: 500;
}

/* Empty State */
.empty-state {
  padding: 60px 20px;
  text-align: center;
  color: #94a3b8;
}

.empty-icon {
  margin-bottom: 16px;
  color: #cbd5e1;
}

.empty-state h3 {
  color: #475569;
  font-weight: 600;
  font-size: 1.15rem;
  margin: 0 0 6px 0;
}

.empty-state p {
  font-size: 0.9rem;
  margin: 0;
  color: #94a3b8;
}
</style>
