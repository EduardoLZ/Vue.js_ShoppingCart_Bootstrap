<template>
  <div class="container">
    <div class="row mt-2 justify-content-center">
      <div class="col-2" v-for="product in products" :key="product.id">
        <div class="card" style="width: 10rem;">
          <img :src="product.url" class="card-img-top" />
          <div class="card-body">
            <h6 class="card-title">
              {{ product.name }} - $ {{ product.price }}
            </h6>
            <button
              :disabled="product.cart"
              @click="addProduct(product)"
              href="#"
              class="btn  btn-block"
              :class="{
                'btn-primary': !product.cart,
                'btn-success': product.cart,
              }"
            >
              {{ !product.cart ? "Add" : "Added" }}
            </button>
          </div>
        </div>
      </div>
    </div>
    <div class="row mt-2">
      <table class="table  text-center">
        <thead>
          <tr>
            <th scope="col">#</th>
            <th scope="col">Product</th>
            <th scope="col">Product Name</th>
            <th scope="col">Price</th>
            <th scope="col">Quantity</th>
            <th scope="col">Remove</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(product, index) in cart" :key="product.id">
            <th scope="row">{{ index + 1 }}</th>
            <th scope="row">
              <img :src="product.url" style="width: 4rem;" />
            </th>
            <td>{{ product.name }}</td>
            <td>
              {{ product.price }}
            </td>
            <td>
              <button @click="decreaseQ(product)" class="btn btn-info btn-sm">
                -
              </button>
              {{ product.quantity }}
              <button
                @click="increaseQ(product)"
                class="btn btn-info btn-sm"
                size="sm"
              >
                +
              </button>
            </td>

            <td>
              <button @click="removeProduct(product)" class="btn btn-danger">
                Remove
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="row">
      <div class="col text-center">
        <h4>TOTAL: {{ totalCart }}</h4>
      </div>
    </div>
  </div>
</template>

<script>
import { computed, ref } from "vue";
export default {
  name: "Shoppingcart",
  setup() {
    const cart = ref([]);
    const products = ref([
      {
        id: 1,
        name: "Product 1",
        price: 1,
        url:
          "https://images.pexels.com/photos/59945/strawberry-fruit-delicious-red-59945.jpeg",
        cart: false,
        quantity: 1,
      },
      {
        id: 2,
        name: "Product 2",
        price: 2,
        url:
          "https://images.pexels.com/photos/52533/orange-fruit-vitamins-healthy-eating-52533.jpeg",
        cart: false,
        quantity: 1,
      },
      {
        id: 3,
        name: "Product 3",
        price: 3,
        url:
          "https://images.pexels.com/photos/51312/kiwi-fruit-vitamins-healthy-eating-51312.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940",
        cart: false,
        quantity: 1,
      },
    ]);

    function addProduct(product) {
      cart.value.push(product);
      products.value.map((p) => {
        if (product.id == p.id) {
          p.cart = !p.cart;
        }
      });
    }
    function removeProduct(product) {
      cart.value = cart.value.filter((p) => p.id != product.id);
      products.value.map((p) => {
        if (product.id == p.id) {
          p.cart = !p.cart;
          p.quantity = 1;
        }
      });
    }
    function increaseQ(product) {
      products.value.map((p) => {
        if (product.id == p.id) {
          p.quantity += 1;
        }
      });
    }
    function decreaseQ(product) {
      products.value.map((p) => {
        if (product.id == p.id && p.quantity > 1) {
          p.quantity -= 1;
        }
      });
    }
    const totalCart = computed(() => {
      let t = 0;
      cart.value.forEach((e) => {
        t += e.quantity * e.price;
      });
      return t;
    });
    return {
      products,
      addProduct,
      increaseQ,
      removeProduct,
      decreaseQ,
      cart,
      totalCart,
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
