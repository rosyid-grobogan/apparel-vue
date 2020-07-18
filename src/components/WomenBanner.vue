<template>
        <!-- Women Banner Section Begin -->
    <section class="women-banner spad">
        <div class="container-fluid">
            <div class="row">
                <div class="col-lg-12 mt-5" v-if="products.length > 0">

                    <carousel class="product-slider" :nav="false" :autoplay="false" :items="3" :dots="false">
                        
                        <div class="product-item" v-for="itemProduct in products" :key="itemProduct.id">
                            <div class="pi-pic">
                                <img :src="itemProduct.galleries[0].photo" alt="" />
                                <ul>
                                    <li class="w-icon active">
                                        <button @click="saveCarts(
                                        itemProduct.id, itemProduct.name,
                                        itemProduct.price, itemProduct.galleries[0].photo
                                        )" href="#"><i class="icon_bag_alt"></i>
                                        </button>
                                    </li>
                                    <li class="quick-view">
                                        <router-link :to="'/product/'+itemProduct.id">
                                        + Quick View
                                        </router-link>
                                    </li>
                                </ul>
                            </div>
                            <div class="pi-text">
                                <div class="catagory-name">{{ itemProduct.type }}</div>
                                <router-link to="/product">
                                    <h5>{{ itemProduct.name }}</h5>
                                </router-link>
                                <div class="product-price">
                                    {{ itemProduct.price }}
                                    <span>$35.00</span>
                                </div>
                            </div>
                        </div>



                    </carousel>

                </div>

                <div class="col-lg-12" v-else>
                    <p>Produk terbaru belum tersedia untuk saat ini.</p>
                </div>
            </div>
        </div>
    </section>
    <!-- Women Banner Section End -->
</template>

<script>
import carousel from 'vue-owl-carousel'
import axios from 'axios'

export default {
    name: 'WomenBanner',
    components: {
     carousel 
  },
  data() {
      return {
          products: [],
          carts: []
      }
  },
  methods: {
    saveCarts(idProduct, nameProduct, priceProduct, photoProduct) {

        var productStored = {
            'id': idProduct,
            'name': nameProduct,
            'price': priceProduct,
            'photo': photoProduct
        }

        this.carts.push(productStored);
        const parsed = JSON.stringify(this.carts);
        localStorage.setItem('carts', parsed);
    }
  },
  mounted() {
      // validasi localStorage
      if (localStorage.getItem('carts')) {
          try {
              this.cart = JSON.parse(localStorage.getItem('carts'));
          } catch(e) {
              localStorage.removeItem('carts');
          }
      }      
      axios
        .get("http://shayna-backend.belajarkoding.com/api/products")
        .then( result => (this.products = result.data.data.data) )
        .catch( err => console.log(err) )
  } 
}
</script>

<style scoped>
.product-item {
    margin-right: 25px;
}
.product-item .pi-pic ul li.w-icon.active button {
    background: #e7ab3c;
    color: #ffffff;
}
</style>