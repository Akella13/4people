<template>
  <ul>
    <li v-for="group in groupedGoods" :key="group.id">
      <h3>{{ GroupName(group.id) }}</h3>
      <ul>
        <li v-for="(good, index) in group.goods" :key="index">
          {{ GoodName(group.id, good.id) }}
          ({{ good.inStock }})
          <b :class="currencyRaised">
            {{ PriceRub(good.priceUSD) }}
          </b>
          <button @click="Buy(good, group.id)">Buy</button>
        </li>
      </ul>
    </li>
  </ul>
</template>

<script>
import names from '/task/names.json';

export default {
  props: {
    currency: Number,
  },
  data() {
    return {
      goods: [],
      names,
      currencyRaised: null,
    };
  },
  computed: {
    groupedGoods() {
      return this.goods.reduce((acc, curVal) => {
        const { groupId, ...good } = curVal;
        // find existing group
        const group = acc.find(({ id }) => id === groupId);
        // add good to this group
        if (group) {
          group.goods.push(good);
        // else create new group with this good
        } else {
          acc.push({
            id: groupId,
            goods: [ good ],
          })
        }
        return acc;
      }, [])
    },
  },
  watch: {
    currency(next, prev) {
      const diff = next - prev;
      if (diff > 0) {
        this.currencyRaised = 'price--up';
      } else if (diff < 0) {
        this.currencyRaised = 'price--down';
      } else {
        this.currencyRaised = null;
      }
    },
  },
  mounted() {
    this.ImportGoods();
    setInterval(this.ImportGoods, 15000);
  },
  methods: {
    GroupName(groupId) {
      return this.names[groupId].G;
    },
    GoodName(groupId, goodId) {
      return this.names[groupId].B[goodId].N;
    },
    PriceRub(usd) {
      return Math.round(this.currency * usd);
    },
    Buy(good, groupId) {
      this.$emit('buy', {
        groupId,
        ...good,  
      });
    },
    ImportGoods() {
      console.log('import')
      import('/task/data.json').then(a => {
        this.goods = a.Value.Goods.map(el => {
          return {
            priceUSD: el.C,
            groupId: el.G,
            id: el.T,
            inStock: el.P,
          };
        });
      });
    }
  },
}
</script>