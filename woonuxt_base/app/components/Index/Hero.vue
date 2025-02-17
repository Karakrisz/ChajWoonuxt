<script setup>
const slides = ref([
    {
        image: '/img/slider/slider.jpeg',
    },
    {
        image: '/img/slider/slider2.jpeg',
    },
    {
        image: '/img/slider/slider3.jpeg',
    },
])

const currentSlide = ref(0)

const nextSlide = () => {
    currentSlide.value = (currentSlide.value + 1) % slides.value.length
}

const prevSlide = () => {
    currentSlide.value = currentSlide.value === 0
        ? slides.value.length - 1
        : currentSlide.value - 1
}

const goToSlide = (index) => {
    currentSlide.value = index
}

// Automatikus váltás
onMounted(() => {
    const interval = setInterval(nextSlide, 5000)

    onUnmounted(() => {
        clearInterval(interval)
    })
})
</script>
<template>
    <div class="relative w-full h-[600px] overflow-hidden">
        <!-- Slides container -->
        <div class="relative w-full h-full">
            <div v-for="(slide, index) in slides" :key="index"
                class="absolute w-full h-full transition-transform duration-500 ease-in-out"
                :class="{ 'translate-x-0': currentSlide === index, 'translate-x-full': currentSlide < index, '-translate-x-full': currentSlide > index }">
                <!-- Background image -->
                <img :src="slide.image" :alt="slide.title" class="w-full h-full object-cover" />
            </div>
        </div>

        <!-- Navigation buttons -->
        <button @click="prevSlide"
            class="absolute left-4 top-1/2 -translate-y-1/2 w-10 h-10 bg-white/80 rounded-full flex items-center justify-center hover:bg-white transition-colors">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24"
                stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
            </svg>
        </button>

        <button @click="nextSlide"
            class="absolute right-4 top-1/2 -translate-y-1/2 w-10 h-10 bg-white/80 rounded-full flex items-center justify-center hover:bg-white transition-colors">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24"
                stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
            </svg>
        </button>

        <!-- Dots navigation -->
        <div class="absolute bottom-4 left-1/2 -translate-x-1/2 flex gap-2">
            <button v-for="(_, index) in slides" :key="index" @click="goToSlide(index)"
                class="w-3 h-3 rounded-full transition-colors"
                :class="currentSlide === index ? 'bg-white' : 'bg-white/50'"></button>
        </div>
    </div>
</template>