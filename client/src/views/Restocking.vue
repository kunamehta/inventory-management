<template>
  <div class="restocking">
    <div class="page-header">
      <h2>Restocking Planner</h2>
      <p>Set your budget and review recommended restocking items based on demand forecasts.</p>
    </div>

    <div v-if="loading" class="loading">Loading...</div>
    <div v-else-if="error" class="error">{{ error }}</div>
    <div v-else>
      <div class="card budget-card">
        <div class="budget-row">
          <label class="budget-label">Available Budget</label>
          <span class="budget-amount">{{ currencySymbol }}{{ budget.toLocaleString() }}</span>
        </div>
        <input
          type="range"
          v-model.number="budget"
          min="0"
          max="500000"
          step="1000"
          class="budget-slider"
        />
        <div class="budget-summary">
          <span>{{ recommendedItems.length }} items recommended</span>
          <span class="separator">·</span>
          <span>Total: {{ currencySymbol }}{{ totalCost.toLocaleString() }}</span>
          <span class="separator">·</span>
          <span>Remaining: {{ currencySymbol }}{{ remaining.toLocaleString() }}</span>
        </div>
      </div>

      <div class="card">
        <div class="card-header">
          <h3 class="card-title">Recommended Restocking Items</h3>
        </div>

        <div v-if="submitted && submittedOrder" class="success-banner">
          Order {{ submittedOrder.order_number }} submitted. Expected delivery: {{ formatDate(submittedOrder.expected_delivery) }}.
        </div>

        <div v-if="recommendedItems.length === 0" class="empty-state">
          No items fit within the current budget.
        </div>
        <div v-else class="table-container">
          <table class="restock-table">
            <thead>
              <tr>
                <th>SKU</th>
                <th>Item Name</th>
                <th class="col-num">Forecasted Qty</th>
                <th class="col-num">Unit Cost</th>
                <th class="col-num">Line Total</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in recommendedItems" :key="item.sku">
                <td><code class="sku">{{ item.sku }}</code></td>
                <td>{{ item.name }}</td>
                <td class="col-num">{{ item.quantity.toLocaleString() }}</td>
                <td class="col-num">{{ currencySymbol }}{{ item.unit_cost.toLocaleString() }}</td>
                <td class="col-num"><strong>{{ currencySymbol }}{{ item.line_total.toLocaleString() }}</strong></td>
              </tr>
            </tbody>
          </table>
        </div>

        <div class="card-footer">
          <button
            v-if="!submitted"
            class="btn-primary"
            :disabled="recommendedItems.length === 0 || submitting"
            @click="placeOrder"
          >
            {{ submitting ? 'Placing Order...' : 'Place Order' }}
          </button>
          <button
            v-else
            class="btn-secondary"
            @click="submitted = false"
          >
            Place Another Order
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed, onMounted } from 'vue'
import { api } from '../api'
import { useI18n } from '../composables/useI18n'

