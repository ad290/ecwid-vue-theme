<template>
  <div class="product-carousel-container">
    <!-- Category Selection -->
    <div class="category-selection">
      <label for="category-select" class="category-label">Select Category ID:</label>
      <select 
        id="category-select" 
        v-model="selectedCategoryId" 
        @change="loadProductsByCategory"
        class="category-select"
      >
        <option value="">Select a category</option>
        <option value="new">NEW ARRIVALS</option>
        <option value="best">BESTSELLERS</option>
        <option value="summer">SUMMER SHOES</option>
      </select>
    </div>

    <!-- Category Navigation Tabs -->
    <div class="category-tabs">
      <button
        v-for="category in categories"
        :key="category.id"
        @click="selectCategory(category.id)"
        :class="['category-tab', { active: selectedCategoryId === category.id }]"
      >
        {{ category.name }}
      </button>
    </div>

    <!-- Shoe Display -->
    <div class="shoe-display-container">
      <button 
        @click="previousSlide" 
        class="nav-arrow prev"
        :disabled="currentSlide === 0"
      >
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none">
          <path d="M15 18L9 12L15 6" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </button>

      <div class="shoe-display">
        <div
          v-for="(product, index) in visibleProducts"
          :key="product.id"
          :class="[
            'shoe-item',
            {
              'left-shoe': index === 0,
              'center-shoe': index === 1,
              'right-shoe': index === 2
            }
          ]"
        >
          <img 
            :src="product.image" 
            :alt="product.title"
            class="shoe-image"
            @click="selectProduct(product)"
          />
        </div>
      </div>

      <button 
        @click="nextSlide" 
        class="nav-arrow next"
        :disabled="currentSlide >= maxSlide"
      >
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none">
          <path d="M9 18L15 12L9 6" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </button>
    </div>

    <!-- Product info panel -->
    <div
      v-if="activeProduct"
      class="product-info-panel"
    >
      <h3 class="product-title">
        {{ activeProduct.title }}
      </h3>

      <div class="product-details">
        <span class="product-description">{{ activeProduct.description }}</span>
        <span class="separator">-</span>
        <span class="product-price">{{ activeProduct.price }}</span>
      </div>

      <div class="shop-buttons">
        <a
          v-if="activeProduct.mensUrl"
          :href="activeProduct.mensUrl"
          class="shop-button"
          target="_blank"
        >
          Shop Men
        </a>
        <a
          v-if="activeProduct.womensUrl"
          :href="activeProduct.womensUrl"
          class="shop-button"
          target="_blank"
        >
          Shop Women
        </a>
      </div>
    </div>

    <!-- Ecwid Integration Status -->
    <div class="ecwid-status">
      <p class="status-text">
        <strong>Status:</strong> 
        <span :class="['status-indicator', { 'connected': isEcwidConnected }]">
          {{ isEcwidConnected ? 'Connected to Ecwid' : 'Using Demo Data' }}
        </span>
      </p>
      <p class="category-info">
        <strong>Category ID:</strong> {{ selectedCategoryId || 'None selected' }}
      </p>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'

// Ecwid Integration Status
const isEcwidConnected = ref(false)

// Category and Product Management
const selectedCategoryId = ref('')
const currentSlide = ref(0)

// Dummy Categories (for demo)
const categories = [
  { id: 'new', name: 'NEW ARRIVALS' },
  { id: 'best', name: 'BESTSELLERS' },
  { id: 'summer', name: 'SUMMER SHOES' }
]

