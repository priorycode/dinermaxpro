<script setup>
import { useSidebarStore } from '@/stores/sidebar'
import { onClickOutside } from '@vueuse/core'
import { ChevronsLeft, LayoutDashboard, Gem, Wallet, Network, MessageCircleQuestion } from 'lucide-vue-next'
import { ref } from 'vue'
import SidebarItem from './SidebarItem.vue'
import DarkModeSwitcher from "./DarkModeSwitcher.vue"
import { useRoute } from 'vue-router';

const target = ref(null)
const sidebarStore = useSidebarStore()
const route = useRoute();

onClickOutside(target, () => {
  sidebarStore.isSidebarOpen = false
})

const menuGroups = ref([
  {
    menuItems: [
      { icon: LayoutDashboard, label: 'Dashboard', route: '/dashboard' },
      { icon: Gem, label: 'Membresia', route: '/dashboard/membership' },
      { icon: Wallet, label: 'Billetera', route: '/dashboard/wallet' },
      { icon: Network, label: 'Red de referidos', route: '/dashboard/references' }
    ]
  }
])

function isActive(routePath) {
  return route.path === routePath;
}
</script>

<template>
  <aside
      class="md:w-[280px] md:pt-12 pt-4 absolute left-0 top-0 z-50 flex h-screen w-72.5 flex-col overflow-y-hidden bg-bgDashboardLigth dark:bg-bgDashboardDark duration-300 ease-linear dark:bg-boxdark lg:static lg:translate-x-0"
      :class="{
      'translate-x-0': sidebarStore.isSidebarOpen,
      '-translate-x-full': !sidebarStore.isSidebarOpen,
      'lg:translate-x-0': sidebarStore.isSidebarOpen  // Aseguramos que en pantallas grandes esté siempre visible
    }"
      ref="target"
  >
    <!-- SIDEBAR HEADER -->
    <div class="flex items-center justify-between gap-2 px-6 py-5.5 lg:py-6.5">
      <router-link to="/dashboard" class="flex gap-3 items-center">
        <img src="@/assets/logonew.png" class="w-12  block dark:hidden" />
        <img src="@/assets/logowhite.png" class="w-12  hidden dark:block" />
        <span class="md:text-[24px] font-bold text-colorTextBlack dark:text-white">DinnerMax</span>
      </router-link>

      <button class="block lg:hidden text-colorTextBlack dark:text-white" @click="sidebarStore.isSidebarOpen = !sidebarStore.isSidebarOpen">
        <ChevronsLeft />
      </button>
    </div>
    <!-- SIDEBAR HEADER -->

    <div class="no-scrollbar flex flex-col overflow-y-auto duration-300 ease-linear">
      <!-- Sidebar Menu -->
      <nav class="mt-5 py-4 px-4 lg:mt-9 lg:px-6">
        <template v-for="menuGroup in menuGroups" :key="menuGroup.name">
          <div>
            <ul class="mb-6 flex flex-col gap-[22px]">
              <SidebarItem
                  v-for="(menuItem, index) in menuGroup.menuItems"
                  :item="menuItem"
                  :key="index"
                  :index="index"
                  :icon="menuItem.icon"
                  :class="isActive(menuItem.route) ? 'bg-gray-200 dark:bg-colorTextBlack' : ''"
                  @click="sidebarStore.isSidebarOpen = !sidebarStore.isSidebarOpen"
              />
              <a href="https://wa.me/593963620095?text=Hola%2C%20DinnerMax" target="_blank" class="group relative flex items-center gap-5 rounded-sm py-3 px-4 font-medium text-colorTextBlack dark:text-white duration-300 ease-in-out hover:bg-bghoverligth dark:hover:bg-colorTextBlack cursor-pointer">
                <MessageCircleQuestion class="text-current w-5 h-5"/>
                <span>Soporte</span>
              </a>
            </ul>
          </div>
        </template>
      </nav>
    </div>
  </aside>
</template>

<style scoped>

</style>
