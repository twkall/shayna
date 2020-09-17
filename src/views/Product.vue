<template>
  <div class="product">
    <ShaynaHeader />
    <!-- Breadcrumb -->
    <div class="breadcrumb-section">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="breadcrumb-text product-more">
              <a href="./home.html"><i class="fa fa-home"></i> Home</a>
              <span>Detail</span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Product Shop Section -->
    <section class="product-shop spad page-details">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="row">
              <div class="col-lg-6">
                <div class="product-pic-zoom">
                  <img class="product-big-img" :src="default_picture" alt="" />
                </div>
                <div class="product-thumbs" v-if="pictureLength > 0">
                  <carousel
                    class="product-thumbs-track ps-slider"
                    :margin="12"
                    :nav="false"
                    :dots="false"
                  >
                    <div
                      v-for="thumb in productDetails.galleries"
                      :key="thumb.id"
                      class="pt"
                      :class="default_picture == thumb.photo ? 'active' : ''"
                      @click="changeImage(thumb.photo)"
                    >
                      <img :src="thumb.photo" alt="" />
                    </div>
                  </carousel>
                </div>
              </div>
              <div class="col-lg-6">
                <div class="product-details">
                  <div class="pd-title">
                    <span>{{ productDetails.type }}</span>
                    <h3>{{ productDetails.product_name }}</h3>
                  </div>
                  <div class="pd-desc">
                    <p v-html="productDetails.description" />
                    <h4>{{ productDetails.price }}</h4>
                  </div>
                  <div class="quantity">
                    <div class="col-6 pl-1">
                      <a
                        @click="
                          saveKeranjang(
                            productDetails.id,
                            productDetails.product_name,
                            productDetails.galleries[0].photo,
                            productDetails.price
                          )
                        "
                        href="#"
                        class="primary-btn pd-cart"
                        style="background: #252525"
                        >Add To Cart</a
                      >
                    </div>
                    <div class="col-6 pl-1">
                      <router-link to="/cart" class="primary-btn pd-cart"
                        >View Cart
                      </router-link>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <RelatedProducts />
    <ShaynaFooter />
  </div>
</template>

<script>
import carousel from "vue-owl-carousel";
import ShaynaHeader from "@/components/ShaynaHeader.vue";
import ShaynaFooter from "@/components/ShaynaFooter.vue";
import RelatedProducts from "@/components/product/RelatedProducts.vue";

import Axios from "axios";

export default {
  name: "Product",
  components: {
    carousel,
    ShaynaHeader,
    RelatedProducts,
    ShaynaFooter,
  },
  data() {
    return {
      default_picture: "",
      pictureLength: 0,
      productDetails: [],
      keranjangUser: [],
    };
  },
  methods: {
    changeImage(imageUrl) {
      this.default_picture = imageUrl;
    },
    setDefaultPicture(data) {
      this.productDetails = data;
      this.default_picture = data.galleries[0].photo;
      this.pictureLength = Object.keys(data.galleries).length;
    },
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

    Axios.get("http://localhost:8000/api/products", {
      params: { id: this.$route.params.id },
    })
      .then((res) => this.setDefaultPicture(res.data.data))
      // eslint-disable-next-line no-console
      .catch((err) => console.log(err));
  },
};
</script>

<style>
.product-pic-zoom img {
  object-fit: cover;
}

.product-thumbs .pt img {
  object-fit: cover;
}
</style>
