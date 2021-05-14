<template>
  <div>
    <header class="header">
      <label>
        Курс валюты ($):
        <input type="number" :min="20" :max="80" :step="5" v-model.number="currency" tabindex="-1">
      </label>
      <Cart :added="addedItem" :currency="currency" />
    </header>
    <main>
      <Catalogue :currency="currency" @buy="(good) => { addedItem = good }" />
    </main>
  </div>
</template>

<script>
import Catalogue from '@/components/Catalogue';
import Cart from '@/components/Cart';

export default {
  name: 'App',
  components: {
    Catalogue,
    Cart,
  },
  data() {
    return {
      addedItem: {},
      currency: 75,
    };
  },
}
</script>

<style>
  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: #d2dde1;
    border-bottom: 1px solid #ccc;
    padding: 1em;
    position: relative;
  }

  .button {
    cursor: pointer;
  }
</style>