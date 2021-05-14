<template>
  <div>
    <h1>Cart:</h1>
    {{ itemsQuantified }}
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
  computed: {
    itemsQuantified() {
      return this.items.reduce((acc, curVal) => {
        const double = acc.find(el => {
          return el.id === curVal.id && el.groupId === curVal.groupId;
        });
        // if cart contains double, add one more
        if (double) {
          double.quantity += 1;
        // else add first instance
        } else {
          acc.push({
            quantity: 1,
            ...curVal,
          });
        }
        return acc;
      }, [])
    },
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