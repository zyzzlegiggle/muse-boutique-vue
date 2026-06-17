<script setup>
import { computed } from 'vue'
import { useDetailsStore } from '../stores/detailsStore'
import { X, Plus, Minus, Trash2, ArrowRight } from '@lucide/vue'

const detailsStore = useDetailsStore()

const isOpen = computed(() => detailsStore.cartOpen)
const cartItems = computed(() => detailsStore.cart)
const subtotal = computed(() => detailsStore.cartSubtotal)
const checkoutSuccess = computed(() => detailsStore.checkoutSuccess)

const closeCart = () => {
  detailsStore.toggleCart(false)
}

const updateQty = (id, currentQty, amount) => {
  detailsStore.updateQuantity(id, currentQty + amount)
}

const removeItem = (id) => {
  detailsStore.removeFromCart(id)
}

const checkout = () => {
  detailsStore.checkout()
}

const resetStatus = () => {
  detailsStore.resetCheckoutStatus()
}
</script>

<template>
  <div>
    <!-- Backdrop -->
    <div 
      class="drawer-backdrop" 
      :class="{ 'active': isOpen }" 
      @click="closeCart"
    ></div>

    <!-- Drawer Sheet -->
    <div class="drawer" :class="{ 'active': isOpen }">
      <div class="drawer-header">
        <h2 class="drawer-title">Shopping Bag</h2>
        <button class="close-btn" @click="closeCart" aria-label="Close bag">
          <X :size="20" stroke-width="1.5" />
        </button>
      </div>

      <div class="drawer-content">
        <!-- Success State -->
        <div v-if="checkoutSuccess" class="success-state">
          <div class="success-icon">✓</div>
          <h3 class="success-title">Order Confirmed</h3>
          <p class="success-message">
            Thank you for shopping at Muse. Your order has been registered in our simulated storefront. Your curated selection is on its way.
          </p>
          <button class="btn btn-primary" @click="resetStatus">
            Continue Shopping
          </button>
        </div>

        <!-- Empty State -->
        <div v-else-if="cartItems.length === 0" class="empty-state">
          <p class="empty-message">Your shopping bag is currently empty.</p>
          <button class="btn btn-secondary" @click="closeCart">
            Discover Products
          </button>
        </div>

        <!-- Cart Items List -->
        <div v-else class="cart-list">
          <div v-for="item in cartItems" :key="item.id" class="cart-item">
            <div class="item-img-container" v-html="item.product.svg.path" :viewBox="item.product.svg.viewBox">
            </div>
            
            <div class="item-details">
              <div class="item-meta">
                <span class="item-category">{{ item.product.category }}</span>
                <button class="item-remove-btn" @click="removeItem(item.id)" aria-label="Remove item">
                  <Trash2 :size="14" stroke-width="1.5" />
                </button>
              </div>
              
              <h4 class="item-title">{{ item.product.title }}</h4>
              
              <div class="item-variant" v-if="item.variant">
                {{ item.product.variantLabel }}: {{ item.variant }}
              </div>
              
              <div class="item-controls">
                <div class="qty-selector">
                  <button 
                    class="qty-btn" 
                    @click="updateQty(item.id, item.quantity, -1)"
                    :disabled="item.quantity <= 1"
                  >
                    <Minus :size="12" stroke-width="1.5" />
                  </button>
                  <span class="qty-value">{{ item.quantity }}</span>
                  <button 
                    class="qty-btn" 
                    @click="updateQty(item.id, item.quantity, 1)"
                  >
                    <Plus :size="12" stroke-width="1.5" />
                  </button>
                </div>
                
                <span class="item-price">${{ item.product.price * item.quantity }}</span>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Drawer Footer -->
      <div v-if="cartItems.length > 0 && !checkoutSuccess" class="drawer-footer">
        <div class="summary-row">
          <span>Subtotal</span>
          <span class="summary-price">${{ subtotal }}</span>
        </div>
        <p class="shipping-info">Shipping and taxes calculated at checkout.</p>
        
        <button class="btn btn-primary checkout-btn" @click="checkout">
          <span>Checkout</span>
          <ArrowRight :size="16" stroke-width="1.5" class="arrow" />
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.drawer-header {
  padding: 24px;
  border-bottom: 1px solid var(--color-border);
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.drawer-title {
  font-size: 1.5rem;
  letter-spacing: -0.01em;
}

.close-btn {
  background: none;
  border: none;
  cursor: pointer;
  padding: 8px;
  color: var(--color-text-primary);
  display: flex;
  align-items: center;
  justify-content: center;
  transition: var(--transition-fast);
}

.close-btn:hover {
  color: var(--color-accent);
}

.drawer-content {
  flex: 1;
  overflow-y: auto;
  padding: 24px;
}

/* Empty and Success States */
.empty-state, .success-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  height: 80%;
  padding: 0 16px;
}

