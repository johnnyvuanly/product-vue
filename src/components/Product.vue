<template>
    
    <div> <!-- add a div -->

        <h1>{{ product.productName }}</h1>

        <!-- v-bind sets HTML attribute values using Vue data-->
        <!-- Setting the src attribute for an image, using data from the API server  -->
        <img v-bind:src="productApiUrl + 'img/' + product.image">

        <p>{{ product.description }}</p>

        <!--    TODO try changing this data in Vue dev tools -->
        <p v-if="product.available">This product is available!</p>
        <!-- comments here are ok! don't add any other HTML here though.  -->
        <p v-else>Sorry, this product is not available. Please check back later.</p>

        <p>Quantity Available: {{ product.quantityAvailable }}</p>

        <ul>
            <li v-for="feature in product.features">{{ feature }}</li>
        </ul>

        <div>
            <div>
                <p>The maximum quantity you can order is {{ product.maxQuantity }}</p>
                <button v-on:click="decreaseQuantity(product)">-</button>
                <input type="number" v-model="quantity" v-on:change="verifyQuantity(product)">
                <button v-on:click="increaseQuantity(product)">+</button>
            </div>
            <p class="error">{{ message }}</p>
    </div>

    <button v-on:click="order(product)">Order</button>
    
    </div> <!-- End of div -->

</template>


<script>
export default {
    name: 'Product',
    // data that something else - another component - provides
    props: {
        product: Object, // name: type
        productApiUrl: String
    },
    // data that the component generates
    data() {
        return {
            // Where vue component data is 
            // Must be a function
            message: '',
            quantity: 0
        }
    }, // don't forget the commas
    methods: {
            order(product) {
                this.$emit('product-ordered', this.product.id, this.quantity)
            },
            decreaseQuantity(product) {   // include product argument
                this.message = ''

                let newQuantity = this.quantity - 1
                if ( newQuantity < product.minQuantity) {
                    // not allowed
                    this.message = 'Minimum quantity is ' + product.minQuantity

                }
                else {
                    this.quantity = newQuantity
                }
            },
            increaseQuantity(product) {
                this.message = ''
                let newQuantity = this.quantity + 1

                if ( newQuantity > product.maxQuantity) {
                    // not allowed
                    this.message = 'Maximum quantity is ' + product.maxQuantity
                }
                else {
                    this.quantity = newQuantity
                }
                console.log(product, newQuantity)
            },
            verifyQuantity(product) {
                // called when
                this.message = ''

                if (product.quantity < product.minQuantity ) {
                    this.message = 'Minimum quantity is ' + product.minQuantity
                    this.quantity = product.minQuantity
                }
                if (product.quantity > product.maxQuantity) {
                    this.message = 'Maximum quantity is ' + product.maxQuantity
                    this.quantity = product.maxQuantity
                }
            }
        }
}
</script>


<style scoped>
    img {
        height: 300px;
    }
    .error {
        color: red;
    }
</style>