export default {
  name: 'Restocking',
  setup() {
    const { t, currentCurrency, currentLocale } = useI18n()

    const currencySymbol = computed(() => currentCurrency.value === 'JPY' ? '¥' : '$')

    const forecasts = ref([])
    const inventory = ref([])
    const loading = ref(true)
    const error = ref(null)
    const budget = ref(50000)

    const submitting = ref(false)
    const submitted = ref(false)
    const submittedOrder = ref(null)

    const formatDate = (dateString) => {
      const date = new Date(dateString)
      if (isNaN(date.getTime())) return dateString
      const locale = currentLocale.value === 'ja' ? 'ja-JP' : 'en-US'
      return date.toLocaleDateString(locale, {
        year: 'numeric',
        month: 'short',
        day: 'numeric'
      })
    }

    const enrichedForecasts = computed(() => {
      return forecasts.value.map(forecast => {
        const inv = inventory.value.find(i => i.sku === forecast.item_sku)
        return {
          ...forecast,
          unit_cost: inv ? inv.unit_cost : 50
        }
      })
    })

    const recommendedItems = computed(() => {
      const sorted = [...enrichedForecasts.value].sort(
        (a, b) => b.forecasted_demand - a.forecasted_demand
      )
      const result = []
      let running = 0
      for (const f of sorted) {
        const line = f.forecasted_demand * f.unit_cost
        if (running + line <= budget.value) {
          result.push({
            sku: f.item_sku,
            name: f.item_name,
            quantity: f.forecasted_demand,
            unit_cost: f.unit_cost,
            line_total: line
          })
          running += line
        }
      }
      return result
    })

    const totalCost = computed(() =>
      recommendedItems.value.reduce((sum, i) => sum + i.line_total, 0)
    )

    const remaining = computed(() => budget.value - totalCost.value)

    const placeOrder = async () => {
      submitting.value = true
      try {
        const payload = {
          items: recommendedItems.value.map(i => ({
            sku: i.sku,
            name: i.name,
            quantity: i.quantity,
            unit_cost: i.unit_cost
          })),
          total_cost: totalCost.value
        }
        submittedOrder.value = await api.createRestockingOrder(payload)
        submitted.value = true
      } catch (err) {
        console.error('Failed to place order:', err)
      } finally {
        submitting.value = false
      }
    }

    onMounted(async () => {
      loading.value = true
      error.value = null
      try {
        const [forecastData, inventoryData] = await Promise.all([
          api.getDemandForecasts(),
          api.getInventory()
        ])
        forecasts.value = forecastData
        inventory.value = inventoryData
      } catch (err) {
        error.value = 'Failed to load data'
        console.error(err)
      } finally {
        loading.value = false
      }
    })

    return {
      currencySymbol,
      loading,
      error,
      budget,
      recommendedItems,
      totalCost,
      remaining,
      submitting,
      submitted,
      submittedOrder,
      placeOrder,
      formatDate
    }
  }
}
</script>

<style scoped>
.restocking {
  padding: 2rem;
}

.budget-card {
  margin-bottom: 1.5rem;
}

.budget-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 0.75rem;
}

.budget-label {
  font-size: 0.875rem;
  font-weight: 500;
  color: #64748b;
}

.budget-amount {
  font-size: 1.25rem;
  font-weight: 700;
  color: #0f172a;
}

.budget-slider {
  width: 100%;
  accent-color: #3b82f6;
  cursor: pointer;
}

.budget-summary {
  display: flex;
  gap: 0.5rem;
  margin-top: 0.75rem;
  font-size: 0.875rem;
  color: #64748b;
  flex-wrap: wrap;
}

.separator {
  color: #cbd5e1;
}

.success-banner {
  margin: 1rem 1.5rem;
  padding: 0.75rem 1rem;
  background: #f0fdf4;
  border: 1px solid #86efac;
  border-radius: 6px;
  color: #15803d;
  font-size: 0.875rem;
  font-weight: 500;
}

.empty-state {
  padding: 2rem 1.5rem;
  text-align: center;
  color: #64748b;
  font-size: 0.9rem;
}

.restock-table {
  width: 100%;
  table-layout: auto;
}

.col-num {
  text-align: right;
  white-space: nowrap;
}

.sku {
  font-family: monospace;
  font-size: 0.8rem;
  background: #f1f5f9;
  padding: 0.15rem 0.4rem;
  border-radius: 4px;
  color: #475569;
}

.card-footer {
  padding: 1rem 1.5rem;
  border-top: 1px solid #e2e8f0;
  display: flex;
  justify-content: flex-end;
}

.btn-primary {
  padding: 0.5rem 1.25rem;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 6px;
  font-size: 0.875rem;
  font-weight: 500;
  cursor: pointer;
  transition: background 0.15s;
}

.btn-primary:hover:not(:disabled) {
  background: #2563eb;
}

.btn-primary:disabled {
  background: #94a3b8;
  cursor: not-allowed;
}

.btn-secondary {
  padding: 0.5rem 1.25rem;
  background: white;
  color: #374151;
  border: 1px solid #e2e8f0;
  border-radius: 6px;
  font-size: 0.875rem;
  font-weight: 500;
  cursor: pointer;
  transition: background 0.15s;
}

.btn-secondary:hover {
  background: #f8fafc;
}
</style>
