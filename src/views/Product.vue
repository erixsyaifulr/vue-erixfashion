<template>
  <div class="product">
    <HeaderErixCloth />
    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section text-left">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="breadcrumb-text product-more">
              <router-link to="/"><i class="fa fa-home"></i> Home</router-link>
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
                  <img
                    class="product-big-img"
                    :src="image_default"
                    alt=""
                  />
                </div>
                <div
                  class="product-thumbs"
                  v-if="productDetails.galleries.length>0"
                >
                  <carousel
                    class="product-thumbs-track ps-slider"
                    :nav="false"
                    :dots="false"
                  >

                    <div
                      v-for="ss in productDetails.galleries"
                      :key="ss.id"
                      class="pt"
                      @click="changeImage(ss.photo)"
                      :class="ss.photo == image_default ? 'active' : ''"
                    >
                      <img
                        :src="ss.photo"
                        alt=""
                      />
                    </div>

                  </carousel>
                </div>
              </div>
              <div class="col-lg-6">
                <div class="product-details text-left">
                  <div class="pd-title">
                    <span>{{productDetails.type}}</span>
                    <h3>{{productDetails.name}}</h3>
                  </div>
                  <div class="pd-desc">
                    <p></p>
                    <p v-html="productDetails.description"></p>
                    <h4>${{productDetails.price}}</h4>
                  </div>
                  <div class="quantity">
                    <router-link to="/shopping-chart">
                      <a
                        @click="addToChart(productDetails.id, productDetails.name, productDetails.price, productDetails.galleries[0].photo)"
                        href="#"
                        class="primary-btn pd-cart"
                      >Add To Cart</a>
                    </router-link>
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
import HeaderErixCloth from "@/components/HeaderErixCloth.vue";
import Footer from "@/components/Footer.vue";
import carousel from "vue-owl-carousel";
import RelatedProduct from "@/components/RelatedProduct.vue";
import axios from "axios";

export default {
  name: "product",
  components: {
    HeaderErixCloth,
    Footer,
    carousel,
    RelatedProduct,
  },
  data() {
    return {
      image_default: "",
      productDetails: [],
      chartUser: [],
    };
  },
  methods: {
    changeImage(urlImage) {
      this.image_default = urlImage;
    },
    setDataPicture(data) {
      this.productDetails = data;
      this.image_default = data.galleries[0].photo;
    },
    addToChart(idProduct, productName, price, photo) {
      var productStored = {
        id: idProduct,
        name: productName,
        price: price,
        photo: photo,
      };

      this.chartUser.push(productStored);
      const parse = JSON.stringify(this.chartUser);
      localStorage.setItem("chartUser", parse);
    },
  },
  mounted() {
    if (localStorage.getItem("chartUser")) {
      try {
        this.chartUser = JSON.parse(localStorage.getItem("chartUser"));
      } catch (error) {
        localStorage.removeItem("chartUser");
      }
    }
    axios
      .get("https://erixfashion-api.pebatha.com/api/products", {
        params: {
          id: this.$route.params.id,
        },
      })
      .then((res) => {
        this.setDataPicture(res.data.data);
      })
      .catch((err) => {
        console.log(err);
      });
  },
};
</script>

<style scoped>
.product-thumbs .pt {
  margin-right: 20px;
}
</style>