// Dummy Products (for demo)
const products = [
  // NEW ARRIVALS
  {
    id: 'new-1',
    category: 'new',
    title: 'Tree Gliders',
    description: 'Thunder Green (Stony Cream Sole)',
    price: '$135',
    image: 'https://www.allbirds.com/cdn/shop/files/25Q2_BAU_Site_Homepage_CategoryTile-BestSellers_Studio-V2_Desktop_2x3_8ed4fb34-ee53-4e12-bbc0-30e45ecf6e83.jpg?v=1751421204&width=1024',
    mensUrl: 'https://www.allbirds.com/products/mens-tree-gliders-thunder-green',
    womensUrl: 'https://www.allbirds.com/products/womens-tree-gliders-thunder-green'
  },
  {
    id: 'new-2',
    category: 'new',
    title: 'Wool Runners',
    description: 'Natural White (Natural Sole)',
    price: '$110',
    image: 'https://www.allbirds.com/cdn/shop/files/25Q2_BAU_Site_Homepage_CategoryTile-BestSellers-Hover_Studio-V2_Desktop_2x3_1384e108-3ebe-41b1-b43b-2a38fbf2924d.jpg?v=1751421204&width=1024',
    mensUrl: 'https://www.allbirds.com/products/mens-wool-runners-natural-white',
    womensUrl: 'https://www.allbirds.com/products/womens-wool-runners-natural-white'
  },
  {
    id: 'new-3',
    category: 'new',
    title: 'Tree Dashers',
    description: 'Black (Natural Sole)',
    price: '$125',
    image: 'https://www.allbirds.com/cdn/shop/files/25Q2_BAU_Site_Homepage_CategoryTile-BestSellers_Studio-V2_Desktop_2x3_8ed4fb34-ee53-4e12-bbc0-30e45ecf6e83.jpg?v=1751421204&width=1024',
    mensUrl: 'https://www.allbirds.com/products/mens-tree-dashers-black',
    womensUrl: 'https://www.allbirds.com/products/womens-tree-dashers-black'
  },
  // BESTSELLERS
  {
    id: 'best-1',
    category: 'best',
    title: 'Wool Runners',
    description: 'Natural White (Natural Sole)',
    price: '$110',
    image: 'https://www.allbirds.com/cdn/shop/files/25Q2_BAU_Site_Homepage_CategoryTile-BestSellers-Hover_Studio-V2_Desktop_2x3_1384e108-3ebe-41b1-b43b-2a38fbf2924d.jpg?v=1751421204&width=1024',
    mensUrl: 'https://www.allbirds.com/products/mens-wool-runners-natural-white',
    womensUrl: 'https://www.allbirds.com/products/womens-wool-runners-natural-white'
  },
  {
    id: 'best-2',
    category: 'best',
    title: 'Tree Dashers',
    description: 'Black (Natural Sole)',
    price: '$125',
    image: 'https://www.allbirds.com/cdn/shop/files/25Q2_BAU_Site_Homepage_CategoryTile-BestSellers_Studio-V2_Desktop_2x3_8ed4fb34-ee53-4e12-bbc0-30e45ecf6e83.jpg?v=1751421204&width=1024',
    mensUrl: 'https://www.allbirds.com/products/mens-tree-dashers-black',
    womensUrl: 'https://www.allbirds.com/products/womens-tree-dashers-black'
  },
  {
    id: 'best-3',
    category: 'best',
    title: 'Tree Gliders',
    description: 'Thunder Green (Stony Cream Sole)',
    price: '$135',
    image: 'https://www.allbirds.com/cdn/shop/files/25Q2_BAU_Site_Homepage_CategoryTile-BestSellers-Hover_Studio-V2_Desktop_2x3_1384e108-3ebe-41b1-b43b-2a38fbf2924d.jpg?v=1751421204&width=1024',
    mensUrl: 'https://www.allbirds.com/products/mens-tree-gliders-thunder-green',
    womensUrl: 'https://www.allbirds.com/products/womens-tree-gliders-thunder-green'
  },
  // SUMMER SHOES
  {
    id: 'summer-1',
    category: 'summer',
    title: 'Tree Breezers',
    description: 'Natural White (Natural Sole)',
    price: '$95',
    image: 'https://www.allbirds.com/cdn/shop/files/25Q2_BAU_Site_Homepage_CategoryTile-BestSellers_Studio-V2_Desktop_2x3_8ed4fb34-ee53-4e12-bbc0-30e45ecf6e83.jpg?v=1751421204&width=1024',
    mensUrl: 'https://www.allbirds.com/products/mens-tree-breezers-natural-white',
    womensUrl: 'https://www.allbirds.com/products/womens-tree-breezers-natural-white'
  },
  {
    id: 'summer-2',
    category: 'summer',
    title: 'Wool Loungers',
    description: 'Natural White (Natural Sole)',
    price: '$95',
    image: 'https://www.allbirds.com/cdn/shop/files/25Q2_BAU_Site_Homepage_CategoryTile-BestSellers-Hover_Studio-V2_Desktop_2x3_1384e108-3ebe-41b1-b43b-2a38fbf2924d.jpg?v=1751421204&width=1024',
    mensUrl: 'https://www.allbirds.com/products/mens-wool-loungers-natural-white',
    womensUrl: 'https://www.allbirds.com/products/womens-wool-loungers-natural-white'
  },
  {
    id: 'summer-3',
    category: 'summer',
    title: 'Tree Skippers',
    description: 'Natural White (Natural Sole)',
    price: '$95',
    image: 'https://www.allbirds.com/cdn/shop/files/25Q2_BAU_Site_Homepage_CategoryTile-BestSellers_Studio-V2_Desktop_2x3_8ed4fb34-ee53-4e12-bbc0-30e45ecf6e83.jpg?v=1751421204&width=1024',
    mensUrl: 'https://www.allbirds.com/products/mens-tree-skippers-natural-white',
    womensUrl: 'https://www.allbirds.com/products/womens-tree-skippers-natural-white'
  }
]

