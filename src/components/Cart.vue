<template>
  <div>
    <h1>Cart:</h1>
    {{ items }}
  </div>
</template>

<script>
export default {
  props: {
    added: {
      type: Object,
      default: () => {},
    },
  },
  data() {
    return {
      items: [],
    };
  },
  watch: {
    added(item) {
      if (item && Object.keys(item).length > 0) {
        this.items.push(item);
      }
    },
  },
}
</script>