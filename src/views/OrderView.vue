<template>
  <a href="/" class="btn btn-ghost text-xl">Головна</a>
  <h1>Orders</h1>

  <ui>
    <li v-for="(order, index) in orders" :key="order.id" clickable v-ripple>
      <p>Number:{{ index + 1 }}</p>
      <p>Name:{{ order.name }}</p>
      <p>Phone:{{ order.phone }}</p>

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
            <tr v-for="(item, index) in order.order" :key="item.id">
              <th>{{ index + 1 }}</th>
              <td>{{ item.title }}</td>
              <td>{{ item.description }}</td>
              <td>{{ item.price }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </li>
  </ui>
</template>

<script setup>
const orderFromLocalStorage = localStorage.getItem('orders')
const orders = JSON.parse(orderFromLocalStorage)
</script>
