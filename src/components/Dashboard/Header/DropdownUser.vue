<script setup>
import { onClickOutside } from '@vueuse/core'
import { ref, onMounted, onUnmounted } from 'vue'
import { ChevronDown, CircleUserRound, LogOut, Loader2, UserCircle2 } from 'lucide-vue-next'
import { authService } from '@/services/auth_service'
import { userService } from '@/services/user_service'
import { useRouter } from 'vue-router'
import { logError } from '@/utils/logger'

const router = useRouter()
const target = ref(null)
const dropdownOpen = ref(false)
const isLoading = ref(false)
const imageLoading = ref(true)
const imageError = ref(false)
const userData = ref({
  nombre: '',
  email: '',
  photoURL: null
})

let unsubscribe = null

onMounted(() => {
  unsubscribe = userService.subscribeToUser((response) => {
    if (response.success) {
      userData.value = response.data
    } else {
      logError('Error al obtener datos del usuario:', response.error)
    }
  })
})

onUnmounted(() => {
  if (unsubscribe) {
    unsubscribe()
  }
})

onClickOutside(target, () => {
  dropdownOpen.value = false
})

const handleLogout = async () => {
  isLoading.value = true
  try {
    const result = await authService.logout()
    if (result.success) {
      if (unsubscribe) {
        unsubscribe()
      }
      router.push('/login')
    } else {
      throw new Error(result.error)
    }
  } catch (error) {
    logError('Error al cerrar sesión:', error)
  } finally {
    isLoading.value = false
    dropdownOpen.value = false
  }
}

const handleImageLoad = () => {
  imageLoading.value = false
}

const handleImageError = () => {
  imageLoading.value = false
  imageError.value = true
}
</script>

<template>
  <div class="relative" ref="target">
    <router-link
        class="flex items-center gap-4"
        to="#"
        @click.prevent="dropdownOpen = !dropdownOpen"
    >
      <span class="hidden text-right lg:block">
        <span class="block text-sm font-medium text-colorTextBlack dark:text-white">
          {{ userData.nombre }}
        </span>
        <span class="block text-xs font-medium text-colorTextBlack dark:text-white">
          {{ userData.email }}
        </span>
      </span>

      <span class="w-10 h-10 md:h-12 md:w-12 rounded-full flex items-center justify-center bg-gray-100 dark:bg-gray-800 overflow-hidden">
        <template v-if="userData.photoURL">
          <Loader2 v-if="imageLoading" class="animate-spin w-6 h-6 text-primary" />
          <UserCircle2 v-else-if="imageError" class="w-full h-full text-gray-400" />
          <img
              v-show="!imageLoading && !imageError"
              :src="userData.photoURL"
              alt="User"
              class="w-full h-full object-cover"
              @load="handleImageLoad"
              @error="handleImageError"
          />
        </template>
        <UserCircle2 v-else class="w-full h-full text-gray-400" />
      </span>

      <ChevronDown class="text-colorTextBlack dark:text-white hidden md:block" />
    </router-link>

    <div
        v-show="dropdownOpen"
        class="divide-y divide-gray-100 md:divide-y-0 shadow-lg border-[1px] md:border-[3px] w-48 md:w-auto absolute py-[12px]  right-0 mt-4 flex w-62.5 flex-col rounded-[16px] bg-white dark:bg-bgDashboardDark"
    >
      <div class=" px-4 flex items-center  pb-[12px] w-full  gap-2 md:hidden">
        <div class="flex-shrink-0"  v-if="userData.photoURL">
          <Loader2 v-if="imageLoading" class="animate-spin w-6 h-6 text-primary" />
          <UserCircle2 v-else-if="imageError" class="w-full h-full text-gray-400" />
          <img
              v-show="!imageLoading && !imageError"
              :src="userData.photoURL"
              alt="User"
              class="w-10 h-10 rounded-full object-cover"
              @load="handleImageLoad"
              @error="handleImageError"
          />
        </div>
        <UserCircle2 v-else class="flex-shrink-0 w-10 h-10 text-gray-400" />
       <div class="flex flex-col flex-1 min-w-0 ms-1 ">
         <span class="block font-semibold text-[16px] text-gray-900 dark:text-white truncate"> {{ userData.nombre }}</span>
         <span class="block text-[10px]  text-gray-500 truncate dark:text-gray-400"> {{ userData.email }}</span>
       </div>
      </div>
      <ul class="flex pt-2 px-4 flex-col">
        <li>
          <router-link
              @click="dropdownOpen = false"
              to="/dashboard/profileuser"
              class="text-colorTextBlack dark:text-white flex px-2 py-4 md:px-6 items-center gap-3.5 text-sm font-medium duration-300 ease-in-out hover:text-primary lg:text-base hover:bg-bghoverligth dark:hover:bg-colorTextBlack"
          >
            <CircleUserRound />
            Perfil
          </router-link>
        </li>
        <li>
          <button
              @click="handleLogout"
              :disabled="isLoading"
              class="text-colorTextBlack w-full dark:text-white flex items-center gap-3.5 py-4 px-2 md:px-6 text-sm font-medium duration-300 ease-in-out hover:text-primary lg:text-base hover:bg-bghoverligth dark:hover:bg-red-900"
          >
            <Loader2 v-if="isLoading" class="animate-spin" />
            <LogOut v-else />
            Cerrar sesión
          </button>
        </li>
      </ul>

    </div>
  </div>
</template>

<style scoped>

</style>