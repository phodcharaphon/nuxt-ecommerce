<template>
    <MainLayout>
        <div id="ItemPage" class="mt-4 max-w-[1260px] mx-auto px-2">
            <div class="md:flex gap-4 jusitify-between mx-auto w-full">
                <div class="md:w-[40%]">
                    <img v-if="currentImage" class="rounded-lg object-fit" :src="currentImage">
                    <div v-if="images[0] !== ''" class="flex items-center justify-center mt-2">
                        <div v-for="image in images">
                            <img @mouseover="currentImage = image" @click="currentImage = image" width="70"
                                class="rounded-md object-fit border-[3px] cursor-pointer"
                                :class="currentImage === image ? 'border-[#1d4ed8]' : ''" :src="image">
                        </div>
                    </div>
                </div>
                <div class="md:w-[60%] bg-white p-3 rounded-lg">
                    <div v-if="product && product.data">
                        <p class="mb-2">{{ product.data.title }}</p>
                        <p class="font-ight text-[12px] mb-2">{{ product.data.description }}</p>
                    </div>
                
                <div class="border-b" />
                <div class="flex item-center justify-start gap-2 my-2">
                    <div class="text-xl font-bold">{{ priceComputed }} บาท</div>
                </div>
                <button
                @click="addToCart()"
                :disabled="isInCart"
                class="px-6 py-2 rounded-lg text-white text-lg font-semibold bg-gradient-to-r from-[#082f49] to-[#38bdf8]"
                >
            <div v-if="isInCart">มีสินค้านี้แล้ว</div>
            <div v-else>เพิ่มสินค้า</div>
            </button>
            </div>
        </div>
        </div>
    </MainLayout>
</template>

<script setup>
import MainLayout from '~/layouts/MainLayout.vue';
import { useUserStore } from '~/stores/user';
const userStore = useUserStore()

const route = useRoute()

let product = ref(null)
let currentImage = ref(null)

onBeforeMount(async () => {
    product.value = await useFetch(`/api/prisma/get-product-by-id/${route.params.id}`)
})

watchEffect(() => {
    if (product.value && product.value.data) {
        currentImage.value = product.value.data.url
        images.value[0] = product.value.data.url
        userStore.isLoading = false
    }
})

const isInCart = computed(() => {
    let res = false
    userStore.cart.forEach(prod => {
        if (route.params.id == prod.id) {
            res = true
        }
    })
    return res
})

const priceComputed = computed(() => {
    if (product.value && product.value.data) {
        return product.value.data.price
    }
    return '0.00'
})

const images = ref([
    '',
    'https://picsum.photos/id/212/800/800',
    'https://picsum.photos/id/233/800/800',
    'https://picsum.photos/id/165/800/800',
    'https://picsum.photos/id/99/800/800',
    'https://picsum.photos/id/144/800/800',
])

const addToCart = () => {
    userStore.cart.push(product.value.data)
}
</script>
