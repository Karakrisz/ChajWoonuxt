<script setup lang="ts">
const route = useRoute();
const { storeSettings } = useAppConfig();
const { addToCart, isUpdatingCart } = useCart();

const props = defineProps({
  node: { type: Object as PropType<Product>, required: true },
  index: { type: Number, default: 1 },
});

// example: ?filter=pa_color[green,blue],pa_size[large]
const filterQuery = ref(route.query?.filter as string);
const paColor = ref(filterQuery.value?.split('pa_color[')[1]?.split(']')[0]?.split(',') || []);

// watch filterQuery
watch(
  () => route.query,
  () => {
    filterQuery.value = route.query.filter as string;
    paColor.value = filterQuery.value?.split('pa_color[')[1]?.split(']')[0]?.split(',') || [];
  },
);

const mainImage = computed<string>(() => props.node?.image?.producCardSourceUrl || props.node?.image?.sourceUrl || '/images/placeholder.jpg');
const imagetoDisplay = computed<string>(() => {
  if (paColor.value.length) {
    const activeColorImage = props.node?.variations?.nodes.filter((variation) => {
      const hasMatchingAttributes = variation.attributes?.nodes.some((attribute) => paColor.value.some((color) => attribute?.value?.includes(color)));
      const hasMatchingSlug = paColor.value.some((color) => variation.slug?.includes(color));
      return hasMatchingAttributes || hasMatchingSlug;
    });
    if (activeColorImage?.length) return activeColorImage[0]?.image?.producCardSourceUrl || activeColorImage[0]?.image?.sourceUrl || mainImage.value;
  }
  return mainImage.value;
});
</script>

<template>
  <div class="rounded-2xl overflow-hidden flex flex-col bg-white">
    <NuxtLink 
      v-if="node.slug" 
      :to="`/product/${decodeURIComponent(node.slug)}`" 
      :title="node.name" 
      class="w-full h-[300px] relative overflow-hidden grow-0">
      <NuxtImg
        v-if="imagetoDisplay"
        :src="imagetoDisplay"
        :alt="node.image?.altText || node.name || 'Product image'"
        :title="node.image?.title || node.name"
        :loading="index <= 3 ? 'eager' : 'lazy'"
        class="object-cover w-full h-full transition hover:scale-105 duration-300"
        placeholder
        placeholder-class="blur-xl" />
      <SaleBadge :node class="absolute top-2 right-2" />
    </NuxtLink>
    <div class="p-4 flex flex-col gap-2">
      <NuxtLink 
        class="text-xl font-bold uppercase hover:text-stone-700 transition" 
        :to="`/product/${decodeURIComponent(node.slug)}`" 
        :title="node.name">
        {{ node.name }}
      </NuxtLink>
      <!-- <div class="text-gray-600 italic">Artist's name</div> -->
      <ProductPrice class="text-lg font-semibold" :sale-price="node.salePrice" :regular-price="node.regularPrice" />
      <div class="mt-auto">
        <NuxtLink 
          v-if="node.slug" 
          :to="`/product/${decodeURIComponent(node.slug)}`" 
          :title="node.name" 
          class="inline-flex items-center px-6 py-2 border border-black rounded-full hover:bg-black hover:text-white transition-colors mt-2">
          Megrendelés <span class="ml-2">&#8250;</span>
        </NuxtLink>
      </div>
    </div>
  </div>
</template>
