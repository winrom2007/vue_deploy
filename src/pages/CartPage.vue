<template>
<main class="content container">
    <div class="content__top">
      <ul class="breadcrumbs">
        <li class="breadcrumbs__item">
          <router-link class="breadcrumbs__link" href="index.html" :to="{name: 'main'}">
            Каталог
          </router-link>
        </li>
        <li class="breadcrumbs__item">
          <a class="breadcrumbs__link">
            Корзина
          </a>
        </li>
      </ul>

      <h1 class="content__title">
        Корзина
      </h1>
      <span class="content__info">
       {{productsCount}}
      </span>
    </div>

    <section class="cart">
      <form class="cart__form form" action="#" method="POST">
        <div class="cart__field">
          <ul class="cart__list">
           

            <CartItem v-for="item in products" :key="item.productId" :item="item"/>

            
          </ul>
        </div>

        <div class="cart__block">
          <p class="cart__desc">
            Мы&nbsp;посчитаем стоимость доставки на&nbsp;следующем этапе
          </p>
          <p class="cart__price">
            Итого: <span>{{totalPrice | numberFormat}} ₽</span>
          </p>

          <router-link tag="button" :to="{name: 'order'}" class="cart__button button button--primery" type="submit" v-if="products.length>0">
            Оформить заказ
          </router-link>
        </div>
      </form>
    </section>
  </main>
</template>

<script>
import numberFormat from "@/helpers/numberFormat"
import { mapGetters } from 'vuex';
import CartItem from '@/components/CartItem'

export default {
    filters: {numberFormat},
    components: {CartItem},
    computed: {
        ...mapGetters({products:'cartDetailProduct', totalPrice:'cartTotalPrice'}),
      //  products(){
      //      return $store.cartDetailProduct;
       // },

    },
    methods:{
      productsCount() {
        let n=''+this.products.length;
        let ls=Number(n.substr(n.length - 1))
        if(ls===1) return this.products.length+" товар"
        if(ls===2 || ls===3 || ls===4) return this.products.length+" товара"
        else return this.products.length+" товаров"
       }
    }
}
</script>