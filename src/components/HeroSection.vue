<template>
  <section class="hero-main">
    <div class="hero-background">
      <img src="https://www.allbirds.com/cdn/shop/files/25Q2_BAU_Site_Homepage_Cruiser-Static-0716-Womens-Hero_Lifestyle_Desktop_1420x580_1.jpg?v=1753314432&width=2560" alt="Hero Background" />
    </div>
    <div class="hero-overlay">
      <div class="hero-label">THE NEW TREE RUNNER NZ AND CRUISER</div>
      <h1>New shoes, (even) more comfort.</h1>
      <div class="hero-buttons">
        <button>SHOP MEN</button>
        <button>SHOP WOMEN</button>
      </div>
    </div>
    <div class="hero-carousel-controls">
      <button class="nav-btn nav-up">&lt;</button>
      <button class="nav-btn nav-down">&gt;</button>
    </div>
  </section>
</template>

<script setup>
// No logic yet, static hero section
</script>

<style scoped>
.hero-main {
  position: relative;
  width: 100%;
  max-width: 1440px;
  margin: 140px auto 0 auto;
  border-radius: 24px;
  box-shadow: 0 2px 24px 0 rgba(0,0,0,0.10);
  overflow: hidden;
  height: 600px;
  background: #ece9e2;
}

.hero-background {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 1;
}

.hero-background img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.hero-overlay {
  position: absolute;
  right: 0;
  bottom: 0;
  top: 0;
  left: 0;
  z-index: 2;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  align-items: flex-start;
  padding: 0 56px 56px 56px;
  color: #fff;
  background: linear-gradient(90deg, rgba(0,0,0,0.10) 0%, rgba(0,0,0,0.00) 100%);
}

.hero-label {
  font-size: 14px;
  font-weight: 500;
  letter-spacing: 0.12em;
  margin-bottom: 18px;
  opacity: 0.85;
  text-transform: uppercase;
}

.hero-overlay h1 {
  font-size: 2.3rem;
  font-weight: 700;
  margin-bottom: 32px;
  line-height: 1.1;
  letter-spacing: 0.01em;
  text-align: left;
}

.hero-buttons {
  display: flex;
  gap: 18px;
}

.hero-buttons button {
  background: #fff;
  color: #222;
  border: none;
  border-radius: 32px;
  font-size: 1.1rem;
  font-weight: 700;
  text-transform: uppercase;
  padding: 16px 44px;
  margin: 0;
  letter-spacing: 0.08em;
  transition: background 0.2s, color 0.2s;
  box-shadow: 0 1px 4px rgba(0,0,0,0.04);
}

.hero-buttons button:hover {
  background: #222;
  color: #fff;
}

.hero-carousel-controls {
  position: absolute;
  right: 32px;
  top: 50%;
  transform: translateY(-50%);
  display: flex;
  flex-direction: column;
  gap: 8px;
  z-index: 3;
}

.nav-btn {
  background: rgba(255,255,255,0.9);
  border: none;
  border-radius: 50%;
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 14px;
  color: #666;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

.nav-btn:hover {
  background: rgba(255,255,255,1);
  transform: scale(1.1);
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
}

@media (max-width: 1100px) {
  .hero-main {
    height: 420px;
    border-radius: 20px;
  }
  .hero-overlay {
    padding: 0 24px 24px 24px;
  }
  .hero-overlay h1 {
    font-size: 1.4rem;
    margin-bottom: 18px;
  }
  .hero-buttons button {
    font-size: 1rem;
    padding: 12px 24px;
  }
  .nav-btn {
    width: 28px;
    height: 28px;
    font-size: 12px;
  }
}

@media (max-width: 700px) {
  .hero-main {
    height: auto;
    min-height: 320px;
    border-radius: 16px;
    width: 99vw;
  }
  .hero-overlay {
    padding: 7vw 3vw 3vw 3vw;
  }
  .hero-buttons {
    flex-direction: column;
    gap: 12px;
    width: 100%;
  }
  .hero-buttons button {
    width: 100%;
    padding: 12px 0;
  }
}
</style> 