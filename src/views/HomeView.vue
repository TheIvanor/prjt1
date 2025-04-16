<script setup>
import { ref, computed, onMounted } from 'vue'
let products = ref([])

const cartProducts = ref([])

const totoalPrise = computed(() => {
  return cartProducts.value
    .reduce((totoal, product) => totoal + product.price * product.count, 0)
    .toFixed(2)
})

const totoalCountProductsInCart = computed(() => {
  return cartProducts.value.reduce((totoal, product) => totoal + product.count, 0)
})

const addToCart = (product) => {
  product.inCart = 1

  cartProducts.value.push(product)

  localStorage.setItem('cart', JSON.stringify(cartProducts.value))
}

onMounted(() => {
  const localProducts = localStorage.getItem('cart')
  if (localProducts) {
    cartProducts.value = JSON.parse(localProducts)
  }
})

fetch('https://dummyjson.com/products')
  .then((res) => res.json())
  .then((data) => {
    products.value = data.products.map((product) => {
      product.count = 1
      product.inCart = cartProducts.value.some((item) => item.id === product.id)
      return product
    })
  })

function openModal() {
  const element = document.getElementById('my_modal_1')
  element.classList.add('modal-open')
}

function closeModal() {
  const element = document.getElementById('my_modal_1')
  element.classList.remove('modal-open')
}
</script>

<template>
  <div>
    <dialog class="modal" id="my_modal_1">
      <div class="modal-box">
        <form method="dialog">
          <button
            class="btn btn-sm btn-circle btn-ghost absolute right-2 top-2"
            @click="closeModal"
          >
            ✕
          </button>
        </form>
        <h3 class="text-lg font-bold">Корзина</h3>
        <div class="overflow-x-auto">
          <table class="table table-xs">
            <thead>
              <tr>
                <th></th>
                <th>Title</th>
                <th>Description</th>
                <th>Price</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(item, index) in cartProducts" :key="item.id">
                <th>{{ index + 1 }}</th>
                <td>{{ item.title }}</td>
                <td>{{ item.description }}</td>
                <td>{{ item.price }}</td>
              </tr>
            </tbody>
          </table>
        </div>

        <a href="/cart" class="btn btn-primary">Оформити замовлення</a>
      </div>
    </dialog>
    <div class="navbar bg-primary text-pimary-content shadow-sm">
      <div class="flex-1">
        <a class="btn btn-ghost text-xl">daisyUI</a>
      </div>
      <div class="flex-none">
        <div class="dropdown dropdown-end">
          <div tabindex="0" role="button" class="btn btn-ghost btn-circle">
            <div class="indicator">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="h-5 w-5"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13l-2.293 2.293c-.63.63-.184 1.707.707 1.707H17m0 0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 11-4 0 2 2 0 014 0z"
                />
              </svg>
              <span class="badge badge-sm indicator-item">{{ totoalCountProductsInCart }}</span>
            </div>
          </div>
          <div
            tabindex="0"
            class="card card-compact dropdown-content bg-base-100 z-[1] mt-3 w-52 shadow"
          >
            <div class="card-body">
              <span class="text-lg font-bold">{{ totoalCountProductsInCart }} товарів</span>
              <span class="text-info">Сума: ${{ totoalPrise }}</span>
              <div class="card-actions">
                <a href="/cart" class="btn btn-primary btn-block">В корзину</a>
              </div>
            </div>
          </div>
        </div>
        <div class="dropdown dropdown-end">
          <div tabindex="0" role="button" class="btn btn-ghost btn-circle avatar">
            <div class="w-10 rounded-full">
              <img
                alt="Tailwind CSS Navbar component"
                src="https://img.daisyui.com/images/stock/photo-1534528741775-53994a69daeb.webp"
              />
            </div>
          </div>
          <ul
            tabindex="0"
            class="menu menu-sm dropdown-content bg-base-100 rounded-box z-[1] mt-3 w-52 p-2 shadow"
          >
            <li>
              <a class="justify-between">
                Profile
                <span class="badge">New</span>
              </a>
            </li>
            <li><a href="/orders">Orders</a></li>
            <li><a>Logout</a></li>
          </ul>
        </div>
      </div>
    </div>

    <div class="flex flex-wrap gap-4 mx-auto conteiner">
      <div v-for="product in products" :key="product.id" class="card bg-base-100 w-80 shadow-xl">
        <figure>
          <img
            style="width: 100%; max-width: 150px"
            :src="product.images[0]"
            :alt="product.title"
          />
        </figure>
        <div class="card-body">
          <p class="text-center font-bold text-lg">{{ product.price }}</p>
          <h2 class="card-title">{{ product.title }}</h2>
          <p>{{ product.description }}</p>
          <div class="card-actions justify-end">
            <button v-if="!product.inCart" class="btn btn-primary px-6" @click="addToCart(product)">
              <svg
                class="h-6 w-6"
                viewBox="0 0 24 24"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  d="M6.29977 5H21L19 12H7.37671M20 16H8L6 3H3M9 20C9 20.5523 8.55228 21 8 21C7.44772 21 7 20.5523 7 20C7 19.4477 7.44772 19 8 19C8.55228 19 9 19.4477 9 20ZM20 20C20 20.5523 19.5523 21 19 21C18.4477 21 18 20.5523 18 20C18 19.4477 18.4477 19 19 19C19.5523 19 20 19.4477 20 20Z"
                  stroke="#ffffff"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                />
              </svg>
            </button>

            <button v-else class="btn btn-primary px-6" @click="openModal">
              <svg
                class="h-6 w-6"
                viewBox="0 0 24 24"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  d="M21 5L19 12H7.37671M20 16H8L6 3H3M11 6L13 8L17 4M9 20C9 20.5523 8.55228 21 8 21C7.44772 21 7 20.5523 7 20C7 19.4477 7.44772 19 8 19C8.55228 19 9 19.4477 9 20ZM20 20C20 20.5523 19.5523 21 19 21C18.4477 21 18 20.5523 18 20C18 19.4477 18.4477 19 19 19C19.5523 19 20 19.4477 20 20Z"
                  stroke="#00ff00"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                />
              </svg>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
