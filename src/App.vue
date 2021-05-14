<template>
  <main>
    <input type="number" :min="20" :max="80" :step="5" v-model.number="currency" >
    <Catalogue :currency="currency" @buy="(good) => { addedItem = good }" />
    <Cart :added="addedItem" :currency="currency" />
  </main>
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
      currency: 74.3,
    };
  },
}
</script>
