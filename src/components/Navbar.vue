<template>
    <!-- Navbar -->
    <div class="w-full navbar bg-base-100 shadow-lg lg:justify-center">
        <div class="flex-none lg:hidden">
            <label for="my-drawer-3" class="btn btn-square btn-ghost">
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"
                    class="inline-block w-6 h-6 stroke-current">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16">
                    </path>
                </svg>
            </label>
        </div>
        <div class="w-full flex justify-between max-w-7xl">
            <div class="flex-1 px-2 mx-2">
                <div class="flex">
                    <a class="
                        text-xl
                        font-bold
                        text-primary
                        cursor-pointer
                        md:text-2xl
                        hover:text-gray-700
                      " @click="routeTo({ name: 'Welcome' })">
                        AI-Mind Power
                        <sub class="font-light text-sm text-gray-500">v{{ version }}</sub>
                    </a>
                </div>
            </div>
            <div class="flex-none hidden lg:block">
                <ul class="menu menu-horizontal">
                    <!-- Navbar menu content here -->
                    <li v-for="(item, idx) in navItems" :key="idx">
                        <a @click="routeTo(item.routeData)">{{ item.name }}</a>
                    </li>
                    <li class="border border-error text-error" v-if="user">
                        <a @click="logout">登出</a>
                    </li>
                    <li class="border border-primary text-primary" v-else>
                        <a @click="routeTo({ name: 'Login' })">登入</a>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>


<script setup>
import { computed, ref } from 'vue'
import { useRouter } from 'vue-router'
import { useStore } from 'vuex'
const version = computed(() => __APP_VERSION__)
const store = useStore()
const router = useRouter()
const user = computed(() => store.getters.stateUser)
const navItems = ref([
    {
        name: '腦波分析', 
        routeData: { name: 'Home' }
    },
    {
        name: '腦波音樂',
        routeData: { name: 'Music' }
    },
    {
        name: '雙人音樂',
        routeData: { name: 'DualMusic' }
    },
    {
        name: '社交平台',
        routeData: { name: 'SocialPlatform' }
    },
    // {
    //     name: '腦波NFT',
    //     routeData: { name: 'NFT' }
    // },
    {
        name: '關於我們',
        routeData: { name: 'About' }
    }
])

function routeTo(routeData) {
    router.push(routeData)
}

function logout() {
    user.value = null
    store.dispatch('logOut')
    router.push({ name: 'Login' })
}
</script>