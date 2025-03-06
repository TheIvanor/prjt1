<template>
    <div class="p-x-20">
        <ul class="list bg-base-100 rounded-box shadow-md">

            <li class="p-4 pb-2 text-xs opacity-60 tracking-wide">Список товорів у кошику</li>

            <li v-for="product in localProducts" :key="product.id" class="list-row">
                <div><img style="width: 100%;max-width: 150px;" :src="product.images[0]" :alt="product.title"/>
                </div>
                <div>
                    <div>{{ product.title }}</div>
                    <div class="text-xs uppercase font-semibold opacity-60">{{ product.description }}</div>
                </div>
                <div class="font-bolt ">{{ product.price }}</div>
              </li>
        </ul>
          <div class="flex justify-end mt-5">
              <button class="btn btn-primary">Оформити замовлення</button>
          </div>
    </div>
</template>

<script setup>

let localProducts = localStorage.getItem('cart');
if (localProducts){
  localProducts = JSON.parse(localProducts);
}

</script>
