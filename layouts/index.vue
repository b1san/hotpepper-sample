<template>
  <div>
    <header class="header">
      <div class="header__content__wrapper">
        <div class="header__content">
          <h1>Hot Pepper Sample App</h1>
          <p>～ 近くのお店を表示します ～</p>
        </div>
      </div>
      <div class="header__scroll">
        <p class="header__scroll__mark">↓</p>
      </div>
    </header>
    <main>
      <nuxt />
    </main>
  </div>
</template>

<style lang="scss">
.header {
  background: url(../assets/img/header.jpg) center center no-repeat;
  background-attachment: fixed;
  background-size: cover;
  height: 100vh;
  width: 100vw;
  position: relative;
  &__content {
    color: #fff;
    font-weight: bold;
    text-align: center;
    &__wrapper {
      background: rgba(0, 0, 0, 0.7);
      height: 100%;
      width: 100%;
      display: flex;
      align-items: center;
      justify-content: center;
    }
  }
  & h1 {
    font-size: 36px;
    margin-bottom: 30px;
  }
  & p {
    font-size: 20px;
  }
  &__scroll {
    position: absolute;
    bottom: 30px;
    left: calc(50% - 20px);
    text-align: center;
    &__mark {
      border: 1px solid #fff;
      border-radius: 50%;
      color: #fff;
      font-size: 24px;
      line-height: 40px;
      width: 40px;
      animation: f1 1s linear infinite;
    }
  }
}

main {
  display: flex;
  justify-content: center;
  padding-bottom: 40px;
}

@keyframes f1 {
  0%, 100% {
    transform: translateY(0);
    opacity: 1;
  }
  50% {
    transform: translateY(-10px);
    opacity: 0.6;
  }
}
</style>
