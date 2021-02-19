<template>
  <div class="flex-col ">
    <template v-if="asset.id">
      <div class="d-flex justify-content-around items-center mt-4 pt-4 margin">
        <div class="flex flex-col items-center">
          <img
            class=" mr-5"
            width="100px"
            :src="
              `https://static.coincap.io/assets/icons/${asset.symbol.toLowerCase()}@2x.png`
            "
          />
          <h2 class="text-5xl">
            {{ asset.name }}
            <small class="sm:mr-2 text-gray-500">{{ asset.symbol }}</small>
          </h2>
        </div>

        <div class="my-10 flex flex-col">
          <ul class="list-unstyled">
            <li class="flex justify-between">
              <b class="text-secondary mr-4 text-uppercase">Ranking</b>
              <span># {{ asset.rank }}</span>
            </li>
            <li class="flex justify-between">
              <b class="text-secondary mr-4 text-uppercase">Precio actual</b>
              <span>{{ parseFloat(asset.priceUsd).toFixed(2) }}</span>
            </li>
            <li class="flex justify-between">
              <b class="text-secondary mr-4 text-uppercase">Precio más bajo</b>
              <span>{{ min }}</span>
            </li>
            <li class="flex justify-between">
              <b class="text-secondary mr-4 text-uppercase">Precio más alto</b>
              <span>{{ max }}</span>
            </li>
            <li class="flex justify-between">
              <b class="text-secondary mr-4 text-uppercase">Precio Promedio</b>
              <span>{{ parseFloat(avg).toFixed(2) }}</span>
            </li>
            <li class="flex justify-between">
              <b class="text-secondary mr-4 text-uppercase">Variación 24hs</b>
              <span>{{ parseFloat(asset.changePercent24Hr).toFixed(2) }}</span>
            </li>
          </ul>
        </div>

        <div class="my-10 flex flex-col justify-center text-center">
          <button
            @click="toggleConverter"
            class="btn btn-success  text-white font-bold py-2 px-4 rounded "
          >
            {{ fromUsd ? `USD a ${asset.symbol}` : `${asset.symbol} a USD` }}
          </button>

          <div class="flex flex-row my-5">
            <label class="w-full" for="convertValue">
              <input
                v-model="convertValue"
                id="convertValue"
                type="number"
                class="text-center bg-white focus:outline-none focus:shadow-outline border border-gray-300 rounded-lg py-2 px-4 block w-full appearance-none leading-normal"
                :placeholder="`Valor en ${fromUsd ? 'USD' : asset.symbol}`"
              />
            </label>
          </div>

          <span class="text-xl font-weight-bold">
            {{ convertResult }} {{ fromUsd ? asset.symbol : "USD" }}
          </span>
        </div>
      </div>
     
    </template>
  </div>
</template>

<script>

import api from "@/api";
export default {
  name: "CoinDetail",
  data() {
    return {
      asset: {},
      history: [],
      fromUsd: true,
      convertValue: null
    };
  },
  computed: {
    convertResult() {
      if (!this.convertValue) {
        return 0;
      }

      const result = this.fromUsd
        ? this.convertValue / this.asset.priceUsd
        : this.convertValue * this.asset.priceUsd;

      return result.toFixed(4);
    },
    // history.map(h => [h.date, parseFloat(h.priceUsd).toFixed(2)])
    min() {
      return Math.min(
        ...this.history.map(h => parseFloat(h.priceUsd).toFixed(2))
      );
    },
    max() {
      return Math.max(
        ...this.history.map(h => parseFloat(h.priceUsd).toFixed(2))
      );
    },
    avg() {
      return (
        this.history.reduce((a, b) => a + parseFloat(b.priceUsd), 0) /
        this.history.length
      );
    }
  },
  created() {
    this.getCoin();
  },
  watch: {
    $route() {
      this.getCoin();
    }
  },
  methods: {
    toggleConverter() {
      this.fromUsd = !this.fromUsd;
    },
    getCoin() {
      const { id } = this.$route.params;

      Promise.all([api.getAsset(id), api.getAssetHistory(id)]).then(
        ([asset, history]) => {
          this.asset = asset;
          this.history = history;
        }
      );
    }
  }
};
</script>
<style scoped>
.margin{
  margin-top: 5em !important;
}
</style>