.empty-message {
  font-family: var(--font-serif);
  font-size: 1.25rem;
  color: var(--color-text-secondary);
  margin-bottom: 24px;
  font-style: italic;
}

.success-icon {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  border: 1px solid var(--color-accent);
  color: var(--color-accent);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.5rem;
  margin-bottom: 20px;
}

.success-title {
  font-size: 1.75rem;
  margin-bottom: 12px;
}

.success-message {
  font-size: 0.875rem;
  color: var(--color-text-secondary);
  margin-bottom: 24px;
  line-height: 1.7;
}

/* Cart Items List */
.cart-list {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.cart-item {
  display: flex;
  gap: 16px;
  padding-bottom: 24px;
  border-bottom: 1px solid var(--color-border);
}

.cart-item:last-child {
  border-bottom: none;
  padding-bottom: 0;
}

.item-img-container {
  width: 80px;
  height: 100px;
  background-color: var(--color-accent-light);
  border: 1px solid var(--color-border);
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--color-accent);
  padding: 8px;
}

.item-img-container :deep(svg) {
  width: 100%;
  height: 100%;
}

.item-details {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.item-meta {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 4px;
}

.item-category {
  font-size: 0.6875rem;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: var(--color-text-muted);
}

.item-remove-btn {
  background: none;
  border: none;
  color: var(--color-text-muted);
  cursor: pointer;
  padding: 4px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: var(--transition-fast);
}

.item-remove-btn:hover {
  color: #c93b3b;
}

.item-title {
  font-family: var(--font-sans);
  font-size: 0.9375rem;
  font-weight: 500;
  color: var(--color-text-primary);
  margin-bottom: 2px;
}

.item-variant {
  font-size: 0.75rem;
  color: var(--color-text-secondary);
  margin-bottom: 12px;
}

.item-controls {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: auto;
}

.qty-selector {
  display: flex;
  align-items: center;
  border: 1px solid var(--color-border);
  background: var(--color-bg);
}

.qty-btn {
  background: none;
  border: none;
  cursor: pointer;
  width: 28px;
  height: 28px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--color-text-secondary);
  transition: var(--transition-fast);
}

.qty-btn:hover:not(:disabled) {
  color: var(--color-text-primary);
  background-color: var(--color-border);
}

.qty-btn:disabled {
  opacity: 0.3;
  cursor: not-allowed;
}

.qty-value {
  font-size: 0.8125rem;
  width: 24px;
  text-align: center;
}

.item-price {
  font-size: 0.9375rem;
  font-weight: 500;
}

/* Drawer Footer */
.drawer-footer {
  padding: 24px;
  border-top: 1px solid var(--color-border);
  background: var(--color-surface-hover);
}

.summary-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 1rem;
  font-weight: 500;
  margin-bottom: 8px;
}

.summary-price {
  font-size: 1.125rem;
}

.shipping-info {
  font-size: 0.75rem;
  color: var(--color-text-secondary);
  margin-bottom: 20px;
}

.checkout-btn {
  width: 100%;
  display: flex;
  gap: 12px;
}

.checkout-btn .arrow {
  transition: transform var(--transition-fast);
}

.checkout-btn:hover .arrow {
  transform: translateX(4px);
}
</style>
