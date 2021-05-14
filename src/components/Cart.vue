<template>
  <div>
    <button @click="cartShown = !cartShown" class="button">
      Корзина
      <span v-show="items.length > 0">
        ({{ items.length }})  
      </span>
    </button>
    <section v-show="cartShown" class="cart">
      <div v-if="items.length > 0" class="cart__content">
        <table class="table">
          <thead>
            <tr class="table__row">
              <th>Название</th>
              <th>Кол-во</th>
              <th>Цена, &#8381;</th>
              <th></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, index) in items" :key="index" class="table__row">
              <td class="table__cell">
                {{ GoodName(item.groupId, item.id) }}
              </td>
              <td class="table__cell">
                <input type="number" v-model.number="item.quantity" :min="1" class="input__number">
              </td>
              <td class="table__cell">
                <b>{{ PriceRub(item.priceUSD) * item.quantity }}</b>
              </td>
              <td class="table__cell">
                <button class="button table__action" @click="Remove(item)">&#10005;</button>
              </td>
            </tr>
          </tbody>
        </table>
        <p class="cart__total">
          Всего: <b>{{ total }} &#8381;</b>
        </p>
      </div>
      <p v-else class="cart__msg">
        Нет товаров в корзине
      </p>
    </section>
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
      cartShown: false,
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

<style>
  .cart {
    position: absolute;
    right: 1em;
    z-index: 1;
    background-color: #ccc;
    border: 1px solid rgb(118, 118, 118);
    border-radius: 1em 0 1em 1em;
    min-width: 50%;
    min-height: 30vh;
    padding: 1em;
    box-sizing: border-box;
    display: flex;
  }

  .cart__content {
    width: 100%;
  }

  .cart__msg {
    align-self: center;
    width: 100%;
    text-align: center;
  }

  .cart__total {
    text-align: end;
    padding-right: 2.5em;
  }

  .table {
    border-collapse: collapse;
    width: 100%;
  }

  .table__row {
    border-bottom: 1px solid black;
  }

  .table__cell {
    padding: 0 0.5em;
  }

  .table__row:hover .table__action {
    visibility: visible;
  }

  .table__action {
    visibility: hidden;
  }

  .input__number {
    width: 3em;
  }
</style>