// Computed Properties
const maxSlide = computed(() => {
  const catProducts = products.filter(p => p.category === selectedCategoryId.value)
  return Math.max(0, catProducts.length - 1)
})

const visibleProducts = computed(() => {
  const catProducts = products.filter(p => p.category === selectedCategoryId.value)
  const currentIndex = currentSlide.value
  const totalProducts = catProducts.length
  
  if (totalProducts === 0) return []
  
  // Always show 3 products (left, center, right)
  const leftIndex = (currentIndex - 1 + totalProducts) % totalProducts
  const centerIndex = currentIndex
  const rightIndex = (currentIndex + 1) % totalProducts
  
  return [
    catProducts[leftIndex],
    catProducts[centerIndex],
    catProducts[rightIndex]
  ]
})

const activeProduct = computed(() => {
  const catProducts = products.filter(p => p.category === selectedCategoryId.value)
  return catProducts[currentSlide.value]
})

// Methods
function selectCategory(id) {
  selectedCategoryId.value = id
  currentSlide.value = 0
}

function loadProductsByCategory() {
  currentSlide.value = 0
  // In a real implementation, this would fetch products from Ecwid API
  console.log('Loading products for category:', selectedCategoryId.value)
}

function selectProduct(product) {
  // In a real Ecwid integration, you'd navigate to the product's URL
  console.log('Selected product:', product)
  // Example: window.location.href = product.url
}

function nextSlide() {
  const catProducts = products.filter(p => p.category === selectedCategoryId.value)
  if (currentSlide.value < catProducts.length - 1) {
    currentSlide.value++
  }
}

function previousSlide() {
  if (currentSlide.value > 0) {
    currentSlide.value--
  }
}

// Ecwid Integration Functions
function connectToEcwid() {
  // This would be the actual Ecwid API integration
  console.log('Connecting to Ecwid...')
  // Example implementation:
  // 1. Initialize Ecwid API
  // 2. Fetch categories
  // 3. Fetch products by category
  // 4. Update isEcwidConnected status
}

function fetchEcwidCategories() {
  // This would fetch categories from Ecwid API
  console.log('Fetching categories from Ecwid...')
  return categories // For demo, return dummy categories
}

function fetchEcwidProducts(categoryId) {
  // This would fetch products from Ecwid API based on category ID
  console.log('Fetching products for category:', categoryId)
  return products.filter(p => p.category === categoryId) // For demo, return dummy products
}

// Lifecycle
onMounted(() => {
  // Initialize with first category
  if (categories.length > 0) {
    selectedCategoryId.value = categories[0].id
  }
  
  // In a real implementation, you would:
  // 1. Check if Ecwid is available
  // 2. Connect to Ecwid API
  // 3. Load initial data
  console.log('ProductCarousel component mounted')
})
</script>

<style scoped>
.product-carousel-container {
  background-color: #FDFBF6;
  padding: 80px 0 60px 0;
  position: relative;
}

.category-selection {
  margin-bottom: 20px;
  text-align: center;
}

