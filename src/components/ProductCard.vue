<script setup>
import { RouterLink } from 'vue-router'

defineProps({
  product: {
    type: Object,
    required: true
  }
})
</script>

<template>
  <RouterLink :to="`/shop/${product.id}`" class="product-card">
    <div class="product-img-wrapper">
      <div class="product-svg-container" v-html="product.svg.path" :viewBox="product.svg.viewBox">
      </div>
      <div class="product-overlay">
        <span class="view-details-text">View Details</span>
      </div>
    </div>
    
    <div class="product-info">
      <span class="product-category">{{ product.category }}</span>
      <h3 class="product-title">{{ product.title }}</h3>
      <span class="product-price">${{ product.price }}</span>
    </div>
  </RouterLink>
</template>

<style scoped>
.product-card {
  display: flex;
  flex-direction: column;
  background-color: var(--color-surface);
  border: 1px solid var(--color-border);
  position: relative;
  overflow: hidden;
  transition: var(--transition-smooth);
}

.product-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.02);
  border-color: var(--color-text-secondary);
}

.product-img-wrapper {
  aspect-ratio: 4/5;
  width: 100%;
  background-color: var(--color-accent-light);
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  overflow: hidden;
  border-bottom: 1px solid var(--color-border);
  color: var(--color-accent);
  padding: 32px;
}

.product-svg-container {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: transform 0.6s cubic-bezier(0.25, 1, 0.5, 1);
}

.product-svg-container :deep(svg) {
  width: 100%;
  height: 100%;
  max-width: 150px;
  max-height: 150px;
}

.product-card:hover .product-svg-container {
  transform: scale(1.08);
}

.product-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(253, 253, 251, 0.4);
  backdrop-filter: blur(2px);
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: opacity 0.4s ease;
}

.product-card:hover .product-overlay {
  opacity: 1;
}

.view-details-text {
  font-size: 0.75rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  font-weight: 600;
  color: var(--color-text-primary);
  border: 1px solid var(--color-text-primary);
  background-color: var(--color-bg);
  padding: 8px 16px;
  transform: translateY(10px);
  transition: transform 0.4s cubic-bezier(0.25, 1, 0.5, 1);
}

.product-card:hover .view-details-text {
  transform: translateY(0);
}

.product-info {
  padding: 20px;
  display: flex;
  flex-direction: column;
}

.product-category {
  font-size: 0.6875rem;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: var(--color-text-muted);
  margin-bottom: 6px;
}

.product-title {
  font-family: var(--font-sans);
  font-size: 0.9375rem;
  font-weight: 500;
  color: var(--color-text-primary);
  margin-bottom: 6px;
  line-height: 1.4;
}

.product-price {
  font-size: 0.9375rem;
  font-weight: 500;
  color: var(--color-text-primary);
  margin-top: auto;
}
</style>
