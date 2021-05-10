<script>
export default {
  name: "MenuItem",
  props: {
    // addToShoppingCart: {
    //   type: Function
    // },
    image: {
      type: Object,
      default() {
        return { source: "", alt: "" };
      }
    },
    inStock: {
      type: Boolean
    },
    name: String,
    price: {
      type: Number,
      required: true
    },
    quantity: Number
  },
  data() {
    return {
      onSale: false
    };
  },
  methods: {
    goToUrl(url) {
      window.location.href = url;
    },
    updateShoppingCart(quantity) {
      this.$emit("add-items-to-cart", quantity);
    },
    changeQuantity(event) {
      let newQuantity = parseInt(event.target.value);
      this.$emit("change-quantity", { quantity: newQuantity, name: this.name });
    }
  },
  computed: {
    generatedPrice() {
      if (this.onSale) {
        return (this.price * 0.9).toFixed(2);
      } else {
        return this.price;
      }
    }
  },
  beforeMount() {
    const today = new Date().getDate();
    if (today % 2 === 0) {
      this.onSale = true;
    }
  }
};
</script>

<template>
  <div class="menu-item">
    <img
      class="menu-item__image"
      :src="image.source"
      :alt="image.alt"
      @click="goToUrl(`item/${name}`)"
    />
    <div>
      <h3>{{ name }}</h3>
      <p>Prix : {{ generatedPrice }}</p>
      <p v-if="inStock">En stock</p>
      <p v-else>En rupture de stock</p>
      <div>
        <label for="add-item-quantity">Quantité : {{ quantity }}</label>
        <input
          id="add-item-quantity"
          type="number"
          :value="quantity"
          @change="changeQuantity($event)"
        />
        <button @click="updateShoppingCart(quantity)">
          Ajouter au panier
        </button>
      </div>
    </div>
  </div>
</template>

<style>
.menu-item {
  display: flex;
  width: 500px;
  justify-content: space-between;
  margin-bottom: 30px;
}

.menu-item__image {
  max-width: 300px;
}
</style>
