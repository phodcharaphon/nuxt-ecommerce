<template>
    <div id="`ProductComponent${product.id}`"
        class="bg-white inline-block rounded hover:shadow-[0_0_10px_3px_rgba(0,0,0,0.15)] cursor-pointer">
        <NuxtLink :to="`/item/${product.id}`">
            <img class="rounded-t" :src="product.url" />
            <div id="ProductDetails">
                <p class="px-1 pt-0.5 text-xl text-[#252525] pt-1">{{ product.title.substring(0, 60) }}</p>
                <span class="flex items-center justify-center gap-3 px-2 pt-1">
                    <span class="text-[#1e1b4b] font-semibold">ราคา {{ priceComputed }} บาท</span>
                    <!-- <span class="text-gray-500 text-sm text-light line-through">{{ oldPirceComputed }} บาท</span> -->
                </span>

                <!-- <span class="px-1 relative -top-1.5 text-[#e11d48] text-xs font-semibold">ลด 10 %</span> -->

                <div class="flex items-center gap-1 px-1 relative -top-1">
                    <!-- <span class="bg-[#FD374F] text-white text-[9px] font-semibold px-1.5 rounded-ms">Test</span>
                    <span class="bg-[#F5F5F5] border text-[#C08562] text-[9px] font-sembold px-1.5 rounded-sm">test2</span> -->
                </div>
               
                
                <p class="px-1 pb-1">
                    <span class="text-[#009A66] text-xs font-semibold">Free Shopping</span>
                </p>
            </div>
        </NuxtLink>
    </div>
</template>

<script setup>
const props = defineProps(['product'])
const { product } = toRefs(props)

const priceComputed = computed(() => {
    return product.value.price
})

// const oldPirceComputed = computed(() => {
//     let res = (product.value.price + (product.value.price))
//     return res.toFixed(2)
// })
</script>