<script>
import FilterList from "@/components/layout/filter.vue";
import Search from "@/components/layout/search.vue";
import ProductList from "@/components/layout/product-list.vue";

export default {
  components: {
    FilterList,
    Search,
    ProductList
  },
  props: {
    filters: {
      type: Array,
      required: true
    },
    products: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      changedFilter: "Все",
      searchValue: ""
    }
  },
  computed: {
    filtration(){
      if(this.changedFilter == "Все") return this.products
      return [...this.products].filter(item => item.contry === this.changedFilter);
    },
    searching(){
      return this.filtration.filter(item => item.name.toLocaleLowerCase().includes(this.searchValue.toLocaleLowerCase()))
    },
    productsCount(){
      this.filters.forEach(item => {
        if(item.name === "Все") item.amount = this.products.length;
        else item.amount = this.products.filter(x => x.contry === item.name).length
      });
      return this.filters;
    }
  }
}

</script>
<template>

  <div class="catalog">
    <div class="row no-gutters">

      <!---Левая половина--->
      <div class="col-xl-3 col-lg-4 col-md-5">
        <div class="p-3">
        
          <filter-list v-model="changedFilter" :filters="productsCount"/><!---Фильтр--->

        </div>
      </div>

      <!---Правая половина--->
      <div class="col-xl-9 col-lg-8 col-md-7 p-3">

        <search v-model="searchValue" :search-value="searchValue"/><!---Поиск--->
        <product-list :products="searching" @add-to-cart="$emit('add-to-cart', $event)"/><!---Лист товаров--->

      </div>

    </div>
  </div>

</template>

<style scoped>
.catalog {
  width: 100%;
  height: 100%;
  padding: 10px;
  margin-top: 70px;
}
</style>
