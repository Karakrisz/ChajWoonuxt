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
const touchStartX = ref(0)
const touchEndX = ref(0)

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

// Touch események kezelése
const handleTouchStart = (e) => {
    touchStartX.value = e.touches[0].clientX
}

const handleTouchEnd = (e) => {
    touchEndX.value = e.changedTouches[0].clientX
    handleSwipe()
}

const handleSwipe = () => {
    const swipeThreshold = 50
    const diff = touchStartX.value - touchEndX.value

    if (Math.abs(diff) > swipeThreshold) {
        if (diff > 0) {
            nextSlide()
        } else {
            prevSlide()
        }
    }
}

// Keyboard navigáció
const handleKeyDown = (e) => {
    if (e.key === 'ArrowLeft') {
        prevSlide()
    } else if (e.key === 'ArrowRight') {
        nextSlide()
    }
}

onMounted(() => {
    const interval = setInterval(nextSlide, 5000)
    window.addEventListener('keydown', handleKeyDown)

    onUnmounted(() => {
        clearInterval(interval)
        window.removeEventListener('keydown', handleKeyDown)
    })
})
</script>
<template>
    <section>
        <div class="relative w-full h-[600px] overflow-hidden" @touchstart="handleTouchStart" @touchend="handleTouchEnd"
            role="region" aria-label="Image slider">
            <!-- Slides container -->
            <div class="relative w-full h-full">
                <div v-for="(slide, index) in slides" :key="index"
                    class="absolute w-full h-full transition-transform duration-500 ease-in-out" :class="{
                        'translate-x-0': currentSlide === index,
                        'translate-x-full': currentSlide < index,
                        '-translate-x-full': currentSlide > index
                    }" role="group" :aria-label="'Slide ' + (index + 1) + ' of ' + slides.length"
                    :aria-hidden="currentSlide !== index">
                    <!-- Background image -->
                    <img :src="slide.image" :alt="'Slide ' + (index + 1)" class="w-full h-full object-cover" />
                </div>
            </div>

            <!-- Navigation buttons -->
            <button @click="prevSlide"
                class="absolute left-4 top-1/2 -translate-y-1/2 w-10 h-10 bg-white/80 rounded-full flex items-center justify-center hover:bg-white transition-colors focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-white"
                aria-label="Previous slide">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24"
                    stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
                </svg>
            </button>

            <button @click="nextSlide"
                class="absolute right-4 top-1/2 -translate-y-1/2 w-10 h-10 bg-white/80 rounded-full flex items-center justify-center hover:bg-white transition-colors focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-white"
                aria-label="Next slide">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24"
                    stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
                </svg>
            </button>

            <!-- Dots navigation -->
            <div class="absolute bottom-4 left-1/2 -translate-x-1/2 flex gap-2" role="tablist">
                <button v-for="(_, index) in slides" :key="index" @click="goToSlide(index)"
                    class="w-3 h-3 rounded-full transition-colors focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-white"
                    :class="currentSlide === index ? 'bg-white' : 'bg-white/50'" role="tab"
                    :aria-selected="currentSlide === index" :aria-label="'Go to slide ' + (index + 1)"></button>
            </div>
        </div>
    </section>
</template>