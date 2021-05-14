<template>
  <div>
    <h4>Cart:</h4>
    <ul>
      <li v-for="(item, index) in items" :key="index">
        {{ GoodName(item.groupId, item.id) }}
        <input type="number" v-model.number="item.quantity" :min="1">
        <b>{{ PriceRub(item.priceUSD) * item.quantity }}</b>
        <button @click="Remove(item)">Remove</button>
      </li>
    </ul>
    Total: <b>{{ total }}</b>
  </div>
</template>

<script>
import names from '/task/names.json';

export default {
  props: {
    added: {
      type: Object,
      default: () => {},
    },
    currency: Number,
  },
  data() {
    return {
      items: [],
      names,
    };
  },
  computed: {
    total() {
      return this.items.reduce((acc, item) => {
        const priceRub = this.PriceRub(item.priceUSD);
        const total = priceRub * item.quantity;
        return acc + total;
      }, 0);
    },
  },
  watch: {
    added(item) {
      if (item && Object.keys(item).length > 0) {
        // find double of item to add
        const double = this.items.find(el => {
          return el.id === item.id && el.groupId === item.groupId;
        });
        // if cart contains double, add one more
        if (double) {
          double.quantity += 1;
        // else add first instance
        } else  {
          this.items.push({
            quantity: 1,
            ...item,
          });
        }
      }
    },
  },
  methods: {
    Remove(item) {
      const index = this.items.findIndex(el => {
        return el.id === item.id && el.groupId === item.groupId;
      });
      this.items.splice(index, 1);
    },
    GroupName(groupId) {
      return this.names[groupId].G;
    },
    GoodName(groupId, goodId) {
      return this.names[groupId].B[goodId].N;
    },
    PriceRub(usd) {
      return Math.round(this.currency * usd);
    },
  }
}
</script>