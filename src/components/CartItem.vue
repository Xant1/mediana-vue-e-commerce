<template>
  <tr>
    <td>
      {{ item.product.title }}
    </td>
    <td>
      {{ item.product.brand }}
    </td>
    <td>${{ item.product.price }}</td>
    <td>
      <a style="cursor: pointer" @click="decrementQuantity(item)">-</a>
      {{ item.quantity }}
      <a style="cursor: pointer" @click="incrementQuantity(item)">+</a>
    </td>
    <td>${{ getItemTotal(item).toFixed(2) }}</td>
    <td>
      <a style="color: red; cursor: pointer" @click="removeFromCart(item)">X</a>
    </td>
  </tr>
</template>

<script>
export default {
  name: 'CartItem',
  props: {
    initialItem: Object,
  },
  data() {
    return {
      item: this.initialItem,
    };
  },
  methods: {
   
    getItemTotal(item) {
      return item.quantity * item.product.price;
    },
    
    decrementQuantity(item) {
      item.quantity -= 1;

      if (item.quantity === 0) {
        this.$emit('removeFromCart', item);
      }

      this.updateCart();
    },
    
    incrementQuantity(item) {
      item.quantity += 1;

      this.updateCart();
    },
    
    updateCart() {
      localStorage.setItem('cart', JSON.stringify(this.$store.state.cart));
    },
    
    removeFromCart(item) {
      this.$emit('removeFromCart', item);

      this.updateCart();
    },
  },
};
</script>
