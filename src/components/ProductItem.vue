<template>
  <li class="catalog__item">
    <router-link
      class="catalog__pic"
      href="#"
      :to="{ name: 'product', params: { id: product.id } }"
    >
      <img :src="product.image" :alt="product.title" />
    </router-link>

    <h3 class="catalog__title">
      <a href="#">
        {{ product.title }}
      </a>
    </h3>

    <span class="catalog__price">
      {{ product.price | numberFormat }}
    </span>

    <ul class="colors colors--black">
      <li class="colors__item" v-for="color in product.colors" :key="color.id">
        <label class="colors__label">
          <input class="colors__radio sr-only" type="radio" :value="color.code" />
          <span class="colors__value" :style="{ 'background-color': color.code }"> </span>
        </label>
      </li>
      <!-- <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" value="#8BE000" v-model="color">
                  <span class="colors__value" style="background-color: #8BE000;">
                  </span>
                </label>
              </li>
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" value="#222" v-model="color">
                  <span class="colors__value" style="background-color: #222;">
                  </span>
                </label>
              </li> -->
    </ul>
  </li>
</template>

<script>
import gotoPage from "@/helpers/gotoPage";
import colors from "/src/data/colors";
import numberFormat from "@/helpers/numberFormat";
export default {
  components: { colors },
  filters: {
    numberFormat,
  },
  props: ["product"],
  methods: {
    gotoPage,
  },
};
</script>