.category-label {
  display: block;
  margin-bottom: 10px;
  font-family: 'Helvetica Neue', Arial, sans-serif;
  font-size: 14px;
  color: #666;
}

.category-select {
  padding: 10px 15px;
  border: 1px solid #E0E0E0;
  border-radius: 8px;
  font-size: 14px;
  color: #333;
  background-color: #FFFFFF;
  cursor: pointer;
  transition: all 0.3s ease;
  width: 250px;
  margin: 0 auto;
}

.category-select:focus {
  outline: none;
  border-color: #D0D0D0;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
}

.category-tabs {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-bottom: 40px;
  flex-wrap: wrap;
}

.category-tab {
  background-color: transparent;
  color: #9E9E9E;
  border: 1px solid #E0E0E0;
  border-radius: 25px;
  padding: 12px 24px;
  font-family: 'Helvetica Neue', Arial, sans-serif;
  font-size: 13px;
  font-weight: 400;
  letter-spacing: 1.5px;
  text-transform: uppercase;
  cursor: pointer;
  transition: all 0.3s ease;
  position: relative;
  min-width: 120px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

.category-tab.active {
  color: #000000;
  background-color: transparent;
  border-color: #D0D0D0;
}

.category-tab.active::after {
  content: '';
  position: absolute;
  bottom: -2px;
  left: 50%;
  transform: translateX(-50%);
  width: 80%;
  height: 2px;
  background-color: #000000;
  border-radius: 1px;
}

.category-tab:hover {
  color: #000000;
  background-color: rgba(255, 255, 255, 0.1);
  border-color: #C0C0C0;
}

/* Shoe Display styles */
.shoe-display-container {
  position: relative;
  width: 100%;
  height: 500px;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 40px;
}

.shoe-display {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
  gap: 40px;
  overflow: hidden; /* Hide overflow to show only 3 items */
}

.shoe-item {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: all 0.5s ease;
  cursor: pointer;
}

.shoe-item.left-shoe {
  transform: scale(0.75) translateX(-80px);
  opacity: 0.7;
  z-index: 1;
}

.shoe-item.center-shoe {
  transform: scale(1.1);
  opacity: 1;
  z-index: 3;
}

.shoe-item.right-shoe {
  transform: scale(0.75) translateX(80px);
  opacity: 0.7;
  z-index: 1;
}

.shoe-image {
  max-width: 500px;
  max-height: 400px;
  object-fit: contain;
  filter: drop-shadow(0 4px 8px rgba(0, 0, 0, 0.1));
  transition: all 0.3s ease;
}

.shoe-image:hover {
  transform: scale(1.05);
  filter: drop-shadow(0 6px 12px rgba(0, 0, 0, 0.15));
}

/* Navigation arrows */
.nav-arrow {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(255, 255, 255, 0.95);
  border: 1px solid #E0E0E0;
  width: 48px;
  height: 48px;
  border-radius: 50%;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #666;
  z-index: 15;
  transition: all 0.3s ease;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.15);
}

.nav-arrow:hover:not(:disabled) {
  background: rgba(255, 255, 255, 1);
  color: #333;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.2);
  border-color: #D0D0D0;
  transform: translateY(-50%) scale(1.05);
}

.nav-arrow:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.nav-arrow.prev {
  left: 50px;
}

.nav-arrow.next {
  right: 50px;
}

/* Product info styles */
.product-info-panel {
  position: relative;
  z-index: 10;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  margin-top: 40px;
}

.product-title {
  font-family: Georgia, serif;
  font-size: 32px;
  font-weight: 400;
  color: #000000;
  margin-bottom: 12px;
  line-height: 1.2;
}

.product-details {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 20px;
  font-family: 'Helvetica Neue', Arial, sans-serif;
  font-size: 14px;
  color: #000000;
}

.product-description {
  font-weight: 400;
}

.separator {
  font-weight: 400;
}

.product-price {
  font-weight: 500;
  letter-spacing: 1px;
  text-transform: uppercase;
}

.shop-buttons {
  display: flex;
  gap: 12px;
}

