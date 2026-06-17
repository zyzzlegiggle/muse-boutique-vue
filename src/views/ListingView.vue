<script setup>
import { onMounted, watch } from 'vue'
import { useRoute } from 'vue-router'
import { useListingStore } from '../stores/listingStore'
import ProductCard from '../components/ProductCard.vue'

const listingStore = useListingStore()
const route = useRoute()

// Check for category route query parameters and apply them
const checkQueryParams = () => {
  if (route.query.category) {
    listingStore.setSelectedCategory(route.query.category)
  } else {
    listingStore.setSelectedCategory('All')
  }
}

onMounted(() => {
  checkQueryParams()
})

// Keep category filters synced if route query changes
watch(() => route.query.category, () => {
  checkQueryParams()
})
</script>

<template>
  <div class="shop-page container animate-fade-in">
    <!-- Header Summary -->
    <header class="shop-header">
      <h1 class="shop-title">The Collection</h1>
      <p class="shop-subtitle">
        Carefully designed wardrobe staples, stone objects, and natural apothecary essentials.
      </p>
    </header>

    <div class="shop-layout">
      <!-- Filter Sidebar -->
      <aside class="shop-sidebar">
        <!-- Search -->
        <div class="filter-group">
          <label for="search-input" class="input-label">Search products</label>
          <input 
            id="search-input"
            type="text" 
            placeholder="Search keywords..." 
            class="input-field"
            :value="listingStore.searchQuery"
            @input="listingStore.setSearchQuery($event.target.value)"
          />
        </div>

        <!-- Categories -->
        <div class="filter-group">
          <span class="input-label">Category</span>
          <div class="category-filters">
            <button 
              v-for="cat in listingStore.categories" 
              :key="cat"
              class="cat-filter-btn"
              :class="{ 'active': listingStore.selectedCategory === cat }"
              @click="listingStore.setSelectedCategory(cat)"
            >
              {{ cat }}
            </button>
          </div>
        </div>

        <!-- Price Range -->
        <div class="filter-group">
          <div class="price-header">
            <label for="price-range" class="input-label">Maximum Price</label>
            <span class="price-value">${{ listingStore.maxPrice }}</span>
          </div>
          <input 
            id="price-range"
            type="range" 
            min="20" 
            max="400" 
            step="5"
            class="price-slider"
            :value="listingStore.maxPrice"
            @input="listingStore.setMaxPrice(parseInt($event.target.value))"
          />
          <div class="price-bounds">
            <span>$20</span>
            <span>$400</span>
          </div>
        </div>

        <!-- Sorting -->
        <div class="filter-group">
          <label for="sort-select" class="input-label">Sort Products</label>
          <select 
            id="sort-select"
            class="input-field select-field"
            :value="listingStore.sortBy"
            @change="listingStore.setSortBy($event.target.value)"
          >
            <option value="default">Default Catalogue</option>
            <option value="price-asc">Price: Low to High</option>
            <option value="price-desc">Price: High to Low</option>
          </select>
        </div>
      </aside>

      <!-- Products Grid -->
      <main class="shop-main">
        <div class="results-meta">
          <span class="results-count">
            Showing {{ listingStore.filteredProducts.length }} results
          </span>
        </div>

        <!-- Empty Results -->
        <div v-if="listingStore.filteredProducts.length === 0" class="empty-results">
          <p class="empty-results-msg">No products found matching your current filters.</p>
          <button 
            class="btn btn-secondary btn-sm"
            @click="listingStore.searchQuery = ''; listingStore.selectedCategory = 'All'; listingStore.maxPrice = 400"
          >
            Clear Filters
          </button>
        </div>

        <!-- Grid -->
        <div v-else class="grid-3">
          <ProductCard 
            v-for="product in listingStore.filteredProducts" 
            :key="product.id" 
            :product="product"
          />
        </div>
      </main>
    </div>
  </div>
</template>

<style scoped>
.shop-page {
  padding-top: 40px;
}

.shop-header {
  margin-bottom: 60px;
  border-bottom: 1px solid var(--color-border);
  padding-bottom: 30px;
}

.shop-title {
  font-size: 3rem;
  margin-bottom: 8px;
}

.shop-subtitle {
  font-size: 1rem;
  color: var(--color-text-secondary);
  max-width: 500px;
}

/* Layout Grid */
.shop-layout {
  display: grid;
  grid-template-columns: 260px 1fr;
  gap: 48px;
  align-items: start;
}

@media (max-width: 900px) {
  .shop-layout {
    grid-template-columns: 1fr;
    gap: 32px;
  }
}

/* Sidebar styling */
.shop-sidebar {
  display: flex;
  flex-direction: column;
  gap: 36px;
  position: sticky;
  top: 120px;
}

.filter-group {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.category-filters {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.cat-filter-btn {
  text-align: left;
  background: none;
  border: none;
  font-size: 0.875rem;
  color: var(--color-text-secondary);
  cursor: pointer;
  padding: 6px 0;
  transition: var(--transition-fast);
  border-left: 2px solid transparent;
  padding-left: 10px;
}

.cat-filter-btn:hover {
  color: var(--color-text-primary);
  border-left-color: var(--color-border);
}

.cat-filter-btn.active {
  color: var(--color-text-primary);
  font-weight: 500;
  border-left-color: var(--color-accent);
}

/* Price range styling */
.price-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.price-value {
  font-size: 0.875rem;
  font-weight: 500;
}

.price-slider {
  -webkit-appearance: none;
  appearance: none;
  width: 100%;
  height: 2px;
  background: var(--color-border);
  outline: none;
}

.price-slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 16px;
  height: 16px;
  border-radius: 50%;
  background: var(--color-text-primary);
  cursor: pointer;
  transition: var(--transition-fast);
}

.price-slider::-webkit-slider-thumb:hover {
  background: var(--color-accent);
}

.price-bounds {
  display: flex;
  justify-content: space-between;
  font-size: 0.75rem;
  color: var(--color-text-muted);
}

.select-field {
  cursor: pointer;
}

/* Main Shop Area */
.results-meta {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 24px;
}

.results-count {
  font-size: 0.8125rem;
  color: var(--color-text-secondary);
}

.empty-results {
  text-align: center;
  padding: 80px 24px;
  border: 1px dashed var(--color-border);
  background-color: var(--color-surface);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 16px;
}

.empty-results-msg {
  font-family: var(--font-serif);
  font-size: 1.25rem;
  color: var(--color-text-secondary);
  font-style: italic;
}

.btn-sm {
  padding: 8px 16px;
  font-size: 0.75rem;
}
</style>
