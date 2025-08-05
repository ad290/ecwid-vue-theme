<template>
  <div class="card-grid-container">
    <button class="nav-arrow nav-left">&lt;</button>
    <div class="card-grid">
      <div
        v-for="(card, idx) in cards"
        :key="card.label"
        class="card-grid-item"
        :style="{ background: card.bg }"
        @mouseenter="hovered = idx"
        @mouseleave="hovered = null"
      >
        <img 
          :src="hovered === idx ? card.hoverImg : card.img" 
          :alt="card.label" 
          class="card-grid-img" 
        />
        <div class="card-grid-labels">
          <div class="card-pill">{{ card.label }}</div>
          <template v-if="hovered === idx">
            <button v-for="btn in card.buttons" :key="btn" class="card-pill card-pill-btn">{{ btn }}</button>
          </template>
        </div>
      </div>
    </div>
    <button class="nav-arrow nav-right">&gt;</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const hovered = ref(null)
const cards = [
  {
    label: 'BESTSELLERS',
    img: 'https://www.allbirds.com/cdn/shop/files/25Q2_BAU_Site_Homepage_CategoryTile-BestSellers_Studio-V2_Desktop_2x3_8ed4fb34-ee53-4e12-bbc0-30e45ecf6e83.jpg?v=1751421204&width=1024',
    hoverImg: 'https://www.allbirds.com/cdn/shop/files/25Q2_BAU_Site_Homepage_CategoryTile-BestSellers-Hover_Studio-V2_Desktop_2x3_1384e108-3ebe-41b1-b43b-2a38fbf2924d.jpg?v=1751421204&width=1024',
    bg: '#8ea2ad',
    buttons: ['SHOP MEN', 'SHOP WOMEN'],
  },
  {
    label: 'NEW ARRIVALS',
    img: 'https://www.allbirds.com/cdn/shop/files/25Q2_BAU_Site_Homepage_CategoryTile-NewArrivals_Studio-TreeRunnerNZ_Desktop_2x3_ec636811-3a31-4807-875c-72000dbe591a.jpg?v=1752599188&width=1024',
    hoverImg: 'https://www.allbirds.com/cdn/shop/files/25Q2_BAU_Site_Homepage_CategoryTile-NewArrivals-Hover_Studio-TreeRunnerNZ_Desktop_2x3_cbe7f145-3870-41ee-94c3-25505b343a59.jpg?v=1752599189&width=1024',
    bg: '#6b7352',
    buttons: ['SHOP MEN', 'SHOP WOMEN'],
  },
  {
    label: 'MENS',
    img: 'https://www.allbirds.com/cdn/shop/files/25Q2_BAU_Site_Homepage_CategoryTile-Men_Studio_Desktop_2x3_3217c505-0d21-494b-9ab1-b82f3ceec8b3.png?v=1751420466&width=1024',
    hoverImg: 'https://www.allbirds.com/cdn/shop/files/25Q2_BAU_Site_Homepage_CategoryTile-Men-Hover_Studio_Desktop_2x3_6f54de43-3951-4661-b10b-9121528fb3dc.png?v=1751420468&width=1024',
    bg: '#8d8e93',
    buttons: ['SHOP MEN', 'SHOP WOMEN'],
  },
  {
    label: 'WOMENS',
    img: 'https://www.allbirds.com/cdn/shop/files/25Q2_BAU_Site_Homepage_CategoryTile-Women_Studio_Desktop_2x3_59169f1b-c9cf-4d6c-956f-258ec8b6a300.png?v=1751420467&width=1024',
    hoverImg: 'https://www.allbirds.com/cdn/shop/files/25Q2_BAU_Site_Homepage_CategoryTile-Women-Hover_Studio_Desktop_2x3_aa3cc4aa-b4f3-4821-98fc-9bf79ea5a728.png?v=1751420467&width=1024',
    buttons: ['SHOP MEN', 'SHOP WOMEN'],
  },
]
</script>

<style scoped>
.card-grid-container {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 40px auto 0 auto;
  max-width: 1440px;
  padding: 0 24px;
  background: #ece9e2;
  border-radius: 32px;
}
.card-grid {
  display: flex;
  gap: 32px;
  justify-content: center;
  align-items: center;
  width: 100%;
  padding: 32px 0;
}
.card-grid-item {
  flex: 1 1 0;
  min-width: 0;
  min-height: 380px;
  max-width: 340px;
  border-radius: 32px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  position: relative;
  overflow: hidden;
  transition: all 0.3s ease;
  box-shadow: 0 2px 16px 0 rgba(0,0,0,0.08);
  cursor: pointer;
}
.card-grid-item:hover {
  transform: translateY(-4px);
  box-shadow: 0 4px 24px 0 rgba(0,0,0,0.12);
}
.card-grid-img {
  width: 90%;
  max-width: 320px;
  height: auto;
  margin: 0 auto 0 auto;
  display: block;
  z-index: 1;
  transition: transform 0.3s ease;
}
.card-grid-item:hover .card-grid-img {
  transform: scale(1.05);
}
.card-grid-labels {
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 16px;
  z-index: 2;
  transition: all 0.3s ease;
}
.card-pill {
  background: transparent;
  color: #fff;
  font-weight: 700;
  font-size: 0.9rem;
  padding: 8px 24px;
  border-radius: 32px;
  letter-spacing: 0.04em;
  text-align: center;
  box-shadow: 0 1px 4px rgba(0,0,0,0.04);
  margin-bottom: 0;
  transition: all 0.3s ease;
  pointer-events: none;
  border: 1px solid #fff;
  opacity: 1;
}
.card-pill-btn {
  background: transparent;
  color: #fff;
  border: 1px solid #fff;
  font-size: 0.9rem;
  font-weight: 700;
  margin-top: 0;
  margin-bottom: 0;
  padding: 8px 24px;
  border-radius: 32px;
  cursor: pointer;
  pointer-events: auto;
  transition: all 0.3s ease;
  box-shadow: 0 1px 4px rgba(0,0,0,0.04);
  opacity: 0;
  transform: translateY(10px);
}
.card-grid-item:hover .card-pill-btn {
  opacity: 1;
  transform: translateY(0);
}
.card-pill-btn:hover {
  background: rgba(255,255,255,0.1);
  color: #fff;
  transform: translateY(-2px);
}
.nav-arrow {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(255,255,255,0.8);
  border: none;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 18px;
  color: #666;
  cursor: pointer;
  transition: all 0.3s ease;
  z-index: 3;
}
.nav-arrow:hover {
  background: rgba(255,255,255,0.9);
  transform: translateY(-50%) scale(1.1);
}
.nav-left {
  left: 16px;
}
.nav-right {
  right: 16px;
}
@media (max-width: 1100px) {
  .card-grid {
    gap: 16px;
  }
  .card-grid-item {
    min-height: 260px;
    max-width: 220px;
  }
  .card-grid-img {
    max-width: 180px;
  }
  .nav-arrow {
    width: 32px;
    height: 32px;
    font-size: 14px;
  }
}
@media (max-width: 700px) {
  .card-grid {
    flex-wrap: wrap;
    gap: 16px;
  }
  .card-grid-item {
    flex: 1 1 45%;
    min-width: 140px;
    max-width: 100%;
    min-height: 180px;
  }
  .nav-arrow {
    display: none;
  }
}
@media (max-width: 500px) {
  .card-grid {
    flex-direction: column;
    gap: 12px;
  }
  .card-grid-item {
    min-width: 0;
    min-height: 120px;
    max-width: 100vw;
  }
}
</style> 