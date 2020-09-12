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
                <div class="product-thumbs">
                  <carousel
                    class="product-thumbs-track ps-slider"
                    :margin="12"
                    :nav="false"
                    :dots="false"
                  >
                    <div
                      v-for="thumb in thumbs"
                      :key="thumb"
                      class="pt"
                      v-bind:class="{ active: default_picture == thumb }"
                      @click="changeImage(thumb)"
                    >
                      <img :src="thumb" alt="" />
                    </div>
                  </carousel>
                </div>
              </div>
              <div class="col-lg-6">
                <div class="product-details">
                  <div class="pd-title">
                    <span>oranges</span>
                    <h3>Pure Pineapple</h3>
                  </div>
                  <div class="pd-desc">
                    <p>
                      Lorem ipsum dolor sit amet consectetur adipisicing elit.
                      Corporis, error officia. Rem aperiam laborum voluptatum
                      vel, pariatur modi hic provident eum iure natus quos non a
                      sequi, id accusantium! Autem.
                    </p>
                    <p>
                      Lorem ipsum dolor sit, amet consectetur adipisicing elit.
                      Quam possimus quisquam animi, commodi, nihil voluptate
                      nostrum neque architecto illo officiis doloremque et
                      corrupti cupiditate voluptatibus error illum. Commodi
                      expedita animi nulla aspernatur. Id asperiores blanditiis,
                      omnis repudiandae iste inventore cum, quam sint molestiae
                      accusamus voluptates ex tempora illum sit perspiciatis.
                      Nostrum dolor tenetur amet, illo natus magni veniam quia
                      sit nihil dolores. Commodi ratione distinctio harum
                      voluptatum velit facilis voluptas animi non laudantium, id
                      dolorem atque perferendis enim ducimus? A exercitationem
                      recusandae aliquam quod. Itaque inventore obcaecati, unde
                      quam impedit praesentium veritatis quis beatae ea atque
                      perferendis voluptates velit architecto?
                    </p>
                    <h4>$495.00</h4>
                  </div>
                  <div class="quantity">
                    <router-link to="/cart" class="primary-btn pd-cart"
                      >Add To Cart</router-link
                    >
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
      default_picture: "img/mickey1.jpg",
      isActive: true,
      thumbs: [
        "img/mickey1.jpg",
        "img/mickey2.jpg",
        "img/mickey3.jpg",
        "img/mickey4.jpg",
      ],
    };
  },
  methods: {
    changeImage(imageUrl) {
      this.default_picture = imageUrl;
    },
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
