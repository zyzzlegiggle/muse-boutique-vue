<script setup>
import { computed } from 'vue'
import { RouterLink } from 'vue-router'
import { useListingStore } from '../stores/listingStore'
import ProductCard from '../components/ProductCard.vue'
import { ArrowRight } from '@lucide/vue'

const listingStore = useListingStore()

// Select 3 featured products to show on homepage
const featuredProducts = computed(() => {
  return [listingStore.products[0], listingStore.products[3], listingStore.products[6]]
})
</script>

<template>
  <div class="home-page animate-fade-in">
    <!-- Hero Editorial Section -->
    <section class="hero-section">
      <div class="hero-image-container">
        <!-- We use the generated image copy from assets -->
        <img src="../assets/hero_banner.png" alt="Muse Lifestyle Banner" class="hero-image" />
        <div class="hero-overlay"></div>
      </div>
      
      <div class="container hero-content-container">
        <div class="hero-text-card">
          <span class="hero-subtitle">NEW ARRIVALS</span>
          <h1 class="hero-title">A Study in<br />Minimal Form</h1>
          <p class="hero-description">
            Thoughtfully crafted apparel, objects, and organic scents that bring calm and intentional utility to daily rituals.
          </p>
          <RouterLink to="/shop" class="btn btn-primary hero-btn">
            <span>Explore Collection</span>
            <ArrowRight :size="16" class="arrow" stroke-width="1.5" />
          </RouterLink>
        </div>
      </div>
    </section>

    <!-- Editorial Philosophy Statement -->
    <section class="philosophy-section">
      <div class="container philosophy-container">
        <span class="section-label">Our Philosophy</span>
        <h2 class="philosophy-quote">
          "Simplicity is not the absence of clutter, but the presence of clarity."
        </h2>
        <p class="philosophy-text">
          Muse Boutique was born out of a desire for quiet elegance. We collaborate with artisans across the globe to bring you raw stone, hand-blown glassware, clean textiles, and organic botanical extractions that celebrate natural materials. We believe in owning fewer things of higher quality.
        </p>
      </div>
    </section>

    <!-- Category Highlights Grid -->
    <section class="categories-section">
      <div class="container">
        <span class="section-label">Curated Spaces</span>
        <div class="categories-grid">
          <!-- Apparel -->
          <RouterLink to="/shop?category=Apparel" class="category-banner bg-apparel">
            <div class="category-meta">
              <h3 class="category-title">Apparel</h3>
              <span class="category-action">Browse Garments</span>
            </div>
          </RouterLink>

          <!-- Objects -->
          <RouterLink to="/shop?category=Objects" class="category-banner bg-objects">
            <div class="category-meta">
              <h3 class="category-title">Objects</h3>
              <span class="category-action">Browse Homeware</span>
            </div>
          </RouterLink>

          <!-- Scents -->
          <RouterLink to="/shop?category=Scents" class="category-banner bg-scents">
            <div class="category-meta">
              <h3 class="category-title">Scents</h3>
              <span class="category-action">Browse Apothecary</span>
            </div>
          </RouterLink>
        </div>
      </div>
    </section>

    <!-- Featured Collection -->
    <section class="featured-section">
      <div class="container">
        <div class="section-header">
          <div>
            <span class="section-label">The Selection</span>
            <h2 class="section-title">Selected Pieces</h2>
          </div>
          <RouterLink to="/shop" class="btn-link">View All Products</RouterLink>
        </div>
        
        <div class="grid-3">
          <ProductCard 
            v-for="product in featuredProducts" 
            :key="product.id" 
            :product="product" 
          />
        </div>
      </div>
    </section>
  </div>
</template>

<style scoped>
/* Hero Section */
.hero-section {
  position: relative;
  height: calc(100vh - 80px);
  min-height: 550px;
  display: flex;
  align-items: center;
  overflow: hidden;
}

.hero-image-container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
}

.hero-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  filter: contrast(1.02) brightness(0.98);
}

.hero-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, rgba(253, 253, 251, 0.7) 0%, rgba(253, 253, 251, 0.2) 60%, rgba(0,0,0,0) 100%);
}

.hero-content-container {
  position: relative;
  z-index: 2;
}

.hero-text-card {
  max-width: 480px;
  background-color: rgba(253, 253, 251, 0.9);
  backdrop-filter: blur(16px);
  padding: 48px;
  border: 1px solid var(--color-border);
}

