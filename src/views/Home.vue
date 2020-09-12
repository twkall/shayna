<template>
  <div class="home">
    <ShaynaHeader />
    <ShaynaHero />
    <ShaynaBanner />
    <ShaynaInsta />
    <ShaynaPartnerLogo />
    <ShaynaFooter />
  </div>
</template>

<script>
// @ is an alias to /src
import ShaynaHeader from "@/components/homepage/ShaynaHeader.vue";
import ShaynaHero from "@/components/homepage/ShaynaHero.vue";
import ShaynaBanner from "@/components/homepage/ShaynaBanner.vue";
import ShaynaInsta from "@/components/homepage/ShaynaInsta.vue";
import ShaynaPartnerLogo from "@/components/homepage/ShaynaPartnerLogo.vue";
import ShaynaFooter from "@/components/homepage/ShaynaFooter.vue";

export default {
  name: "Home",
  components: {
    ShaynaHeader,
    ShaynaHero,
    ShaynaBanner,
    ShaynaInsta,
    ShaynaPartnerLogo,
    ShaynaFooter,
  },
};
</script>
