<template>
    <div>
        <ul class="list bg-base-100 rounded-box shadow-md">

            <li class="p-4 pb-2 text-xs opacity-60 tracking-wide">Most played songs this week</li>

            <li v-for="product in products" :key="product.id" class="list-row">
                <div><img class="size-10 rounded-box" src="https://img.daisyui.com/images/profile/demo/1@94.webp" />
                </div>
                <div>
                    <div>{{ product.title }}</div>
                    <div class="text-xs uppercase font-semibold opacity-60">{{ product.description }}</div>
                </div>
                <button class="btn btn-square btn-ghost">
                    <svg class="size-[1.2em]" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
                        <g stroke-linejoin="round" stroke-linecap="round" stroke-width="2" fill="none"
                            stroke="currentColor">
                            <path d="M6 3L20 12 6 21 6 3z"></path>
                        </g>
                    </svg>
                </button>
                <button class="btn btn-square btn-ghost">
                    <svg class="size-[1.2em]" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
                        <g stroke-linejoin="round" stroke-linecap="round" stroke-width="2" fill="none"
                            stroke="currentColor">
                            <path
                                d="M19 14c1.49-1.46 3-3.21 3-5.5A5.5 5.5 0 0 0 16.5 3c-1.76 0-3 .5-4.5 2-1.5-1.5-2.74-2-4.5-2A5.5 5.5 0 0 0 2 8.5c0 2.3 1.5 4.05 3 5.5l7 7Z">
                            </path>
                        </g>
                    </svg>
                </button>
            </li>
        </ul>
    </div>
</template>

<script setup>
import { ref } from 'vue';
let products = ref([
    {
        id: 1,
        title: 'ips 10',
        description: 'great choice',
        price: 1500,
    },
    {
        id: 2,
        title: 'ips 11',
        description: 'great choice',
        price: 1500,
    },
    {
        id: 3,
        title: 'ips 12',
        description: 'great choice',
        price: 1500,
    },
    {
        id: 4,
        title: 'ips 13',
        description: 'great choice',
        price: 1500,
    },
    {
        id: 5,
        title: 'ips 14',
        description: 'great choice',
        price: 1500,

    }
]);

</script>