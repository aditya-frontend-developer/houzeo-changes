<template>
  <header class="bg-white border-b border-gray-200 sticky top-0 z-50 shadow-sm">
    <div class="max-w-[1920px] mx-auto px-4 sm:px-6 md:px-6 lg:px-8">

<div class="grid grid-cols-3 items-center h-14 md:h-16 lg:h-20 md:flex md:justify-between">

  <button
    @click="mobileMenuOpen = !mobileMenuOpen"
    class="md:hidden p-2 text-gray-700 justify-self-start"
  >
    <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
        d="M4 6h16M4 12h16M4 18h16" />
    </svg>
  </button>

  <div class="flex justify-center md:justify-start">
    <img
      :src="logoUrl"
      alt="Houzeo Logo"
      class="h-7 md:h-8 lg:h-10 w-auto mx-auto"
    />
  </div>

  <button class="md:hidden p-2 text-gray-700 justify-self-end">
    <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
        d="M16 7a4 4 0 11-8 0 4 4 0 018 0z
           M12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" />
    </svg>
  </button>

  <nav class="hidden md:flex items-center gap-4 lg:gap-6">

    <div
      class="relative"
      @mouseenter="openDropdown = 'product'"
      @mouseleave="openDropdown = null"
    >
      <button class="text-gray-700 hover:text-blue-600 text-sm font-medium flex items-center gap-2">
        Product
        <img src="../../assets/images/drop_down.svg" class="w-3 h-3">
      </button>
      <div v-if="openDropdown === 'product'"
        class="absolute top-full left-0 mt-2 w-48 bg-white rounded-lg shadow-lg border py-2 z-50">
        <a class="block px-4 py-2 text-sm hover:bg-gray-50">Features</a>
        <a class="block px-4 py-2 text-sm hover:bg-gray-50">Pricing Plans</a>
        <a class="block px-4 py-2 text-sm hover:bg-gray-50">Compare Plans</a>
      </div>
    </div>

    <a class="text-gray-700 hover:text-blue-600 text-sm font-medium">How Houzeo Works</a>
    <a class="text-gray-700 hover:text-blue-600 text-sm font-medium">Reviews</a>

    <div
      class="relative"
      @mouseenter="openDropdown = 'buy'"
      @mouseleave="openDropdown = null"
    >
      <button class="text-gray-700 hover:text-blue-600 text-sm font-medium flex items-center gap-2">
        Buy
        <img src="../../assets/images/drop_down.svg" class="w-3 h-3">
      </button>
      <div v-if="openDropdown === 'buy'"
        class="absolute top-full left-0 mt-2 w-48 bg-white rounded-lg shadow-lg border py-2 z-50">
        <a class="block px-4 py-2 text-sm hover:bg-gray-50">Browse Homes</a>
        <a class="block px-4 py-2 text-sm hover:bg-gray-50">Saved Searches</a>
        <a class="block px-4 py-2 text-sm hover:bg-gray-50">Market Trends</a>
        <a class="block px-4 py-2 text-sm hover:bg-gray-50">Buyer's Guide</a>
      </div>
    </div>

    <div
      class="relative"
      @mouseenter="openDropdown = 'sell'"
      @mouseleave="openDropdown = null"
    >
      <button class="text-gray-700 hover:text-blue-600 text-sm font-medium flex items-center gap-2">
        Sell
        <img src="../../assets/images/drop_down.svg" class="w-3 h-3">
      </button>
      <div v-if="openDropdown === 'sell'"
        class="absolute top-full left-0 mt-2 w-48 bg-white rounded-lg shadow-lg border py-2 z-50">
        <a class="block px-4 py-2 text-sm hover:bg-gray-50">List Your Home</a>
        <a class="block px-4 py-2 text-sm hover:bg-gray-50">FSBO Services</a>
        <a class="block px-4 py-2 text-sm hover:bg-gray-50">Seller's Guide</a>
        <a class="block px-4 py-2 text-sm hover:bg-gray-50">Home Valuation</a>
      </div>
    </div>

    <a class="text-gray-700 hover:text-blue-600 text-sm font-medium">Pricing</a>

    <div
      class="relative"
      @mouseenter="openDropdown = 'resources'"
      @mouseleave="openDropdown = null"
    >
      <button class="text-gray-700 hover:text-blue-600 text-sm font-medium flex items-center gap-2">
        Resources
        <img src="../../assets/images/drop_down.svg" class="w-3 h-3">
      </button>
      <div v-if="openDropdown === 'resources'"
        class="absolute top-full left-0 mt-2 w-48 bg-white rounded-lg shadow-lg border py-2 z-50">
        <a class="block px-4 py-2 text-sm hover:bg-gray-50">Blog</a>
        <a class="block px-4 py-2 text-sm hover:bg-gray-50">Help Center</a>
        <a class="block px-4 py-2 text-sm hover:bg-gray-50">Guides & Tips</a>
        <a class="block px-4 py-2 text-sm hover:bg-gray-50">Market Reports</a>
        <a class="block px-4 py-2 text-sm hover:bg-gray-50">Contact Support</a>
      </div>
    </div>

    <a class="text-gray-700 hover:text-blue-600 text-sm font-medium">Sign In</a>

    <div class="hidden lg:block text-sm text-gray-600 ml-4">
      <span>(844) 448-0110</span><br>
      <span class="text-gray-400">(9am to 6pm CST, Mon-Sat)</span>
    </div>

    <button
      class="text-white px-4 py-2 rounded-md text-sm font-medium hover:opacity-90"
      style="background: linear-gradient(270deg, #0E5293 0%, #2876C1 97.2%);"
    >
      Start Free Listing
    </button>

  </nav>
</div>

<div v-if="mobileMenuOpen" class="md:hidden border-t border-gray-200 py-4">
  <nav class="flex flex-col gap-2">
    <a class="text-gray-700 hover:text-blue-600 text-sm font-medium py-2">Product</a>
    <a class="text-gray-700 hover:text-blue-600 text-sm font-medium py-2">How Houzeo Works</a>
    <a class="text-gray-700 hover:text-blue-600 text-sm font-medium py-2">Reviews</a>
    <a class="text-gray-700 hover:text-blue-600 text-sm font-medium py-2">Buy</a>
    <a class="text-gray-700 hover:text-blue-600 text-sm font-medium py-2">Sell</a>
    <a class="text-gray-700 hover:text-blue-600 text-sm font-medium py-2">Pricing</a>
    <a class="text-gray-700 hover:text-blue-600 text-sm font-medium py-2">Resources</a>
    <a class="text-gray-700 hover:text-blue-600 text-sm font-medium py-2">Sign In</a>
  </nav>
</div>

</div>

  </header>
</template>

<script setup>
import { ref } from 'vue'
import logoUrl from '../../assets/images/logo/houzeo_log.svg'

const mobileMenuOpen = ref(false)
const openDropdown = ref(null)
const mobileDropdown = ref(null)
</script>

