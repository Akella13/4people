<template>
  <main>
    <Catalogue @buy="AddToCart"/>
    <Cart :added="addedItem"/>
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
    };
  },
  methods: {
    AddToCart(good) {
      // hack to add double of previous good
      this.addedItem = { ...good };
    }
  },
}
</script>
