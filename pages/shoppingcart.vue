<template>
    <MainLayout>
        <div id="ShoppingCartPage" class="mt-4 max-w-[1260px] mx-auto px-2">
            <div v-if="!userStore.cart.length" class="h-[500px] flex items-center justify-center">
                <div class="pt-20">
                    <img class="mx-auto" width="250" src="">
                    <div class="text-xl text-center mt-4">ไม่มีสินค้า</div>

                    <div v-if="!user" class="flex text-center">
                        <NuxtLink to="/auth" class="bg-[#082f49] w-full text-white font-semibold p-1.5 rounded-full mt-4">
                            เข้าสู่ระบบ
                        </NuxtLink>
                    </div>
                </div>
            </div>
            <div v-else class="md:flex gap-4 justify-between mx-auto w-full">
                <div class="md:w-[65%]">
                    <div class="bg-white rounded-lg p-4">
                        <div class="text-2xl font-bold mb-2">
                            ตระกร้าสินค้า ({{ userStore.cart.length }})
                        </div>
                    </div>

                    <div id="Items" class="bg-white rounded-lg p-4 mt-4">
                        <div v-for="product in userStore.cart">
                            <CartItem 
                                :product="product" 
                                :selectedArray="selectedArray"
                                @selectedRadio="selectedRadioFunc"
                            />
                        </div>
                    </div>
                </div>
                <div class="md:hidden block my-4" />
                <div class="md:w-[35%]">
                    <div id="Summery" class="bg-white rounded-lg-p-4">
                        <div class="text-2xl text-center font-extrabold mb-2 ml-2">การชำระเงิน</div>
                        <div class="flex items-center justify-between my-4">
                            <div class="font-semibold ml-2">รวม</div>
                            <div class="text-1xl font-semibold">
                                <span class="font-extrabold mr-2">{{ totalPriceComputed }} บาท</span>
                            </div>
                        </div>
                        <button @click="goToCheckout"
                            class="flex items-center justify-center bg-[#0c4a6e] w-full text-white text-[18px] font-semibold p-1.5 rounded-full mt-4">
                            ชำระเงิน
                        </button>
                    </div>
                    <div id="PaymentProtection" class="bg-white rounded-lg p-4 mt-4">
                        <div class="text-lg font-semibold mb-2">Payment</div>
                        <div class="flex items-center justify-start gap-8 my-4">
                            <div v-for="card in cards">
                                <img class="h-6" :src="card">
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </MainLayout>
</template>

<script setup>
import MainLayout from '~/layouts/MainLayout.vue';
import { useUserStore } from '~/stores/user';
const userStore = useUserStore()
const user = useSupabaseUser()

let selectedArray = ref([])

onMounted(() => {
    setTimeout(() => userStore.isLoading = false, 200)
})

const cards = ref([
    'visa.png',
    'mastercard.png',
    'paypal.png'
])

const totalPriceComputed = computed(() => {
    let price = 0
    userStore.cart.forEach(prod => {
        price += prod.price
    })
    return price
})

const selectedRadioFunc = (e) => {
    if (!selectedArray.value.length) {
        selectedArray.value.push(e)
        return
    }

    selectedArray.value.forEach((item, i) => {
        if (e.id != item.id) {
            selectedArray.value.push(e)
        } else {
            selectedArray.value.splice(i, 1);
        }
    })
}

const goToCheckout = () => {
    let ids = []
    userStore.checkout = []

    selectedArray.value.forEach(item => ids.push(item.id))

    let res = userStore.cart.filter((item) => {
        return ids.indexOf(item.id) != -1
    })

    res.forEach(item => userStore.checkout.push(toRaw(item)))

    return navigateTo('/checkout')
}
</script>