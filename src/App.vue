<template>
  <main>
    <Catalogue @buy="(good) => { addedItem = good }" />
    <Cart :added="addedItem" />
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
}
</script>
