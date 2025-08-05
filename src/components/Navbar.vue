<template>
  <nav class="navbar-outer">
    <div class="navbar-inner">
      <div class="navbar-left">
        <div class="logo-container">
          <img src="/images/Navbar/allbirds-logo.svg" alt="Allbirds logo" class="navbar-logo" />
        </div>
      </div>
      <ul class="navbar-center">
        <li>MEN</li>
        <li>WOMEN</li>
        <li>SALE</li>
      </ul>
      <div class="navbar-right">
        <ul class="navbar-links">
          <li>Our Stores</li>
          <li>About</li>
          <li>ReRun</li>
        </ul>
        <div class="navbar-icons">
          <svg class="icon-outline" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="#222" stroke-width="2">
            <path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"/>
            <circle cx="12" cy="7" r="4"/>
          </svg>
          <svg class="icon-outline" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="#222" stroke-width="2">
            <circle cx="12" cy="12" r="10"/>
            <path d="M9.09 9a3 3 0 0 1 5.83 1c0 2-3 3-3 3"/>
            <line x1="12" y1="17" x2="12.01" y2="17"/>
          </svg>
          <svg class="icon-outline cart-icon" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="#222" stroke-width="2">
            <circle cx="9" cy="21" r="1"/>
            <circle cx="20" cy="21" r="1"/>
            <path d="M1 1h4l2.68 13.39a2 2 0 0 0 2 1.61h9.72a2 2 0 0 0 2-1.61L23 6H6"/>
          </svg>
          <div class="cart-badge">1</div>
        </div>
      </div>
    </div>
  </nav>
</template>

<script setup>
// Hamburger menu logic can be added for mobile
</script>

<style scoped>
.navbar-outer {
  width: 100vw;
  display: flex;
  justify-content: center;
  position: fixed;
  top: 44px;
  left: 0;
  z-index: 100;
  background: transparent;
  padding: 16px 0;
}
.navbar-inner {
  background: #fff;
  border-radius: 12px;
  box-shadow: 0 2px 16px 0 rgba(0,0,0,0.08);
  display: flex;
  align-items: center;
  width: 100%;
  max-width: 1440px;
  min-height: 64px;
  margin: 0 auto;
  padding: 0 32px;
}
.navbar-left {
  flex: 1 1 0;
  display: flex;
  align-items: center;
}
.logo-container {
  background: #fff;
  border-radius: 12px;
  padding: 8px 16px;
  box-shadow: 0 1px 4px rgba(0,0,0,0.1);
}
.navbar-logo {
  width: 100px;
  height: auto;
  display: block;
}
.navbar-center {
  flex: 2 1 0;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 48px;
  list-style: none;
  font-size: 16px;
  font-weight: 600;
  color: #111;
  letter-spacing: 0.01em;
}
.navbar-center li {
  cursor: pointer;
  padding: 0 8px;
  transition: color 0.2s;
}
.navbar-center li:hover {
  color: #888;
}
.navbar-right {
  flex: 1 1 0;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  gap: 24px;
}
.navbar-links {
  display: flex;
  gap: 16px;
  list-style: none;
  font-size: 14px;
  font-weight: 500;
  color: #111;
}
.navbar-links li {
  cursor: pointer;
  padding: 0 6px;
  transition: color 0.2s;
  white-space: nowrap;
  text-align: left;
}
.navbar-links li:hover {
  color: #888;
}
.navbar-icons {
  display: flex;
  align-items: center;
  gap: 16px;
  margin-left: 16px;
  position: relative;
}
.icon-outline {
  width: 24px;
  height: 24px;
  display: block;
  cursor: pointer;
  transition: stroke 0.2s;
}
.icon-outline:hover {
  stroke: #666;
}
.cart-icon {
  position: relative;
}
.cart-badge {
  position: absolute;
  top: -6px;
  right: -6px;
  background: #222;
  color: #fff;
  border-radius: 50%;
  width: 16px;
  height: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 10px;
  font-weight: 600;
}
@media (max-width: 900px) {
  .navbar-inner {
    min-height: 56px;
    padding: 0 12px;
    max-width: 98vw;
  }
  .navbar-logo {
    width: 80px;
  }
  .navbar-center {
    font-size: 14px;
    gap: 18px;
  }
  .navbar-links {
    font-size: 12px;
    gap: 8px;
  }
  .icon-outline {
    width: 20px;
    height: 20px;
  }
}
@media (max-width: 600px) {
  .navbar-inner {
    flex-direction: column;
    min-height: 48px;
    padding: 0.5em 0.5em;
    width: 98vw;
  }
  .navbar-center, .navbar-links, .navbar-icons {
    display: none;
  }
  /* Hamburger menu styles would go here */
}
</style> 