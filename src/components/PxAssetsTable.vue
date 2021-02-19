<template>
  <table class="table mt-4">
    <thead>
      <tr>
        <th></th>
        <th scope="col">
          <span>
            Ranking
          </span>
        </th>
        <th scope="col">Nombre</th>
        <th scope="col">Precio</th>
        <th scope="col">Cap. de mercados</th>
        <th scope="col">Variación 24hs</th>
        <td>
          <input
            class="  bg-light text-dark
              py-2 px-4 block "
            id="filter"
            placeholder="Buscar..."
            type="text"
            v-model="filter"
          />
        </td>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(coin, index) in filteredAssets" :key="index">
        <td>
          <img
            :src="
              `https://static.coincap.io/assets/icons/${coin.symbol.toLowerCase()}@2x.png`
            "
            :alt="coin.name"
          />
        </td>
        <td class="text-center">
          <b
            >#
            {{ coin.rank }}
          </b>
        </td>
        <td>
          <!-- <router-link :to="`/coin/${coin.name.toLowerCase()}`" 
          style="text-decoration:none text-dark"> -->
          <router-link :to="{ name: 'coin-detail', params: { id: coin.id } }">
            {{ coin.name }}
            <small>{{ coin.symbol }}</small>
          </router-link>
        </td>
        <td>{{ coin.priceUsd }}</td>
        <td>{{ coin.marketCapUsd }}</td>
        <td
          :class="
            coin.changePercent24Hr.includes('-')
              ? 'text-danger'
              : 'text-success'
          "
        >
          {{ coin.changePercent24Hr }}
        </td>
      </tr>
    </tbody>
  </table>
</template>
<script>
export default {
  name: "PxAssetsTable",
  props: {
    assets: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      filter: ""
    };
  },
  computed: {
    filteredAssets() {
      if (!this.filter) {
        return this.assets;
      }

      return this.assets.filter(
        a =>
          a.name.toLowerCase().includes(this.filter.toLowerCase()) ||
          a.symbol.toLowerCase().includes(this.filter.toLowerCase())
      );
    }
  }
};
</script>

<style scoped>
img {
  height: 32px;
}
</style>
