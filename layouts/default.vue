<template>
  <div class="default-layout">
    <header>
      <h1 @click="$router.push('/')">Hot Pepper Sample App</h1>
    </header>
    <div class="content__wrapper">
      <main class="content">
        <nuxt />
      </main>
    </div>
  </div>
</template>

<style lang="scss">
.default-layout {
  min-height: 100vh;
  height: 100%;
  header {
    background: #c0392b;
    box-shadow: 0px 2px 3px rgba(0, 0, 0, 0.3);
    padding: 15px 70px;
    position: fixed;
    z-index: 10;
    top: 0;
    left: 0;
    width: 100%;
    &::before {
      content: '★';
      background: #fff;
      box-shadow: 0px 2px 3px rgba(0, 0, 0, 0.3);
      color: #c0392b;
      font-size: 28px;
      width: 40px;
      line-height: 56px;
      position: absolute;
      top: -3px;
      left: 15px;
      text-align: center;
    }
    h1 {
      color: #fff;
      font-size: 20px;
      font-weight: bold;
      cursor: pointer;
    }
  }
}

.content {
  background: #fff;
  max-width: 1080px;
  width: 100%;
  margin: 20px 10px 0px 10px;
  border: 1px solid #eee;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.3);
  &__wrapper {
    display: flex;
    justify-content: center;
    padding: 50px 0 20px 0;
    min-height: calc(100vh - 70px);
  }
}
</style>