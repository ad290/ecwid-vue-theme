<template>
  <div class="announcement-bar">
    <slot>Free Shipping On Orders Over $75. Easy Returns.</slot>
  </div>
</template>

<style scoped>
.announcement-bar {
  width: 100vw;
  background: #333;
  color: #fff;
  text-align: center;
  font-size: 14px;
  font-weight: 500;
  padding: 12px 0;
  letter-spacing: 0.01em;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 200;
}
</style> 