<template>
  <div class="home">
      <Header />
      <Carousel />
      <WomenBanner />
      <Instagram />
      <Logo />
      <Footer />

  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
 import Header from '@/components/Header.vue'
 import Carousel from '@/components/Carousel.vue'
 import WomenBanner from '@/components/WomenBanner.vue'
 import Instagram from '@/components/Instagram.vue'
 import Logo from '@/components/Logo.vue'
 import Footer from '@/components/Footer.vue'

export default {
  name: 'Home',
  components: {
     Header,
     Carousel,
     WomenBanner,
     Instagram,
     Logo,
     Footer 
  }
}
</script>
