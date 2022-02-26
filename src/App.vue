<template>
  <div class="bg-gray-200 p-5">
    <div class="bg-white p-2 shadow-lg m-2 float-right">Cart {{ cart }}</div>
    <div class="max-w-md mx-auto bg-white shadow-xl p-5 flex flex-wrap">
      <img
        :src="chosenVariant.image"
        class="w-[250px] h-[250px] object-contain"
      />
      <div>
        <div class="font-bold">
          {{ title }}
        </div>
        <div
          v-if="chosenVariant.stock"
          class="inline-flex bg-green-500 px-2 rounded-full text-[10px] text-white font-bold"
        >
          In Stock
        </div>
        <div
          v-else
          class="inline-flex bg-red-500 px-2 rounded-full text-[10px] text-white font-bold"
        >
          Out of Stock
        </div>
        <ul>
          <li
            v-for="(detail, index) in details"
            :key="index"
            class="text-gray-500 text-[10px] inline-block mx-1"
          >
            {{ detail }}
          </li>
        </ul>
        <div class="space-x-2">
          <label
            v-for="variant in variants"
            :key="variant.id"
            :for="variant.id"
            class="cursor-pointer inline-flex items-center transition duration-150 ease-in-out"
          >
            <input
              type="radio"
              name="color"
              :id="variant.id"
              :value="variant.color"
              @change="chosenVariant = variant"
              class="hidden"
            />
            <div
              :style="{ 'background-color': variant.color }"
              :class="{ 'opacity-20': !variant.stock }"
              class="w-6 h-6 rounded-full capitalize"
              :title="variant.color"
            ></div>
          </label>
        </div>

        <button
          @click="addToCart()"
          class="disabled:bg-gray-200 disabled:hover:bg-gray-200 disabled:active:bg-gray-200 disabled:focus:bg-gray-200 disabled:text-gray-500 disabled:border-gray-500 disabled:cursor-not-allowed mt-10 bg-black text-white hover:bg-white hover:text-black active:bg-white active:text-black focus:bg-white focus:text-black border border-black transition duration-150 ease-in-out px-4 rounded-full uppercase text-sm font-semibold"
          :disabled="!chosenVariant.stock"
        >
          Add to Cart
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      cart: 0,
      product: "Socks",
      brand: "Super special",
      details: ["50% cotton", "30% wool", "20% polyester"],
      chosenVariant: "",
      variants: [
        {
          id: 2234,
          color: "green",
          image: "./assets/images/socks_green.jpg",
          stock: 2,
        },
        {
          id: 2235,
          color: "blue",
          image: "./assets/images/socks_blue.jpg",
          stock: 5,
        },
      ],
    };
  },
  mounted() {
    this.chosenVariant = this.variants[0];
  },
  methods: {
    addToCart() {
      if (this.chosenVariant.stock) {
        this.cart += 1;
        this.variants.filter((product) => {
          return this.variants.id === product.id ? (product.stock -= 1) : "";
        });
        this.chosenVariant.stock -= 1;
      }
    },
  },
  computed: {
    title() {
      return this.brand + " " + this.product;
    },
  },
};
</script>



