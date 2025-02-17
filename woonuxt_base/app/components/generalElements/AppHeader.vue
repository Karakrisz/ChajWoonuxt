<script setup lang="ts">
const links = [
  { name: 'Rólunk', link: '/', prefetch: true },
  { name: 'Galéria', link: '/' },
  { name: 'Étlap', link: '/', prefetch: true },
  { name: 'Kapcsolat', link: '/' },
];
</script>

<template>
  <header class="z-50 sticky top-0 bg-white">
    <div class="container flex justify-between items-center py-4 gap-10">
      <MenuTrigger class="lg:hidden" />

      <NuxtLink to="/" class="shrink-0">
        <NuxtImg src="/img/logo.svg" class="" />
      </NuxtLink>
      <div class="space-x-16 lg:flex items-center hidden">
        <NuxtLink v-for="link in links" :key="link.name" :to="link.link" class="">{{ link.name }}</NuxtLink>
      </div>
      <div class="flex gap-4">
        <ProductSearch class="hidden md:inline-flex max-w-[320px]" />
        <SignInLink />
        <CartTrigger />
      </div>
    </div>
  </header>
</template>
