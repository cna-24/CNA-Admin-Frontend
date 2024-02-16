
<template>
    <div>
      <form @submit.prevent="submitForm">
        <label for="name">Product Name:</label>
        <input v-model="product.name" type="text" id="name" required>
  
        <label for="price">Product Price:</label>
        <input v-model="product.price" type="number" id="price" required>
  
        <label for="quantity">Product Quantity:</label>
        <input v-model="product.quantity" type="number" id="quantity" required>
  
        <label for="description">Product Description:</label>
        <textarea v-model="product.description" id="description" rows="4"></textarea>
  
        <button type="submit">{{ isEditing ? 'Update Product' : 'Add Product' }}</button>
      </form>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    props: {
      product: {
        type: Object,
        default: () => ({ name: '', price: 0, quantity: 0, description: '' })
      },
      isEditing: {
        type: Boolean,
        default: false
      }
    },
    methods: {
      async submitForm() {
        try {
          if (this.isEditing && this.product.id) {
            // Updatera produkt
            await axios.put(`/api/products/${this.product.id}`, this.product);
            this.$emit('update-product', this.product);
          } else {
            // Ny produkt
            const response = await axios.post('/api/products', this.product);
            this.$emit('add-product', response.data);
          }
        } catch (error) {
          console.error('API request failed:', error);
          this.$emit('api-request-failed', 'Failed to submit the form. Please try again.');
        }
      }
    }
  };
  </script>