<script setup>
import Header from "@/components/Inicio/Header.vue";
import Hero from "@/components/Inicio/Hero.vue";
import About from "@/components/Inicio/About.vue";
import Planes from "@/components/Inicio/Planes.vue";
import CircleDecoration from "@/components/Inicio/Shared/CircleDecoration.vue";
import StarsDecoration from "@/components/Inicio/Shared/StarsDecoration.vue";
import Simulador from "@/components/Inicio/Simulador.vue";
import EmailMarketing from "@/components/Inicio/EmailMarketing.vue";
import Footer from "@/components/Inicio/Footer.vue";
</script>

<template>
  <div class="relative bg-white dark:bg-colorBgBlack pb-10 overflow-x-hidden">
    <CircleDecoration class="hidden dark:block absolute top-0 z-0" />
    <StarsDecoration class="hidden dark:block absolute top-0 z-0" />
    <StarsDecoration class="hidden dark:block absolute top-0 -left-[450px] z-0" />
    <StarsDecoration class="hidden dark:block absolute top-0 -right-[450px] z-0" />
    <main class="relative z-10">
      <Header />
      <Hero />
      <About />
      <Planes />
      <Simulador/>
      <EmailMarketing/>
      <Footer/>
    </main>
  </div>
</template>

<style scoped>

</style>
