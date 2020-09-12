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
import ShaynaHero from "@/components/homepage/ShaynaHero.vue";
import ShaynaBanner from "@/components/homepage/ShaynaBanner.vue";
import ShaynaInsta from "@/components/homepage/ShaynaInsta.vue";
import ShaynaPartnerLogo from "@/components/homepage/ShaynaPartnerLogo.vue";

import ShaynaHeader from "@/components/ShaynaHeader.vue";
import ShaynaFooter from "@/components/ShaynaFooter.vue";

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
