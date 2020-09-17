<template>
  <section class="women-banner spad">
    <div class="container-fluid">
      <div class="row">
        <div class="col-lg-12 mt-5" v-if="products.length > 0">
          <carousel class="product-slider" :nav="false" :dots="false">
            <div
              class="product-item"
              v-for="itemProduct in products"
              v-bind:key="itemProduct.id"
            >
              <div class="pi-pic">
                <img :src="itemProduct.galleries[0].photo" alt="" />
                <ul>
                  <li
                    class="w-icon active"
                    @click="
                      saveKeranjang(
                        itemProduct.id,
                        itemProduct.product_name,
                        itemProduct.galleries[0].photo,
                        itemProduct.price
                      )
                    "
                  >
                    <a href="#"><i class="icon_bag_alt"></i></a>
                  </li>
                  <li class="quick-view">
                    <router-link :to="'/product/' + itemProduct.id"
                      >+ Quick View
                    </router-link>
                  </li>
                </ul>
              </div>
              <div class="pi-text">
                <div class="catagory-name">{{ itemProduct.type }}</div>
                <router-link to="/product">
                  <h5>{{ itemProduct.product_name }}</h5>
                </router-link>
                <div class="product-price">
                  {{ itemProduct.price }}
                  <span>20</span>
                </div>
              </div>
            </div>
          </carousel>
        </div>

        <div class="col-lg-12 mt05" v-else>
          <p>No Products</p>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import carousel from "vue-owl-carousel";
import Axios from "axios";

export default {
  name: "ShaynaBanner",
  components: {
    carousel,
  },
  data() {
    return {
      products: [],
      keranjangUser: [],
    };
  },
  methods: {
    saveKeranjang(productId, productName, productPhoto, productPrice) {
      var productStored = {
        id: productId,
        product_name: productName,
        photo: productPhoto,
        price: productPrice,
      };

      this.keranjangUser.push(productStored);
      const parsed = JSON.stringify(this.keranjangUser);
      localStorage.setItem("keranjangUser", parsed);
      window.location.reload();
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

    Axios.get("http://localhost:8000/api/products")
      .then((res) => (this.products = res.data.data.data))
      // eslint-disable-next-line no-console
      .catch((err) => console.log(err));
  },
};
</script>

<style></style>
