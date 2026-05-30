<template>
  <div class="vfg-container">
    <form @submit.prevent="handleSubmit" class="vfg-form">
      <div class="vfg-grid">
        <div 
          v-for="field in schema.fields" 
          :key="field.model" 
          class="vfg-field-wrapper"
          :class="{ 
            'vfg-featured': field.featured, 
            'vfg-disabled': field.enable === false || field.disabled 
          }"
        >
          <label :for="field.model" class="vfg-label">
            {{ field.label }}
            <span v-if="field.required" class="vfg-required-indicator">*</span>
          </label>
          
          <div class="vfg-input-container">
            <input 
              v-if="field.type === 'input'"
              :id="field.model"
              :type="field.inputType || 'text'"
              :placeholder="field.placeholder"
              :required="field.required"
              :disabled="field.enable === false || field.disabled"
              v-model="internalModel[field.model]"
              @input="handleInput(field.model)"
              class="vfg-input"
            />
            
            <textarea 
              v-else-if="field.type === 'textArea'"
              :id="field.model"
              :placeholder="field.placeholder"
              :required="field.required"
              v-model="internalModel[field.model]"
              @input="handleInput(field.model)"
              class="vfg-textarea"
            ></textarea>
          </div>
        </div>
      </div>
      
      <div class="vfg-actions">
        <button type="submit" class="vfg-submit-btn">
          <span>Guardar Producto</span>
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="18" height="18" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <polyline points="20 6 9 17 4 12"></polyline>
          </svg>
        </button>
      </div>
    </form>
  </div>
</template>

<script setup>
import { reactive, watch, defineProps, defineEmits } from 'vue'

const props = defineProps({
  schema: {
    type: Object,
    required: true,
    default: () => ({ fields: [] })
  },
  model: {
    type: Object,
    default: () => ({})
  }
})

const emit = defineEmits(['submit', 'update:model', 'change'])

// Reactively copy/bind model values
const internalModel = reactive({ ...props.model })

// Initialize missing values in the model based on schema fields
props.schema.fields.forEach(field => {
  if (internalModel[field.model] === undefined) {
    internalModel[field.model] = ''
  }
})

// Auto-calculation logic for pricing (a beautiful value-added helper!)
const calculatePrecioFinal = () => {
  const regular = parseFloat(internalModel.precioregular) || 0
  const desc = parseFloat(internalModel.descuento) || 0
  
  if (regular > 0) {
    const finalVal = regular - (regular * (desc / 100))
    internalModel.preciofinal = finalVal.toFixed(2)
  } else {
    internalModel.preciofinal = '0.00'
  }
  emit('update:model', { ...internalModel })
}

// Watch inputs for the calculation
watch(
  () => [internalModel.precioregular, internalModel.descuento],
  () => {
    calculatePrecioFinal()
  }
)

// Sync internal model changes back to parent
const handleInput = (fieldModel) => {
  emit('update:model', { ...internalModel })
  emit('change', { field: fieldModel, value: internalModel[fieldModel], model: { ...internalModel } })
}

// Watch parent model updates
watch(
  () => props.model,
  (newVal) => {
    Object.assign(internalModel, newVal)
  },
  { deep: true }
)

const handleSubmit = () => {
  emit('submit', { ...internalModel })
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;500;600;700&display=swap');

.vfg-container {
  font-family: 'Outfit', sans-serif;
  background: rgba(255, 255, 255, 0.7);
  backdrop-filter: blur(12px);
  border-radius: 20px;
  padding: 30px;
  border: 1px solid rgba(255, 255, 255, 0.4);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.04);
  max-width: 700px;
  margin: 0 auto;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.vfg-container:hover {
  box-shadow: 0 15px 40px rgba(66, 185, 131, 0.1);
  border-color: rgba(66, 185, 131, 0.2);
}

.vfg-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 24px;
}

@media (max-width: 600px) {
  .vfg-grid {
    grid-template-columns: 1fr;
  }
}

.vfg-field-wrapper {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  transition: all 0.2s ease;
}

.vfg-featured {
  grid-column: span 1;
}

.vfg-disabled {
  opacity: 0.8;
}

.vfg-label {
  font-size: 0.9rem;
  font-weight: 500;
  color: #374151;
  margin-bottom: 8px;
  display: flex;
  align-items: center;
  gap: 4px;
}

.vfg-required-indicator {
  color: #ef4444;
  font-weight: bold;
}

.vfg-input-container {
  position: relative;
  width: 100%;
}

.vfg-input {
  width: 100%;
  padding: 12px 16px;
  font-size: 0.95rem;
  font-family: inherit;
  border-radius: 12px;
  border: 1.5px solid #e5e7eb;
  background: #f9fafb;
  color: #1f2937;
  outline: none;
  box-sizing: border-box;
  transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
}

.vfg-input:focus {
  border-color: #42b983;
  background: #ffffff;
  box-shadow: 0 0 0 4px rgba(66, 185, 131, 0.15);
}

.vfg-input:disabled {
  background: #f3f4f6;
  border-color: #e5e7eb;
  color: #9ca3af;
  cursor: not-allowed;
}

.vfg-textarea {
  width: 100%;
  min-height: 100px;
  padding: 12px 16px;
  font-size: 0.95rem;
  font-family: inherit;
  border-radius: 12px;
  border: 1.5px solid #e5e7eb;
  background: #f9fafb;
  color: #1f2937;
  outline: none;
  resize: vertical;
  box-sizing: border-box;
  transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
}

.vfg-textarea:focus {
  border-color: #42b983;
  background: #ffffff;
  box-shadow: 0 0 0 4px rgba(66, 185, 131, 0.15);
}

.vfg-actions {
  margin-top: 35px;
  display: flex;
  justify-content: flex-end;
}

.vfg-submit-btn {
  background: linear-gradient(135deg, #42b983 0%, #3aa372 100%);
  color: white;
  border: none;
  padding: 14px 28px;
  font-size: 1rem;
  font-weight: 600;
  border-radius: 14px;
  cursor: pointer;
  box-shadow: 0 4px 15px rgba(66, 185, 131, 0.25);
  display: flex;
  align-items: center;
  gap: 10px;
  transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
}

.vfg-submit-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(66, 185, 131, 0.35);
  background: linear-gradient(135deg, #4ed396 0%, #3aa372 100%);
}

.vfg-submit-btn:active {
  transform: translateY(1px);
}
</style>
