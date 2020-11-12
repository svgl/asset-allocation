<template>
  <header>
    <h1 class="text-6xl text-teal-600 font-semibold tracking-wider italic">
      {{ title }}
    </h1>
  </header>
  <main>
    <div class="mt-8 container mx-auto">
      <table class="table-fixed min-w-full">
        <thead class="bg-indigo-300">
          <tr>
            <th class="p-4 w-1/4 text-left">Ticker</th>
            <th class="p-4 w-1/4 text-left">Amount</th>
            <th class="p-4 w-1/4 text-left">Target</th>
            <th class="p-4 w-1/4 text-left">Actual</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(asset, index) in assets"
            :class="{ 'bg-gray-200': index % 2 !== 0 }"
            :key="asset.ticker"
          >
            <td class="px-4 py-2 text-left uppercase">{{ asset.ticker }}</td>
            <td class="px-4 py-2 text-left">${{ asset.amount }}</td>
            <td class="px-4 py-2 text-left">
              {{ displayPercent(asset.target) }}%
            </td>
            <td class="px-4 py-2 text-left">
              {{ displayPercent(calculateActual(asset.amount)) }}%
              <span
                >({{
                  displayPercent(
                    calculateDifference(
                      asset.target,
                      calculateActual(asset.amount)
                    )
                  )
                }})</span
              >
            </td>
          </tr>
          <tr class="bg-indigo-300">
            <td class="p-2">
              <input
                class="px-3 py-2 w-full border border-gray-500 rounded"
                v-model="newAsset.ticker"
                type="text"
                ref="ticker"
                name="ticker"
              />
            </td>
            <td class="p-2">
              <input
                class="px-3 py-2 w-full border border-gray-500 rounded"
                v-model.number="newAsset.amount"
                type="text"
                name="amount"
              />
            </td>
            <td class="p-2">
              <input
                class="px-3 py-2 w-full border border-gray-500 rounded"
                v-model.number="newAsset.target"
                @keyup.enter="addAsset"
                placeholder="0.1"
                type="text"
                name="target"
              />
            </td>
            <td class="p-2 text-left">
              <button
                class="px-3 py-2 w-full font-semibold tracking-wide text-gray-100 bg-blue-500 rounded"
                v-on:click="addAsset"
                type="button"
              >
                Add
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </main>
</template>

<script>
export default {
  name: "App",
  components: {},
  data: function () {
    return {
      title: "Asset Alloc",
      assets: [],
      newAsset: {
        ticker: "",
        amount: null,
        target: null,
      },
    };
  },
  methods: {
    addAsset() {
      this.assets.push({
        ticker: this.newAsset.ticker,
        amount: this.newAsset.amount,
        target: this.newAsset.target,
      });

      this.newAsset.ticker = "";
      this.newAsset.amount = null;
      this.newAsset.target = null;

      this.$refs.ticker.focus();
    },
    calculateActual(amount) {
      let total = 0;
      for (let i = 0; i < this.assets.length; i++) {
        total += this.assets[i].amount;
      }

      return (amount / total).toPrecision(4);
    },
    displayPercent(pct) {
      return (pct * 100).toPrecision(2);
    },
    calculateDifference(target, actual) {
      return (target - actual).toPrecision(4);
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
