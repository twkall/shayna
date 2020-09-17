<template>
  <div>
    <ShaynaHeader />
    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="breadcrumb-text product-more">
              <a href="/"><i class="fa fa-home"></i> Home</a>
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
                  <table v-if="keranjangUser.length > 0">
                    <thead>
                      <tr>
                        <th>Image</th>
                        <th class="p-name text-center">Product Name</th>
                        <th>Price</th>
                        <th>Action</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr
                        v-for="keranjang in keranjangUser"
                        :key="keranjang.id"
                      >
                        <td class="cart-pic first-row">
                          <img :src="keranjang.photo" />
                        </td>
                        <td class="cart-title first-row text-center">
                          <h5>{{ keranjang.product_name }}</h5>
                        </td>
                        <td class="p-price first-row">{{ keranjang.price }}</td>
                        <td
                          class="delete-item"
                          @click="removeItem(keranjang.id)"
                        >
                          <a href="#"
                            ><i class="material-icons">
                              close
                            </i></a
                          >
                        </td>
                      </tr>
                    </tbody>
                  </table>
                  <table v-else>
                    <tr>
                      <td>Keranjang Masih Kosong</td>
                    </tr>
                  </table>
                </div>
              </div>
              <div class="col-lg-8" v-if="keranjangUser.length > 0">
                <h4 class="mb-4">
                  Informasi Pembeli:
                </h4>
                <div class="user-checkout">
                  <form>
                    <div class="form-group">
                      <label for="namaLengkap">Nama lengkap</label>
                      <input
                        type="text"
                        class="form-control"
                        id="namaLengkap"
                        aria-describedby="namaHelp"
                        placeholder="Masukan Nama"
                        v-model="transactionInfo.customer_name"
                      />
                    </div>
                    <div class="form-group">
                      <label for="namaLengkap">Email Address</label>
                      <input
                        type="email"
                        class="form-control"
                        id="emailAddress"
                        aria-describedby="emailHelp"
                        placeholder="Masukan Email"
                        v-model="transactionInfo.email"
                      />
                    </div>
                    <div class="form-group">
                      <label for="namaLengkap">No. HP</label>
                      <input
                        type="text"
                        class="form-control"
                        id="noHP"
                        aria-describedby="noHPHelp"
                        placeholder="Masukan No. HP"
                        v-model="transactionInfo.number"
                      />
                    </div>
                    <div class="form-group">
                      <label for="alamatLengkap">Alamat Lengkap</label>
                      <textarea
                        class="form-control"
                        id="alamatLengkap"
                        rows="3"
                        placeholder="Masukkan Alamat Lengkap"
                        v-model="transactionInfo.address"
                      ></textarea>
                    </div>
                  </form>
                </div>
              </div>
              <div v-else></div>
            </div>
          </div>
          <div class="col-lg-4">
            <div class="row">
              <div class="col-lg-12">
                <div class="proceed-checkout">
                  <ul>
                    <li class="subtotal">
                      ID Transaction <span>#SH12000</span>
                    </li>
                    <li class="subtotal mt-3">
                      Subtotal <span>{{ totalTransaction }}</span>
                    </li>
                    <li class="subtotal mt-3">
                      Pajak
                      <span>10%</span>
                    </li>
                    <li class="subtotal mt-3">
                      Total Biaya <span>{{ totalTransactionWithTax }} </span>
                    </li>
                    <li class="subtotal mt-3">
                      Bank Transfer <span>Mandiri</span>
                    </li>
                    <li class="subtotal mt-3">
                      No. Rekening <span>2208 1996 1403</span>
                    </li>
                    <li class="subtotal mt-3">
                      Nama Penerima <span>Shayna</span>
                    </li>
                  </ul>
                  <!-- <router-link to="/success"> -->
                  <a href="#" class="proceed-btn" @click="checkout()"
                    >I ALREADY PAID</a
                  >
                  <!--  </router-link> -->
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
import ShaynaHeader from "@/components/ShaynaHeader.vue";
import Axios from "axios";

export default {
  name: "ShoppingCart",
  components: { ShaynaHeader },
  data() {
    return {
      keranjangUser: [],
      transactionInfo: {
        customer_name: "",
        email: "",
        number: "",
        address: "",
      },
    };
  },
  methods: {
    removeItem(idx) {
      let keranjangUserStorage = JSON.parse(
        localStorage.getItem("keranjangUser")
      );
      let itemKeranjangUserStorage = keranjangUserStorage.map(
        (itemKeranjangUserStorage) => itemKeranjangUserStorage.id
      );

      let index = itemKeranjangUserStorage.findIndex((id) => id == idx);
      this.keranjangUser.splice(index, 1);

      const parsed = JSON.stringify(this.keranjangUser);
      localStorage.setItem("keranjangUser", parsed);
      window.location.reload();
    },
    checkout() {
      let productIds = this.keranjangUser.map(function(product) {
        return product.id;
      });

      let checkoutData = {
        customer_name: this.transactionInfo.customer_name,
        email: this.transactionInfo.email,
        number: this.transactionInfo.number,
        address: this.transactionInfo.address,
        transaction_total: Math.trunc(this.totalTransactionWithTax),
        transaction_status: "PENDING",
        transaction_details: productIds,
      };

      Axios.post("http://localhost:8000/api/checkout", checkoutData)
        .then(() => this.$router.push("success"))
        // eslint-disable-next-line no-console
        .catch((err) => console.log(err));
    },
  },
  mounted() {
    if (localStorage.getItem("keranjangUser")) {
      try {
        this.keranjangUser = JSON.parse(localStorage.getItem("keranjangUser"));
      } catch (e) {
        localStorage.removeItem("keranjangUser");
      }
    }
  },
  computed: {
    totalTransaction() {
      return this.keranjangUser.reduce(function(items, data) {
        return items + data.price;
      }, 0);
    },
    totalTransactionWithTax() {
      return (this.totalTransaction * 110) / 100;
    },
  },
};
</script>

<style></style>