@media (max-width: 768px) {
  .hero-section {
    height: auto;
    padding: 60px 0;
  }
  .hero-image-container {
    opacity: 0.2;
  }
  .hero-overlay {
    background: none;
  }
  .hero-text-card {
    background: none;
    backdrop-filter: none;
    border: none;
    padding: 0;
    max-width: 100%;
  }
}

.hero-subtitle {
  font-size: 0.75rem;
  letter-spacing: 0.15em;
  font-weight: 600;
  color: var(--color-accent);
  display: block;
  margin-bottom: 16px;
}

.hero-title {
  font-size: 3.5rem;
  line-height: 1.1;
  color: var(--color-text-primary);
  margin-bottom: 20px;
}

@media (max-width: 480px) {
  .hero-title {
    font-size: 2.5rem;
  }
}

.hero-description {
  font-size: 0.9375rem;
  color: var(--color-text-secondary);
  line-height: 1.7;
  margin-bottom: 32px;
}

.hero-btn {
  display: inline-flex;
  gap: 12px;
}

.hero-btn .arrow {
  transition: transform var(--transition-fast);
}

.hero-btn:hover .arrow {
  transform: translateX(4px);
}

/* Philosophy Section */
.philosophy-section {
  padding: 100px 0;
  background-color: var(--color-accent-light);
  border-bottom: 1px solid var(--color-border);
}

.philosophy-container {
  max-width: 760px;
  text-align: center;
  margin: 0 auto;
}

.section-label {
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.15em;
  font-weight: 600;
  color: var(--color-accent);
  display: block;
  margin-bottom: 24px;
}

.philosophy-quote {
  font-size: 2.25rem;
  line-height: 1.3;
  margin-bottom: 24px;
  color: var(--color-text-primary);
  font-style: italic;
}

@media (max-width: 600px) {
  .philosophy-quote {
    font-size: 1.75rem;
  }
}

.philosophy-text {
  font-size: 0.9375rem;
  color: var(--color-text-secondary);
  line-height: 1.8;
}

/* Curated Spaces Banners */
.categories-section {
  padding: 80px 0;
}

.categories-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
}

@media (max-width: 768px) {
  .categories-grid {
    grid-template-columns: 1fr;
  }
}

.category-banner {
  height: 380px;
  display: flex;
  align-items: flex-end;
  padding: 30px;
  position: relative;
  overflow: hidden;
  border: 1px solid var(--color-border);
  transition: var(--transition-smooth);
}

.category-banner::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(0deg, rgba(28, 26, 23, 0.4) 0%, rgba(0,0,0,0) 60%);
  z-index: 1;
}

.category-banner.bg-apparel {
  background: #eae6df;
}

.category-banner.bg-objects {
  background: #f1ebd9;
}

.category-banner.bg-scents {
  background: #e6e2da;
}

/* Elegant visual abstractions inside category banners */
.category-banner::after {
  content: '';
  position: absolute;
  width: 120px;
  height: 120px;
  border-radius: 50%;
  border: 1px stroke rgba(28, 26, 23, 0.05);
  top: 30%;
  left: 50%;
  transform: translate(-50%, -50%);
  border: 1px solid rgba(28, 26, 23, 0.06);
  pointer-events: none;
  transition: var(--transition-smooth);
}

.category-banner:hover::after {
  width: 150px;
  height: 150px;
  border-color: rgba(28, 26, 23, 0.12);
}

.category-meta {
  position: relative;
  z-index: 2;
  color: var(--color-bg);
}

.category-title {
  font-size: 2rem;
  color: var(--color-bg);
  margin-bottom: 6px;
  letter-spacing: -0.01em;
}

.category-action {
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  font-weight: 500;
  opacity: 0.8;
  display: inline-flex;
  align-items: center;
  gap: 6px;
  transition: var(--transition-fast);
}

.category-banner:hover .category-action {
  opacity: 1;
  transform: translateX(4px);
}

/* Featured Section */
.featured-section {
  padding: 80px 0 120px 0;
  border-top: 1px solid var(--color-border);
}

.section-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  margin-bottom: 48px;
}

.section-title {
  font-size: 2.25rem;
  color: var(--color-text-primary);
  margin-top: 4px;
}
</style>
