<template>
  <div class="shoppingchart">
    <HeaderErixCloth />

    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section text-left">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="breadcrumb-text product-more">
              <router-link to="/"><i class="fa fa-home"></i> Home</router-link>
              <span>Shopping Cart</span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Shopping Cart Section Begin -->
    <section class="shopping-cart spad">
      <div class="container">
        <div class="row">
          <div class="col-lg-8">
            <div class="row">
              <div class="col-lg-12">
                <div class="cart-table">
                  <table>
                    <thead>
                      <tr>
                        <th>Image</th>
                        <th class="p-name text-center">Product Name</th>
                        <th>Price</th>
                        <th>Action</th>
                      </tr>
                    </thead>
                    <tbody v-if="chartUser.length > 0">
                      <tr
                        v-for="chart in chartUser"
                        :key="chart.id"
                      >
                        <td class="cart-pic first-row">
                          <img :src="chart.photo" />
                        </td>
                        <td class="cart-title first-row text-center">
                          <h5>{{chart.name}}</h5>
                        </td>
                        <td class="p-price first-row">Rp {{chart.price}}</td>
                        <td
                          @click="removeItem(chart.id)"
                          class="delete-item"
                        ><a href="#"><i class="material-icons">
                              close
                            </i></a></td>
                      </tr>
                    </tbody>
                    <tbody v-else>
                      <tr>
                        <td>Keranjang kosong</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
              <div class="col-lg-8">
                <h4 class="mb-4">
                  Informasi Pembeli:
                </h4>
                <div class="user-checkout text-left">
                  <form>
                    <div class="form-group">
                      <label for="name">Nama lengkap</label>
                      <input
                        type="text"
                        class="form-control"
                        id="name"
                        aria-describedby="namaHelp"
                        placeholder="Masukan Nama"
                        v-model="customerInfo.name"
                      >
                    </div>
                    <div class="form-group">
                      <label for="email">Email Address</label>
                      <input
                        type="email"
                        class="form-control"
                        id="email"
                        aria-describedby="emailHelp"
                        placeholder="Masukan Email"
                        v-model="customerInfo.email"
                      >
                    </div>
                    <div class="form-group">
                      <label for="number">No. HP</label>
                      <input
                        type="text"
                        class="form-control"
                        id="number"
                        aria-describedby="noHPHelp"
                        placeholder="Masukan No. HP"
                        v-model="customerInfo.number"
                      >
                    </div>
                    <div class="form-group">
                      <label for="address">Alamat Lengkap</label>
                      <textarea
                        class="form-control"
                        id="address"
                        rows="3"
                        v-model="customerInfo.address"
                      ></textarea>
                    </div>
                  </form>
                </div>
              </div>
            </div>
          </div>
          <div class="col-lg-4">
            <div class="row">
              <div class="col-lg-12">
                <div class="proceed-checkout text-left">
                  <ul>
                    <li class="subtotal">ID Transaction <span>#SH12000</span></li>
                    <li class="subtotal mt-3">Subtotal <span>Rp {{totalPrice}}</span></li>
                    <li class="subtotal mt-3">Pajak <span>11%</span></li>
                    <li class="subtotal mt-3">Total Biaya <span>Rp {{totalPrice - totalPriceTax}}</span></li>
                    <li class="subtotal mt-3">Bank Transfer <span>Mandiri</span></li>
                    <li class="subtotal mt-3">No. Rekening <span>2208 1996 1403</span></li>
                    <li class="subtotal mt-3">Nama Penerima <span>erixfashion</span></li>
                  </ul>
                  <a
                    @click="checkout()"
                    class="proceed-btn"
                    href="#"
                  >I ALREADY PAID</a>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- Shopping Cart Section End -->

  </div>
</template>

<script>
import HeaderErixCloth from "@/components/HeaderErixCloth.vue";
import axios from "axios";

export default {
  name: "ShoppingChart",
  components: {
    HeaderErixCloth,
  },
  data() {
    return {
      chartUser: [],
      customerInfo: {
        name: "",
        email: "",
        number: "",
        address: "",
      },
    };
  },
  methods: {
    removeItem(id) {
      const items = JSON.parse(localStorage.getItem("chartUser"));
      const filtered = items.filter((item) => item.id !== id);
      localStorage.setItem("chartUser", JSON.stringify(filtered));
    },
    checkout() {
      let productId = this.chartUser.map((item) => item.id);

      let checkoutData = {
        email: this.customerInfo.email,
        name: this.customerInfo.name,
        number: this.customerInfo.number,
        address: this.customerInfo.address,
        transaction_total: this.totalPrice - this.totalPriceTax,
        transaction_status: "PENDING",
        transaction_details: productId,
      };

      axios
        .post("https://erixfashion-api.pebatha.com/api/checkout", checkoutData)
        .then(() => this.$router.push("/success"))
        .catch((err) => console.log(err));
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
      let total = 0;
      this.chartUser.forEach((item) => {
        total += parseInt(item.price);
      });
      return total;
    },
    totalPriceTax() {
      let total = 0;
      this.chartUser.forEach((item) => {
        total += parseInt(item.price);
      });
      return total * 0.11;
    },
  },
};
</script>