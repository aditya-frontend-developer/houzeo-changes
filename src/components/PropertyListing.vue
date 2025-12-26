<template>
  <div class="bg-white rounded-lg overflow-hidden shadow-sm hover:shadow-xl transition-all duration-300 border border-gray-200 hover:border-gray-300">
    
    <div 
      class="relative aspect-[3/2] bg-gray-200 overflow-hidden group max-h-[240px] sm:max-h-[280px] md:max-h-[300px]"
      @mouseenter="showNavigation = true"
      @mouseleave="showNavigation = false"
    >
      
      <div class="relative w-full h-full">
        <div 
          class="flex transition-transform duration-300 ease-in-out h-full"
          :style="{ transform: `translateX(-${currentImageIndex * 100}%)` }"
        >
          <div 
            v-for="(image, index) in propertyImages" 
            :key="index"
            class="w-full h-full flex-shrink-0"
          >
            <img 
              :src="image" 
              :alt="`Property Image ${index + 1}`" 
              class="w-full h-full object-cover"
              :loading="index === 0 ? 'eager' : 'lazy'"
              :data-index="index"
            >
          </div>
        </div>
      </div>

      
      <button
        v-if="showNavigation && currentImageIndex > 0"
        @click="previousImage"
        class="absolute left-2 top-1/2 transform -translate-y-1/2 bg-white/90 backdrop-blur-sm p-2 rounded-full hover:bg-white transition-all opacity-0 group-hover:opacity-100 z-10 shadow-md"
        aria-label="Previous image"
      >
        <svg class="w-5 h-5 text-gray-700" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
        </svg>
      </button>

      <button
        v-if="showNavigation && currentImageIndex < propertyImages.length - 1"
        @click="nextImage"
        class="absolute right-2 top-1/2 transform -translate-y-1/2 bg-white/90 backdrop-blur-sm p-2 rounded-full hover:bg-white transition-all opacity-0 group-hover:opacity-100 z-10 shadow-md"
        aria-label="Next image"
      >
        <svg class="w-5 h-5 text-gray-700" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
        </svg>
      </button>

      
      <div class="absolute top-3 left-3 bg-white/90 backdrop-blur-sm px-2 py-1 rounded text-xs font-medium text-gray-700 z-10">
        {{ daysOnHouzeo }} days on Houzeo
      </div>

       
      <button 
        @click="toggleFavorite"
        class="heart-button absolute top-3 right-3 bg-white/90 backdrop-blur-sm p-2 rounded-full hover:bg-white transition-colors z-10"
        aria-label="Toggle favorite"
      >
        <svg 
          class="w-5 h-5 transition-all duration-200" 
          :class="isFavorite ? 'text-red-500' : 'text-gray-600'"
          :fill="isFavorite ? 'currentColor' : 'none'"
          stroke="currentColor" 
          :stroke-width="isFavorite ? '0' : '2'"
          viewBox="0 0 24 24"
        >
          <path 
            stroke-linecap="round" 
            stroke-linejoin="round" 
            d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z" 
          />
        </svg>
      </button>

      
      <div 
        v-if="showNavigation && propertyImages.length > 1"
        class="absolute bottom-3 left-1/2 transform -translate-x-1/2 flex gap-1.5 opacity-0 group-hover:opacity-100 transition-opacity z-10"
      >
        <button
          v-for="(image, index) in propertyImages"
          :key="index"
          @click="goToImage(index)"
          :class="[
            'w-2 h-2 rounded-full transition-all',
            currentImageIndex === index ? 'bg-blue-600 w-6' : 'bg-white/80 hover:bg-white'
          ]"
          :aria-label="`Go to image ${index + 1}`"
        ></button>
      </div>

      
      <div class="absolute bottom-3 left-3 bg-white/90 backdrop-blur-sm px-2 py-1 rounded text-xs font-medium text-gray-700 z-10">
        TRIANGLE MLS
      </div>
    </div>

    
    <div class="p-3 sm:p-3 md:p-4">
      
      <div class="flex items-center justify-between mb-2">
        <div class="flex items-center gap-2">
          <div class="w-2 h-2 bg-green-500 rounded-full"></div>
          <span class="text-sm text-gray-700 font-medium">{{ propertyType }}</span>
        </div>
        
        <div class="flex items-center gap-1 text-xs text-gray-500">
          <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" />
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M2.458 12C3.732 7.943 7.523 5 12 5c4.478 0 8.268 2.943 9.542 7-1.274 4.057-5.064 7-9.542 7-4.477 0-8.268-2.943-9.542-7z" />
          </svg>
          <span>{{ formatViews(views) }}</span>
        </div>
      </div>

      
      <div class="text-2xl font-bold text-gray-900 mb-2">
        {{ formatPrice(price) }}
      </div>

      
      <div v-if="sqft > 0" class="text-sm text-gray-600 mb-2">
        {{ beds }} Beds {{ baths }} Baths {{ sqft.toLocaleString() }} sqft.
      </div>
      <div v-else class="text-sm text-gray-600 mb-2">
        Land Property
      </div>

      
      <div class="text-sm text-gray-700 mb-2">
        {{ address }}
      </div>

      
      <div class="text-xs text-gray-500">
        {{ brokerage }}
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import propertyImage from '../assets/images/property1.png';
import propertyImage2 from '../assets/images/property2.png';
import propertyImage3 from '../assets/images/property3.png';
import propertyImage4 from '../assets/images/property4.png';

const props = defineProps({
  daysOnHouzeo: {
    type: Number,
    default: 6
  },
  propertyType: {
    type: String,
    default: 'House For Sale'
  },
  price: {
    type: Number,
    default: 3349000
  },
  beds: {
    type: Number,
    default: 4
  },
  baths: {
    type: Number,
    default: 3
  },
  sqft: {
    type: Number,
    default: 998
  },
  address: {
    type: String,
    default: '2856 Meadow Park Ave, Henderson, NV 89052'
  },
  brokerage: {
    type: String,
    default: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID'
  },
  views: {
    type: Number,
    default: 2300
  },
  startImageIndex: {
    type: Number,
    default: 0
  }
})

const propertyImages = [propertyImage, propertyImage2, propertyImage3, propertyImage4]

const currentImageIndex = ref(props.startImageIndex % propertyImages.length)
const showNavigation = ref(false)

const isFavorite = ref(false)

const nextImage = () => {
  if (currentImageIndex.value < propertyImages.length - 1) {
    currentImageIndex.value++
  }
}

const previousImage = () => {
  if (currentImageIndex.value > 0) {
    currentImageIndex.value--
  }
}

const goToImage = (index) => {
  currentImageIndex.value = index
}

const toggleFavorite = () => {
  isFavorite.value = !isFavorite.value
}

const formatPrice = (price) => {
  return new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD',
    minimumFractionDigits: 0,
    maximumFractionDigits: 0
  }).format(price)
}

const formatViews = (views) => {
  if (views >= 1000) {
    return (views / 1000).toFixed(1) + 'K'
  }
  return views.toString()
}
</script>

<style scoped>
.heart-button:hover {
  animation: pulse-heart 1.5s ease-in-out infinite;
}

@keyframes pulse-heart {
  0%, 100% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.1);
  }
}

.heart-button:hover svg {
  animation: pulse-icon 1.5s ease-in-out infinite;
}

@keyframes pulse-icon {
  0%, 100% {
    transform: scale(1);
    opacity: 1;
  }
  50% {
    transform: scale(1.15);
    opacity: 0.9;
  }
}
</style>

