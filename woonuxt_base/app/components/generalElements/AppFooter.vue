<template>
    <footer class="w-full py-6 bg-gray-200">
      <div class="container mx-auto px-4">
        <div class="flex flex-col md:flex-row items-center justify-between gap-4">
          <!-- Logo -->
          <div class="flex items-center">
            <NuxtImg src="/img/logo.svg" alt="Logo" class="h-8 w-auto" />
          </div>
  
          <!-- Basic Links -->
          <nav class="flex gap-6">
            <NuxtLink 
              v-for="link in links" 
              :key="link.text" 
              :to="link.to"
              class="text-sm text-gray-600 hover:text-gray-900"
            >
              {{ link.text }}
            </NuxtLink>
          </nav>
        </div>
      </div>
    </footer>
  </template>
  
  <script setup>
  const links = [
    { text: 'Privacy', to: '/privacy' },
    { text: 'Terms', to: '/terms' },
    { text: 'Contact', to: '/contact' }
  ]
  </script>