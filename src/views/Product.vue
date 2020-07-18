<template>
  <div class="product">
      <Header />

    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section">
        <div class="container">
            <div class="row">
                <div class="col-lg-12 text-left">
                    <div class="breadcrumb-text product-more">
                        <router-link to="/">
                        <a href="#"><i class="fa fa-home"></i> Home</a>
                        </router-link>
                        <span>Detail</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="row">
                        <div class="col-lg-6">
                            <div class="product-pic-zoom">
                                <img class="product-big-img" :src="product_image" alt="" />
                            </div>
                            <div class="product-thumbs" v-if="productDetails.galleries.length > 0">

                                <carousel class="product-thumbs-track ps-slider" :nav="false" :dots="false">
                                    <div 
                                    v-for="imgpd in productDetails.galleries"
                                    :key="imgpd.id"
                                    class="pt active" @click="changeImage(imgpd.photo)" :class="imgpd.photo == product_image ? 'active' : ''">
                                        <img :src="imgpd.photo" alt="" />
                                    </div>

                                </carousel>

                            </div>
                        </div>
                        <div class="col-lg-6">
                            <div class="product-details text-left">
                                <div class="pd-title">
                                    <span>{{ productDetails.type }}</span>
                                    <h3>{{ productDetails.name }}</h3>
                                </div>
                                <div class="pd-desc">
                                    <div v-html="productDetails.description ">
                                    
                                    </div>

                                    <h4>{{ productDetails.price }}</h4>
                                </div>
                                <div class="quantity">
                                    <!-- <router-link to="/cart" class="primary-btn pd-cart" @click="saveCart(productDetails.id, productDetails.name)">Add To Cart</router-link> -->
                                    <a @click="saveCarts(
                                        productDetails.id, productDetails.name,
                                        productDetails.price, productDetails.galleries[0].photo
                                        )" href="#" class="primary-btn pd-cart">Add To Cart</a>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
    <!-- Product Shop Section End -->

      <RelatedProduct />
      <Footer />

  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
 import Header from '@/components/Header.vue'
 import carousel from 'vue-owl-carousel'
 import RelatedProduct from '@/components/RelatedProduct.vue'
 import Footer from '@/components/Footer.vue'
 import axios from 'axios'

export default {
  name: 'product',
  components: {
     Header,
     carousel,
     RelatedProduct,
     Footer 
  },
  data() {
      return {
          product_image:'',
        //   thumbs: [
        //       "img/mickey1.jpg",
        //       "img/mickey2.jpg",
        //       "img/mickey3.jpg",
        //       "img/mickey4.jpg"
        //   ],
          //idProduct: this.$route.params.id
          productDetails: [],
          carts: []
      }
  },
  methods: {
    changeImage(urlImage) {
          this.product_image = urlImage;
         // console.log(this.idProduct);
      },
    // function setDataPicture  
    setDataPicture(data) {
        // replace object productDetails dengan data dari API
        this.productDetails = data;

        // replace value gambar default dengan data dari API (galleries)
        this.product_image = data.galleries[0].photo;
      
    },
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
        .get("http://shayna-backend.belajarkoding.com/api/products", {
            params: {
                id: this.$route.params.id
            }
        })
        .then( result => ( this.setDataPicture(result.data.data) ) )
        .catch( err => console.log(err) )
  }  
}
</script>

<style scoped>
.product-thumbs .pt {
    margin-right: 10px;
}
</style>