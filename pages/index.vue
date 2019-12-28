<template>
  <section class="index-page">
    <h2>検索結果（{{shops.length}}件）</h2>
    <p class="error__message" v-if="error">データの取得に失敗しました。</p>
    <ul>
      <li v-for="shop in shops" :key="shop.id" @click="$router.push(`/${shop.id}`)" class="card">
        <div class="card__img">
          <img :src="shop.photo.pc.l">
        </div>
        <div class="card__content">
          <p class="card__name">{{shop.name}}</p>
          <div class="card__genre">
            <span>{{shop.genre.name}}</span>
          </div>
          <p class="card__text">{{shop.address}}</p>
          <p class="card__text">{{shop.open}}</p>
          <p class="card__text">{{shop.budget.average}}</p>
        </div>
      </li>
    </ul>
  </section>
</template>

<script>
const getCurrentPosition = () => {
  return new Promise((resolve, reject) => {
    navigator.geolocation.getCurrentPosition(resolve, reject)
  })
}

export default {
  layout: 'index',
  scrollTop: true,
  data() {
    return {
      shops: [],
      error: false
    }
  },
  methods: {
    setError(error) {
      console.log(error)
      this.error = true
    }
  },
  async mounted() {
    try {
      //現在地を取得する
      const position = await getCurrentPosition().catch(this.setError)
      //APIからデータを取得
      const { data } = await this.$axios.get('http://localhost:3000/api/gourmet/v1/',{
        params: {
          key: process.env.apikey,
          lat: position.coords.latitude,
          lng: position.coords.longitude,
          format: 'json'
        }
      })
      //エラーを取得した場合
      if (data.results.error !== undefined) {
        this.error = true
        return;
      }
      //取得したデータを設定
      this.shops = data.results.shop
    } catch(error) {
      this.setError(error)
    }
  }
}
</script>

<style lang="scss">
.index-page {
  max-width: 1240px;
  width: 100%;
  h2 {
    background: #fff;
    color: #2c3e50;
    font-size: 20px;
    font-weight: bold;
    margin: 40px 20px 20px 20px;
    padding: 15px 70px;
    position: relative;
    &::before {
      content: '★';
      background: #e74c3c;
      box-shadow: 0px 2px 3px rgba(0, 0, 0, 0.3);
      color: #fff;
      font-size: 26px;
      line-height: 60px;
      width: 40px;
      text-align: center;
      position: absolute;
      left: 14px;
      top: -5px;
    }
  }
}
.card {
  background: #fff;
  border: 1px solid #eee;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.3);
  cursor: pointer;
  margin: 10px;
  padding: 10px;
  display: flex;
  transition: all 0.4s;
  &:hover {
    box-shadow: 1px 2px 4px rgba(0, 0, 0, 0.5);
  }
  &__img {
    margin-right: 12px;
    & img {
      width: 250px;
      height: 100%;
      object-fit: cover
    }
  }
  &__content {
    color: #333;
    padding: 8px;
  }
  &__name {
    font-size: 20px;
    font-weight: bold;
  }
  &__genre {
    margin: 12px 0;
    > span {
      background: #e74c3c;
      border-radius: 3px;
      color: #fff;
      font-size: 14px;
      padding: 2px 8px;
    }
  }
  &__text {
    font-size: 18px;
    line-height: 28px;
  }
}
</style>