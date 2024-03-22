<template>
    <div class="itemList">

      <div v-for="(item, index) in items" :key="index" class="item">
        <div class="item-details">
          <h2 class="item-name">{{ item.name }}</h2>
          <h4 class="item-price">$ {{ item.price }}</h4>
        </div>
        <div class="button-group">
          <button @click="addToCart(item)" class="add-to-cart-btn">Add to Cart</button>
          <button @click="updateItem(index)" class="update-btn">Update</button>
          <button @click="deleteItem(index)" class="delete-btn">Delete</button>
          <br>
          <br>
        </div>
      </div>
      <br>
      <button @click="addItem" class="add-item-btn">Add New Item</button>
      <br>
      <br>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        items: [
          { name: 'Milk', price: 10 },
          { name: 'Soap', price: 20 },
          { name: 'Toothpaste', price: 30 },
          { name: 'Shampoo', price: 40 },
          { name: 'Lotion', price: 50 },
        ]
      };
    },
    methods: {
      addToCart(item) {
        this.$emit('add-to-cart', item);
      },
      updateItem(index) {
        const item = this.items[index];
        const newName = prompt('Updated Name:', item.name);
        const newPrice = parseFloat(prompt('Updated Price:', item.price));
        
        if (newName && !isNaN(newPrice)) {
          item.name = newName;
          item.price = newPrice;
        }
      },
      deleteItem(index) {
        this.items.splice(index, 1);
      },
      addItem() {
        const name = prompt('Name of Item');
        const price = parseFloat(prompt('Price of Item'));
        
        if (name && !isNaN(price)) {
          this.items.push({ name, price });
        }
      }
    }
  };
  </script>
  