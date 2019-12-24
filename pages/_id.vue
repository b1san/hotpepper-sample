<template>
  <section class="detail-page">
    <p v-if="error!==undefined">{{error.message}}</p>
    <div class="detail-page__header">
      <h2 class="detail-page__name">{{shop.name}}</h2>
      <div class="detail-page__genre">
        <span>{{shop.genre.name}}</span>
      </div>
    </div>  
    <a :href="shop.coupon_urls.pc" target="_blank" class="detail-page__url">{{shop.coupon_urls.pc}}</a>
    <p class="detail-page__catch">{{shop.catch}}</p>
    <ul class="detail-page__list">
      <li class="detail-page__item" v-for="(d, i) in details" :key="i">
        <p class="detail-page__item__title">{{d.title}}</p>
        <p class="detail-page__item__text">{{d.text === '' ? 'なし' : d.text}}</p>
      </li>
    </ul>
    <a @click="$router.back()" class="detail-page__back">戻る</a>
  </section>
</template>

<script>
export default {
  scrollTop: true,
  async asyncData({ $axios, params, env, redirect }) {
    try {
      const { data } = await $axios.get('http://localhost:3000/api/gourmet/v1/', {
        params: {
          key: env.apikey,
          id: params.id,
          format: 'json'
        }
      })
      if (data.results.shop.length <= 0) {
        redirect("/")
      }
      return {
        shop: data.results.shop[0],
        error: undefined
      }
    } catch (error) {
      console.log(error)
      return {
        shop: undefined,
        error: {
          message: "データの取得に失敗しました"
        }
      }
    }
  },
  computed: {
    details() {
      return [
        {title: '住所', text: this.shop.address},
        {title: '交通アクセス', text: this.shop.access},
        {title: '最寄り駅', text: this.shop.station_name},
        {title: '営業時間', text: this.shop.open},
        {title: '定休日', text: this.shop.close},
        {title: '平均ディナー予算', text: this.shop.budget.average},
        {title: '食べ放題', text: this.shop.free_drink},
        {title: '飲み放題', text: this.shop.free_food},
        {title: '料金備考', text: this.shop.budget_memo},
        {title: '席数', text: `${this.shop.capacity}席`},
        {title: '個室', text: this.shop.private_room},
        {title: '座敷', text: this.shop.tatami},
        {title: '備考', text: this.shop.shop_detail_memo}
      ]
    }
  }
}
</script>

<style lang="scss">
.detail-page {
  width: 100%;
  padding: 20px 30px;
  &__header {
    display: flex;
    align-items: center;
    padding: 20px 0;
  }
  &__name {
    font-size: 24px;
    font-weight: bold;
    margin-right: 30px;
  }
  &__genre {
    margin-top: -5px;
    & span {
      background: #e74c3c;
      border-radius: 3px;
      color: #fff;
      font-size: 14px;
      padding: 2px 8px;
    }
  }
  &__url {
    display: block;
    margin-bottom: 30px;
    border: 1px solid #ddd;
    padding: 7px 5px 7px 0px;
    color: #2d3436;
    font-size: 12px;
    text-decoration: none;
    border-radius: 5px;
    &::before {
      content: 'クーポン';
      color: #fff;
      background: #e74c3c;
      padding: 5.5px 5px 4.5px 10px;
      margin-right: 10px;
      border-top-left-radius: 5px;
      border-bottom-left-radius: 5px;
    }
  }
  &__catch {
    background: #ffeaa744;
    padding:  25px 40px 20px 40px;
    margin-bottom: 30px;
    position: relative;
    &::before {
      content: 'お店のキャッチ';
      position: absolute;
      top: -7.5px;
      left: 15px;
      font-weight: bold;
    }
  }
  &__item {
    margin-bottom: 35px;
    &__title {
      font-size: 16px;
      font-weight: bold;
      border-bottom: 1px solid #eee;
      margin-bottom: 12px;
      &::before {
        content: '◆';
        color: #e74c3c;
        margin-right: 7px;
      }
    }
  }
  &__back {
    display: block;
    border: 1px solid #ddd;
    border-radius: 3px;
    padding: 15px;
    cursor: pointer;
    text-align: center;
  }
}
</style>