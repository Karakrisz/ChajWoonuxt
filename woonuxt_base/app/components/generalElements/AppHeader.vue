<script setup lang="ts">
const links = [
  { name: 'Kezdőlap', link: '/' },
  { name: 'Rólunk', link: '/', prefetch: true },
  { name: 'Galéria', link: '/' },
  { name: 'Étlap', link: '/', prefetch: true },
  { name: 'Kapcsolat', link: '/' },
];
</script>

<template>
  <header class="z-50 top-0 bg-white">
    <div class="container flex justify-between items-center py-4 gap-10 menu-content--format">
      <MenuTrigger class="lg:hidden" />

      <NuxtLink to="/" class="shrink-0">
        <NuxtImg src="/img/header/logo.png" class="logo" />
      </NuxtLink>
      <div class="menu">
        <div class="space-x-16 lg:flex items-center hidden">
          <NuxtLink v-for="link in links" :key="link.name" :to="link.link" class="menu-link">{{ link.name }}</NuxtLink>
        </div>
        <div class="flex gap-4 cart-user-box">
          <!-- <ProductSearch class="hidden md:inline-flex max-w-[320px]" /> -->
          <SignInLink />
          <CartTrigger />
        </div>
      </div>
    </div>
  </header>
</template>
