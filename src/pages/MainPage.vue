<template>
  <main class="content container">
    <div class="content__top content__top--catalog">
      <h1 class="content__title">Каталог</h1>
      <span class="content__info"> 152 товара </span>
    </div>

    <div class="content__catalog">
      <ProductFilter
        :price-from.sync="filterPriceFrom"
        :price-to.sync="filterPriceTo"
        :category-id.sync="filterCategoryId"
        :filter-colorid.sync="filterColorid"
      />
      <section class="catalog">
        <div v-if="productsLoading">Загрузка товаров</div>
        <div v-if="productsLoadingFailed">Произошла ошибка при загрузке товаров <button @click.prevent="loadProducts">Попробовать еще раз</button></div>
        <ProductList :products="products" />
        <BasePagination
          v-model="page"
          :count="countProducts"
          :per-page="productsPerPage"
        />
      </section>
    </div>
  </main>
</template>

<script>
//import products from "@/data/products";
import ProductList from "@/components/ProductList";
import BasePagination from "@/components/BasePagination";
import ProductFilter from "@/components/ProductFilter";
import axios from "axios";
import {API_BASE_URL} from '../config'
export default {
  components: { ProductList, BasePagination, ProductFilter },
  data() {
    return {
      filterPriceFrom: 0,
      filterPriceTo: 0,
      filterCategoryId: 0,
      page: 1,
      productsPerPage: 3,
      filterColorid: 0,

      productsData: null,
      productsLoading: false,
      productsLoadingFailed: false,
    };
  },
  computed: {
 /*   filteredProducts() {
       let filteredProducts = products;
      if (this.filterPriceFrom > 0) {
        filteredProducts = filteredProducts.filter(
          (product) => product.price > this.filterPriceFrom
        );
      }
      if (this.filterPriceTo > 0) {
        filteredProducts = filteredProducts.filter(
          (product) => product.price < this.filterPriceTo
        );
      }
      if (this.filterCategoryId) {
        filteredProducts = filteredProducts.filter(
          (product) => product.categoryId === this.filterCategoryId
        );
      }
      if (this.filterColorid) {
        filteredProducts = filteredProducts.filter((product) =>
          product.colorIds.includes(this.filterColorid)
        );
      }
      return filteredProducts; 
    },*/
    products() {
      return this.productsData 
      ? this.productsData.items.map(product =>{
        return {
          ...product,
          image: product.image.file.url
        }
      })
      : [];
    },
    countProducts() {
      return this.productsData ? this.productsData.pagination.total : 0
    },
  },
  methods: {
    loadProducts() {
      this.productsLoading = true;
      this.productsLoadingFailed = false;
      clearTimeout(this.loadProductsTimer);
      this.loadProductsTimer = setTimeout(() => {
        axios
        .get(API_BASE_URL+'/api/products',{
          params: {
            page: this.page,
            limit: this.productsPerPage,
            categoryId: this.filterCategoryId,
            minPrice:this.filterPriceFrom,
            maxPrice: this.filterPriceTo,
            colorId: this.filterColorid
          }
        })
        .then((response) => (this.productsData = response.data))
        .catch(()=>this.productsLoadingFailed=true)
        .then(()=> this.productsLoading=false)
      }, 0);
    },
  },
  watch: {
    page(){
      this.loadProducts();
    },
    filterPriceFrom(){
      this.loadProducts();
    },
    filterPriceTo(){
      this.loadProducts();
    },
    filterCategoryId(){
      this.loadProducts();
    },
    filterColorid(){
      this.loadProducts();
    },
  },
  created() {
    this.loadProducts();
  },
};
</script>
