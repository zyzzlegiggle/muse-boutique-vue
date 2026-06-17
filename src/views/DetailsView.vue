<script setup>
import { onMounted, watch, computed } from 'vue'
import { useRoute, RouterLink } from 'vue-router'
import { useDetailsStore } from '../stores/detailsStore'
import { ArrowLeft, ShoppingBag } from '@lucide/vue'

const route = useRoute()
const detailsStore = useDetailsStore()

const product = computed(() => detailsStore.activeProduct)
const selectedVariant = computed(() => detailsStore.selectedVariant)

const loadActiveProduct = () => {
  detailsStore.loadProduct(route.params.id)
}

onMounted(() => {
  loadActiveProduct()
})

watch(() => route.params.id, () => {
  loadActiveProduct()
})

const selectVariant = (variant) => {
  detailsStore.setSelectedVariant(variant)
}

const addToCart = () => {
  if (product.value) {
    detailsStore.addToCart(product.value, selectedVariant.value)
  }
}
</script>

<template>
  <div class="details-page container animate-fade-in">
    <!-- Back to shop link -->
    <div class="back-nav">
      <RouterLink to="/shop" class="back-link">
        <ArrowLeft :size="16" stroke-width="1.5" />
        <span>Back to collection</span>
      </RouterLink>
    </div>

    <!-- Product Details Content -->
    <div v-if="product" class="details-layout">
      <!-- Left side: Image/Drawing Display -->
      <section class="gallery-section">
        <div class="product-canvas">
          <div class="svg-container" v-html="product.svg.path" :viewBox="product.svg.viewBox"></div>
        </div>
      </section>

      <!-- Right side: Product purchasing & details specs -->
      <section class="info-section">
        <span class="info-category">{{ product.category }}</span>
        
        <h1 class="info-title">{{ product.title }}</h1>
        <p class="info-subtitle">{{ product.subtitle }}</p>
        <span class="info-price">${{ product.price }}</span>
        
        <p class="info-description">{{ product.description }}</p>

        <!-- Variant Selector -->
        <div class="selector-group" v-if="product.variants && product.variants.length > 0">
          <span class="selector-label">Select {{ product.variantLabel }}</span>
          <div class="variant-buttons">
            <button 
              v-for="v in product.variants" 
              :key="v"
              class="variant-btn"
              :class="{ 'active': selectedVariant === v }"
              @click="selectVariant(v)"
            >
              {{ v }}
            </button>
          </div>
        </div>

        <!-- Purchase Button -->
        <button class="btn btn-primary buy-btn" @click="addToCart">
          <ShoppingBag :size="16" stroke-width="1.5" />
          <span>Add to Bag</span>
        </button>

        <!-- Product Accordions -->
        <div class="accordions-container">
          <details class="accordion-item" open>
            <summary class="accordion-header">The Narrative</summary>
            <div class="accordion-content">
              <p>{{ product.details }}</p>
            </div>
          </details>

          <details class="accordion-item">
            <summary class="accordion-header">Specifications</summary>
            <div class="accordion-content">
              <ul class="specs-list">
                <li v-for="spec in product.specs" :key="spec">{{ spec }}</li>
              </ul>
            </div>
          </details>

          <details class="accordion-item">
            <summary class="accordion-header">Shipping & Returns</summary>
            <div class="accordion-content">
              <p>
                We ship globally using carbon-neutral carrier services. Standard deliveries arrive in 3-5 business days. We offer free shipping on orders over $250. Unworn apparel and unboxed home objects can be returned within 14 days of delivery.
              </p>
            </div>
          </details>
        </div>
      </section>
    </div>

    <!-- Product Not Found State -->
    <div v-else class="not-found-state">
      <h2 class="not-found-title">Product Not Found</h2>
      <p class="not-found-msg">We couldn't load the product you are looking for.</p>
      <RouterLink to="/shop" class="btn btn-primary">
        Return to Collection
      </RouterLink>
    </div>
  </div>
</template>

<style scoped>
.details-page {
  padding-top: 30px;
}

.back-nav {
  margin-bottom: 40px;
}

.back-link {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  font-size: 0.8125rem;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: var(--color-text-secondary);
  font-weight: 500;
}

.back-link:hover {
  color: var(--color-text-primary);
  transform: translateX(-4px);
}

/* Split layout grid */
.details-layout {
  display: grid;
  grid-template-columns: 1.1fr 0.9fr;
  gap: 80px;
  align-items: start;
}

@media (max-width: 900px) {
  .details-layout {
    grid-template-columns: 1fr;
    gap: 48px;
  }
}

/* Canvas Gallery */
.product-canvas {
  aspect-ratio: 4/5;
  width: 100%;
  background-color: var(--color-accent-light);
  border: 1px solid var(--color-border);
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--color-accent);
  padding: 60px;
}

.svg-container {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.svg-container :deep(svg) {
  width: 100%;
  height: 100%;
  max-width: 280px;
  max-height: 280px;
}

/* Info purchasing area */
.info-section {
  display: flex;
  flex-direction: column;
}

.info-category {
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.12em;
  color: var(--color-text-muted);
  margin-bottom: 8px;
  font-weight: 600;
}

.info-title {
  font-size: 2.75rem;
  line-height: 1.1;
  color: var(--color-text-primary);
  margin-bottom: 4px;
}

.info-subtitle {
  font-size: 1rem;
  color: var(--color-text-secondary);
  font-style: italic;
  margin-bottom: 16px;
  font-family: var(--font-serif);
}

.info-price {
  font-size: 1.5rem;
  font-weight: 500;
  color: var(--color-text-primary);
  margin-bottom: 24px;
  display: block;
}

.info-description {
  font-size: 0.9375rem;
  color: var(--color-text-secondary);
  line-height: 1.7;
  margin-bottom: 32px;
}

/* Variant button selector */
.selector-group {
  margin-bottom: 32px;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.selector-label {
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: var(--color-text-secondary);
  font-weight: 600;
}

.variant-buttons {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

.variant-btn {
  background: none;
  border: 1px solid var(--color-border);
  padding: 10px 20px;
  font-size: 0.8125rem;
  cursor: pointer;
  transition: var(--transition-fast);
  min-width: 60px;
}

.variant-btn:hover {
  border-color: var(--color-text-primary);
}

.variant-btn.active {
  border-color: var(--color-text-primary);
  background-color: var(--color-text-primary);
  color: var(--color-bg);
}

/* Buy button */
.buy-btn {
  width: 100%;
  margin-bottom: 40px;
  display: flex;
  gap: 10px;
}

/* Accordions */
.accordions-container {
  border-top: 1px solid var(--color-border);
}

.accordion-item {
  border-bottom: 1px solid var(--color-border);
}

.accordion-header {
  padding: 20px 0;
  font-size: 0.875rem;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  font-weight: 600;
  cursor: pointer;
  color: var(--color-text-primary);
  user-select: none;
  outline: none;
  transition: var(--transition-fast);
}

.accordion-header:hover {
  color: var(--color-accent);
}

.accordion-content {
  padding: 0 0 24px 0;
  font-size: 0.875rem;
  color: var(--color-text-secondary);
  line-height: 1.7;
}

.specs-list {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.specs-list li {
  position: relative;
  padding-left: 14px;
}

.specs-list li::before {
  content: '—';
  position: absolute;
  left: 0;
  color: var(--color-text-muted);
}

/* Product not found */
.not-found-state {
  text-align: center;
  padding: 120px 24px;
  max-width: 500px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
}

.not-found-title {
  font-size: 2rem;
}

.not-found-msg {
  color: var(--color-text-secondary);
  margin-bottom: 10px;
}
</style>
