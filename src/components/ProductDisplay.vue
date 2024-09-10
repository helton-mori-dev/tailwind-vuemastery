<script setup>
import { ref, computed } from "vue";
import socksBlueImage from "../assets/socks_blue.jpg";
import socksGreenImage from "../assets/socks_green.jpg";

const props = defineProps({
  premium: {
    type: Boolean,
    required: true,
  },
});

const emit = defineEmits(["add-to-cart"]);

const product = ref("Socks");
const brand = ref("Vue Mastery");
const selectedVariant = ref(0);
const details = ref(["50% cotton", "30% wool", "20% polyester"]);

const variants = ref([
  { id: 2234, color: "green", image: socksGreenImage, quantity: 50 },
  { id: 2235, color: "blue", image: socksBlueImage, quantity: 0 },
]);

const title = computed(() => {
  return brand.value + " " + product.value;
});

const image = computed(() => {
  return variants.value[selectedVariant.value].image;
});

const inStock = computed(() => {
  return variants.value[selectedVariant.value].quantity > 0;
});

const shipping = computed(() => {
  if (props.premium) {
    return "Free";
  }
  return 2.99;
});

function addToCart() {
  emit("add-to-cart", variants.value[selectedVariant.value].id);
}

function updateVariant(index) {
  selectedVariant.value = index;
}
</script>

<template>
  <div class="p-4">
    <div class="flex flex-row flex-wrap">
      <div class="w-[100%] md:w-[50%]">
        <img v-bind:src="image" class="w-[70%] m-10 p-4 border-2 border-mist" />
      </div>
      <div class="w-[100%] ml-[10px] md:w-[50%] md:ml-0">
        <h1>{{ title }}</h1>
        <p v-if="inStock">In Stock</p>
        <p v-else>Out of Stock</p>
        <p>Shipping: {{ shipping }}</p>
        <ul>
          <li v-for="detail in details">{{ detail }}</li>
        </ul>
        <div
          v-for="(variant, index) in variants"
          class="w-12 h-12 mt-2 border-solid border-2 border-mist rounded-[50%]"
          :class="{ green: 'bg-[green]', blue: 'bg-[blue]' }[variant.color]"
          :key="variant.id"
          @mouseover="updateVariant(index)"
        ></div>

        <button
          class="w-40 h-15 m-8 p-5 rounded-[5px] bg-midnight text-white text-center text-lg leading-none cursor-pointer btn-shading-bn"
          :class="inStock ? [] : ['bg-mist', 'cursor-not-allowed']"
          :disabled="!inStock"
          @click="addToCart"
        >
          Add to Cart
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* .product-display {
  padding: 16px;
}

.product-container {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}

.product-image {
  width: 100%;
}

img {
  width: 70%;
  margin: 40px;
  padding: 15px;
  border: 2px solid #d8d8d8;
}

.product-info {
  width: 100%;
  margin-left: 10px;
}

.color-circle {
  width: 50px;
  height: 50px;
  margin-top: 8px;
  border: 2px solid #d8d8d8;
  border-radius: 50%;
}

.button {
  /* width: 160px;
  height: 60px;
  margin: 30px;
  padding: 20px;
  font-size: 18px;
  color: white;
  text-align: center;
  border-radius: 5px;
  box-shadow: inset 0 -0.6em 1em -0.35em rgba(0, 0, 0, 0.17),
    inset 0 0.6em 2em -0.3em rgba(255, 255, 255, 0.15),
    inset 0 0 0em 0.05em rgba(255, 255, 255, 0.12);

  background-color: #39495c;
  cursor: pointer;
}

.button.disabled {
  background-color: #d8d8d8;
  cursor: not-allowed;
}
  */

/* @media only screen and (min-width: 860px) {
  .product-image {
    width: 50%;
  }

  .product-info {
    width: 50%;
    margin-left: 0;
  }
} */
</style>
