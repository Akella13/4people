<template>
  <section>
    <ul>
      <li v-for="group in groupedGoods" :key="group.id">
        <h3>GroupId: {{ group.id }}</h3>
        <ul>
          <li v-for="(good, index) in group.goods" :key="index">
            {{ good }}
          </li>
        </ul>
      </li>
    </ul>
  </section>
</template>

<script>
import goods from '/task/data.json';

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
}
</script>