.shop-button {
  display: inline-block;
  padding: 12px 24px;
  border: 1px solid #000000;
  background-color: #FFFFFF;
  color: #000000;
  text-decoration: none;
  font-family: 'Helvetica Neue', Arial, sans-serif;
  font-size: 14px;
  font-weight: 500;
  letter-spacing: 1px;
  text-transform: uppercase;
  border-radius: 25px;
  transition: all 0.3s ease;
  cursor: pointer;
  min-width: 120px;
}

.shop-button:hover {
  background-color: #000000;
  color: #FFFFFF;
}

.btn {
  display: inline-block;
  padding: 12px 24px;
  border: 1px solid #000;
  background: transparent;
  color: #000;
  text-decoration: none;
  font-size: 14px;
  font-weight: 500;
  letter-spacing: 1px;
  text-transform: uppercase;
  transition: all 0.3s ease;
  cursor: pointer;
}

.btn:hover {
  background: #000;
  color: #fff;
}

.btn-outline-black {
  border-color: #000;
  color: #000;
}

.btn-outline-black:hover {
  background: #000;
  color: #fff;
}

.min-w-32 {
  min-width: 128px;
}

/* Ecwid Integration Status styles */
.ecwid-status {
  position: absolute;
  top: 20px;
  right: 20px;
  background-color: rgba(255, 255, 255, 0.9);
  padding: 10px 20px;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  z-index: 100;
  font-family: 'Helvetica Neue', Arial, sans-serif;
  font-size: 12px;
  color: #333;
}

.status-text {
  margin-bottom: 5px;
}

.status-indicator {
  font-weight: 600;
  color: #4CAF50; /* Green for connected */
}

.status-indicator.connected {
  color: #4CAF50;
}

.category-info {
  font-size: 11px;
  color: #666;
}

/* Responsive design */
@media (max-width: 768px) {
  .product-carousel-container {
    padding: 60px 0 40px 0;
  }

  .shoe-display-container {
    height: 450px;
  }
  
  .shoe-image {
    max-width: 400px;
    max-height: 320px;
  }
  
  .shoe-item.left-shoe {
    transform: scale(0.7) translateX(-60px);
  }
  
  .shoe-item.center-shoe {
    transform: scale(1);
  }
  
  .shoe-item.right-shoe {
    transform: scale(0.7) translateX(60px);
  }
  
  .nav-arrow {
    width: 40px;
    height: 40px;
  }
  
  .nav-arrow.prev {
    left: 20px;
  }
  
  .nav-arrow.next {
    right: 20px;
  }
  
  .category-tab {
    font-size: 12px;
    letter-spacing: 1px;
    padding: 10px 20px;
    min-width: 100px;
  }
  
  .product-title {
    font-size: 28px;
  }
  
  .shop-button {
    padding: 10px 20px;
    min-width: 100px;
  }

  .ecwid-status {
    top: 10px;
    right: 10px;
    padding: 8px 15px;
    font-size: 10px;
  }

  .category-info {
    font-size: 10px;
  }
}

@media (max-width: 480px) {
  .product-carousel-container {
    padding: 40px 0 20px 0;
  }

  .shoe-display-container {
    height: 350px;
  }
  
  .shoe-image {
    max-width: 300px;
    max-height: 240px;
  }
  
  .shoe-item.left-shoe {
    transform: scale(0.6) translateX(-40px);
  }
  
  .shoe-item.center-shoe {
    transform: scale(0.9);
  }
  
  .shoe-item.right-shoe {
    transform: scale(0.6) translateX(40px);
  }
  
  .nav-arrow {
    width: 36px;
    height: 36px;
  }
  
  .nav-arrow.prev {
    left: 10px;
  }
  
  .nav-arrow.next {
    right: 10px;
  }
  
  .category-tab {
    font-size: 11px;
    padding: 8px 16px;
    min-width: 80px;
  }
  
  .product-title {
    font-size: 24px;
  }
  
  .shop-button {
    padding: 8px 16px;
    min-width: 80px;
    font-size: 12px;
  }

  .ecwid-status {
    top: 5px;
    right: 5px;
    padding: 6px 12px;
    font-size: 9px;
  }

  .category-info {
    font-size: 9px;
  }
}
</style> 