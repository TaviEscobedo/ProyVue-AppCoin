<template>
  <main>
    <px-header :links="links" />
    <router-view class="container py-20"></router-view>
  </main>
</template>

<script>
import PxHeader from "@/components/PxHeader";

export default {
  name: "App",
  components: {
    PxHeader
  },
  data() {
    return {
      links: [
        {
          title: "BTC",
          to: { name: "coin-detail", params: { id: "bitcoin" } }
        },
        {
          title: "ETH",
          to: { name: "coin-detail", params: { id: "ethereum" } }
        },
        {
          title: "DOT",
          to: { name: "coin-detail", params: { id: "polkadot" } }
        }
      ]
    };
  }
};
</script>

<style></style>
