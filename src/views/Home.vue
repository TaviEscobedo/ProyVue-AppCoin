<template>
  <div>
    <px-assets-table :assets="valores" />
  </div>
</template>
<script>
import api from "@/api";
import PxAssetsTable from "@/components/PxAssetsTable";

export default {
  name: "Home",
  components: { PxAssetsTable },

  data() {
    return {
      valores: []
    };
  },
  created() {
    api.getAssets().then(assets => (this.valores = assets));
    console.log(this.valores);
  }
};
</script>
