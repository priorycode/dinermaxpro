<script setup>
import { CircleCheck } from "lucide-vue-next";

const props = defineProps({
  planName: {
    type: String,
    required: true,
  },
  priceRange: {
    type: String,
    required: true,
  },
  duration: {
    type: String,
    required: true,
  },
  features: {
    type: Array,
    required: true,
  },
  isMostPopular: {
    type: Boolean,
    default: false,
  },
});
</script>

<template>
  <article
      :class="[
      'flex flex-col  dark:bg-black border-[8px] border-colorPurpleCard rounded-[32px] py-8 px-6',
      { 'bg-colorBgPopular dark:bg-colorBgPopular': isMostPopular },
    ]"
  >
    <header class="relative">
      <span :class="[
          'font-bold text-colorTextBlack dark:text-white text-[16px]',
          {'text-white dark:text-white': isMostPopular },
      ]">
        {{ planName }}
      </span>
      <h3 :class="[
        'font-black text-colorTextBlack dark:text-white text-[27px] mt-4',
        {'text-white dark:text-white': isMostPopular },
      ]">
        {{ priceRange }}
      </h3>
      <p :class="[
          'text-colorTextBlack dark:text-colorPurpleCard mt-2 text-[12px] font-bold',
          {'text-white dark:text-white': isMostPopular },
      ]">{{ duration }}</p>
      <div
          v-if="isMostPopular"
          class="absolute top-0 right-0 py-1 px-2 rounded-[8px] text-[#181059] font-semibold bg-white"
      >
        Más Popular
      </div>
    </header>
    <ul
        :class="[
            'mt-8 text-colorTextBlack dark:text-[#D2D7D9] text-[14px] leading-5 flex flex-col gap-3',
            {'text-white dark:text-white': isMostPopular },
        ]"
    >
      <li v-for="(feature, index) in features" :key="index" class="flex items-start gap-2">
        <div class="flex-shrink-0">
          <CircleCheck :class="[
              'w-5 text-colorBlue',
              {'text-colorBlue dark:text-white': isMostPopular },
          ]" />
        </div>
        <div>
          {{ feature }}
        </div>
      </li>
    </ul>
    <router-link
        to="/login"
        class="rounded-[8px] mt-7 bg-colorBgButton w-full text-center py-3 text-[16px] text-white font-semibold"
    >
      Elegir plan
    </router-link>
  </article>
</template>

<style scoped></style>