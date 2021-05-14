<template>
  <ul class="groups">
    <li v-for="group in groupedGoods" :key="group.id" class="group">
      <h3 class="group__title">{{ GroupName(group.id) }}</h3>
      <ul class="goods">
        <li v-for="(good, index) in group.goods" :key="index" class="good">
          <span class="good__title">
            {{ GoodName(group.id, good.id) }}
            ({{ good.inStock }})
          </span>
          <b :class="currencyRaised" class="good__price">
            {{ PriceRub(good.priceUSD) }}
          </b>
          <button class="button good__action" @click="Buy(good, group.id)">В корзину</button>
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
    },
  },
}
</script>

<style>
  .groups {
    list-style-type: none;
    padding: 0 1em;
  }

  .group {
    border: 1px solid #ccc;
    border-radius: 5px;
    margin-bottom: 1em;
  }

  .group__title {
    background-color: #d2dde1;
    border-bottom: 1px solid #ccc;
    margin: unset;
    padding-left: 1em;
  }

  .goods {
    list-style-type: none;
    padding: unset;
  }

  .good {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .good:not(:last-child) {
    border-bottom: 1px solid #ccc;
  }

  .good:hover .good__action {
    visibility: visible;
  }

  .good__title {
    margin-left: 1em;
    margin-right: auto;
  }

  .good__price {
    margin-right: 1em;
  }

  .good__action {
    margin: 0.25em;
    visibility: hidden;
    margin-right: 1em;
  }
</style>