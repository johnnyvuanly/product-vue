<template>
  <div id="app">

    <h1>Shopping App</h1>

    <p v-for="order in orders">
      Product ID: {{order.id}} Quantity: {{ order.quantity }}
    </p>
    
    <!-- Being connected to the product prop Product.vue, to start with display the first product -->
    <product 
      v-for="productData in products" 
      v-bind:key="productData.id"
      v-bind:product="productData"
      v-bind:productApiUrl="productApiUrl"
      v-on:product-ordered="productOrdered">
    </product>
    
  </div>
</template>

<script>
import Product from './components/Product.vue'

export default {
  name: 'App',
  components: {
    Product
  },
  data() {
    return {
      productApiUrl: 'https://vue-2560-product.herokuapp.com/',
      products: [], // will be set by the api call in mounted
      orders: []
    }
  },
  mounted() { // copy from products2.html
    let productsURL = this.productApiUrl + 'api/products'
    fetch(productsURL)
      .then( response => response.json())
      .then( products => {
          this.products = products  // set Vue data to response from API
      })
  },
  methods: {
    productOrdered(productId, quantity) {
      let orderItem = {id: productId, quantity: quantity}
      this.orders.push(orderItem)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
