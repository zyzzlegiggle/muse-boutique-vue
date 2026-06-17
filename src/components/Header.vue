<script setup>
import { computed } from 'vue'
import { RouterLink } from 'vue-router'
import { useDetailsStore } from '../stores/detailsStore'
import { ShoppingBag } from '@lucide/vue'

const detailsStore = useDetailsStore()
const cartItemsCount = computed(() => detailsStore.cartItemsCount)

const openCart = () => {
  detailsStore.toggleCart(true)
}
</script>

<template>
  <header class="main-header">
    <div class="container header-container">
      <nav class="nav-links">
        <RouterLink to="/" class="nav-link" active-class="active">Home</RouterLink>
        <RouterLink to="/shop" class="nav-link" active-class="active">Shop</RouterLink>
      </nav>

      <RouterLink to="/" class="logo">
        MUSE
      </RouterLink>

      <div class="header-actions">
        <button class="cart-trigger" @click="openCart" aria-label="Open shopping bag">
          <ShoppingBag :size="18" stroke-width="1.5" />
          <span v-if="cartItemsCount > 0" class="cart-count">{{ cartItemsCount }}</span>
        </button>
      </div>
    </div>
  </header>
</template>

<style scoped>
.main-header {
  position: sticky;
  top: 0;
  left: 0;
  width: 100%;
  background-color: rgba(253, 253, 251, 0.85);
  backdrop-filter: blur(12px);
  border-bottom: 1px solid var(--color-border);
  z-index: 900;
  transition: var(--transition-smooth);
}

.header-container {
  height: 80px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.nav-links {
  display: flex;
  gap: 32px;
  flex: 1;
}

.nav-link {
  font-size: 0.8125rem;
  text-transform: uppercase;
  letter-spacing: 0.12em;
  font-weight: 500;
  color: var(--color-text-secondary);
  position: relative;
  padding: 8px 0;
}

.nav-link::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 1px;
  background-color: var(--color-text-primary);
  transform: scaleX(0);
  transform-origin: right;
  transition: var(--transition-smooth);
}

.nav-link:hover {
  color: var(--color-text-primary);
}

.nav-link:hover::after,
.nav-link.active::after {
  transform: scaleX(1);
  transform-origin: left;
}

.nav-link.active {
  color: var(--color-text-primary);
}

.logo {
  font-family: var(--font-serif);
  font-size: 1.75rem;
  font-weight: 400;
  letter-spacing: 0.25em;
  color: var(--color-text-primary);
  text-align: center;
  flex: 1;
  display: flex;
  justify-content: center;
  margin-right: -40px; /* offset to center perfectly since nav links has flex 1 */
}

@media (max-width: 768px) {
  .logo {
    margin-right: 0;
    justify-content: flex-end;
    flex: none;
  }
  .nav-links {
    gap: 16px;
  }
}

.header-actions {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  flex: 1;
}

.cart-trigger {
  background: none;
  border: none;
  cursor: pointer;
  padding: 8px;
  position: relative;
  color: var(--color-text-primary);
  transition: var(--transition-fast);
  display: flex;
  align-items: center;
  justify-content: center;
}

.cart-trigger:hover {
  color: var(--color-accent);
}

.cart-count {
  position: absolute;
  top: 0;
  right: 0;
  background-color: var(--color-text-primary);
  color: var(--color-bg);
  font-size: 0.625rem;
  font-weight: 600;
  min-width: 16px;
  height: 16px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0 4px;
}
</style>
