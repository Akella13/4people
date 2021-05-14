<template>
  <section>
    <ul>
      <li v-for="group in groupedGoods" :key="group.id">
        <h3>{{ GroupName(group.id) }}</h3>
        <ul>
          <li v-for="(good, index) in group.goods" :key="index">
            {{ GoodName(group.id, good.id) }}
            ({{ good.inStock }})
            <b>{{ PriceRub(good.priceUSD) }}</b>
            <button @click="Buy(good, group.id)">Buy</button>
          </li>
        </ul>
      </li>
    </ul>
  </section>
</template>

<script>
import goods from '/task/data.json';
import names from '/task/names.json';

export default {
  data() {
    return {
      goods: goods.Value.Goods.map(el => {
        return {
          priceUSD: el.C,
          groupId: el.G,
          id: el.T,
          inStock: el.P,
        };
      }),
      names,
      currency: 74.3,
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
  },
}
</script>