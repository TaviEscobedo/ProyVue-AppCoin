<template>
  <header class="navbar bg-success text-white">
    <nav class="d-flex">
      <div class=" mr-4 p-3">
        <router-link
          class="text-white   "
          to="/"
          style="
                   text-decoration:none;font-size: 25px"
          >PlatziExchange</router-link
        >
      </div>
      <div class="p-3 ">
        <router-link
          class="text-white mr-4"
          v-for="l in links"
          :key="l.title"
          :to="l.to"
          style="text-decoration:none;font-size: 20px"
        >
          {{ l.title }}</router-link
        >
      </div>
    </nav>
  </header>
</template>
<script>
export default {
  name: "PxHeader",
  props: {
    links: {
      type: Array,
      default: () => []
    }
  }
};
</script>
