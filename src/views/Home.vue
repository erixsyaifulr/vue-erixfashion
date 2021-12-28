<template>
  <div class="home">
    <HeaderErixCloth />
    <SlideCarousel />
    <ProductSlider />
    <InstaErixFashion />
    <PartnerLogo />
    <Footer />
  </div>
</template>

<script>
// @ is an alias to /src
import HeaderErixCloth from "@/components/HeaderErixCloth.vue";
import SlideCarousel from "@/components/SlideCarousel.vue";
import ProductSlider from "@/components/ProductSlider.vue";
import InstaErixFashion from "@/components/InstaErixFashion.vue";
import PartnerLogo from "@/components/PartnerLogo.vue";
import Footer from "@/components/Footer.vue";

export default {
  name: "Home",
  components: {
    HeaderErixCloth,
    SlideCarousel,
    ProductSlider,
    InstaErixFashion,
    PartnerLogo,
    Footer,
  },
};
</script>
