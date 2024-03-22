<template>
    <div class="cart">
 
      <div v-for="(item, index) in cart" :key="index" class="cart-item">
        <div class="item-details">
          <br>
          <h2 class="item-name">{{ item.name }}</h2>
          <h4 class="item-price">$ {{ item.price }}</h4>
          <p class="item-quantity">Quantity: {{ item.quantity }}</p>
        </div>
        <div class="quantity-controls">
          <button @click="openDialog(index)">Update</button>
          <button @click="removeFromCart(index)">Remove</button>
        </div>
      </div>
      <br>
      <h2 class="total">Total: $ {{ total }}</h2>
  
      <div v-if="dialogVisible" class="dialog">
        <div class="dialog-content">
          <p>Enter new quantity for {{ cart[selectedItemIndex].name }}:</p>
          <input type="number" v-model="newQuantity" @keydown.enter="updateQuantity(selectedItemIndex)">
          <button @click="updateQuantity(selectedItemIndex)">Save</button>
          <button @click="closeDialog">Cancel</button>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        dialogVisible: false,
        selectedItemIndex: null,
        newQuantity: '', 
      };
    },
    props: {
      cart: {
        type: Array,
        default: () => [],
      },
    },
    computed: {
      total() {
        if (this.cart.length === 0) {
          return 0; 
        }
        return this.cart.reduce((acc, item) => acc + item.price * item.quantity, 0);
      },
    },
    methods: {
      openDialog(index) {
        this.selectedItemIndex = index;
       
        this.newQuantity = this.cart[index].quantity.toString(); 
        this.dialogVisible = true;
      },
      closeDialog() {
        this.dialogVisible = false;
      },
      updateQuantity(index) {
        
        const newQuantity = parseInt(this.newQuantity);
        
        if (!isNaN(newQuantity) && newQuantity >= 0) {
          this.$emit('update-quantity', { index, quantity: newQuantity });
          this.closeDialog();
        } else {
          alert('Please enter a valid non-negative integer!');
        }
      },
      removeFromCart(index) {
        this.$emit('remove-from-cart', index);
      },
    },
  };
  </script>
  