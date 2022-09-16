<template>
  <div>
    <!-- Header Section Begin -->
    <header class="header-section">
      <div class="header-top">
        <div class="container">
          <div class="ht-left">
            <div class="mail-service">
              <i class=" fa fa-envelope"></i> erixfashion@gmail.com
            </div>
            <div class="phone-service">
              <i class=" fa fa-phone"></i> +628 5156848398
            </div>
          </div>
        </div>
      </div>
      <div class="container">
        <div class="inner-header">
          <div class="row">
            <div class="col-lg-2 col-md-2">
              <div class="logo">
                <router-link to="/"> <a href="#">
                    <img
                      src="img/logo_website_shayna.png"
                      alt=""
                    />
                  </a></router-link>
              </div>
            </div>
            <div class="col-lg-7 col-md-7"></div>
            <div class="col-lg-3 text-right col-md-3">
              <ul class="nav-right">
                <li class="cart-icon">
                  Keranjang Belanja &nbsp;
                  <a href="#">
                    <i class="icon_bag_alt"></i>
                    <span>{{chartUser.length}}</span>
                  </a>
                  <div class="cart-hover">
                    <div class="select-items">
                      <table>
                        <tbody v-if="chartUser.length > 0">
                          <tr
                            v-for="chart in chartUser"
                            :key="chart.id"
                          >
                            <td class="si-pic">
                              <img
                                class="photo-item"
                                :src="chart.photo"
                                alt=""
                              />
                            </td>
                            <td class="si-text">
                              <div class="product-selected">
                                <p>Rp {{chart.price}}</p>
                                <h6>{{chart.name}}</h6>
                              </div>
                            </td>
                            <td
                              @click="removeItem(chart.id)"
                              class="si-close"
                            >
                              <i class="ti-close"></i>
                            </td>
                          </tr>
                        </tbody>
                        <tbody v-else>
                          <tr>
                            <td>Keranjang kosong</td>
                          </tr>
                        </tbody>
                      </table>
                    </div>
                    <div class="select-total">
                      <span>total:</span>
                      <h5>Rp {{ totalPrice }}</h5>
                    </div>
                    <div class="select-button">
                      <router-link to="/shopping-chart"><a
                          href="#"
                          class="primary-btn view-card"
                        >VIEW CARD</a></router-link>
                      <a
                        href="#"
                        class="primary-btn checkout-btn"
                      >CHECK OUT</a>
                    </div>
                  </div>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </header>
    <!-- Header End -->
  </div>
</template>

<script>
export default {
  name: "HeaderErixCloth",
  data() {
    return {
      chartUser: [],
    };
  },
  methods: {
    removeItem(id) {
      let chartUserStorage = JSON.parse(localStorage.getItem("chartUser"));
      let itemChartUserStorage = chartUserStorage.map(
        (itemChartUserStorage) => itemChartUserStorage.id
      );
      let index = itemChartUserStorage.indexOf(id);
      this.chartUser.splice(index, 1);

      const parsed = JSON.stringify(this.chartUser);
      localStorage.setItem("chartUser", parsed);
      window.location.reload();
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
  },
  computed: {
    totalPrice() {
      return this.chartUser.reduce((total, item) => {
        return total + parseInt(item.price);
      }, 0);
    },
  },
};
</script>
<style scoped>
.photo-item {
  width: 80px;
  height: 80px;
}
</style>
