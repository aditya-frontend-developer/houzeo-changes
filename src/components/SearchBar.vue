<template>
  <div class="bg-white border-b border-gray-200 py-3 lg:py-4 shadow-sm">
    <div class="max-w-[1920px] mx-auto px-4 sm:px-6 lg:px-8">
      
      <div class="lg:hidden space-y-3">
        <div class="flex items-center gap-2">
          <div class="flex-1 relative flex items-center">
            <input
              type="text"
              v-model="searchQuery"
              placeholder="Search location..."
              class="filter-input w-full pl-4 pr-20 py-2.5 border border-blue-500 rounded-full focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 text-sm transition-colors duration-300"
            />
            <button
              v-if="searchQuery"
              @click="searchQuery = ''"
              class="absolute right-12 top-1/2 transform -translate-y-1/2 text-gray-400 hover:text-gray-600 z-10"
            >
              <img :src="closeIcon" alt="Close" class="w-5 h-5">
            </button>
            <button class="absolute right-1 top-1/2 transform -translate-y-1/2 hover:bg-blue-700 rounded-full p-2 flex items-center justify-center transition-colors">
              <img :src="searchIcon" alt="Search" style="width: 32px; height: 32px;">
            </button>
          </div>
          <button class="relative p-2.5 border border-gray-300 rounded-lg bg-white text-gray-700 hover:bg-gray-50">
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 4a1 1 0 011-1h16a1 1 0 011 1v2.586a1 1 0 01-.293.707l-6.414 6.414a1 1 0 00-.293.707V17l-4 4v-6.586a1 1 0 00-.293-.707L3.293 7.293A1 1 0 013 6.586V4z" />
            </svg>
            <span v-if="activeFilters > 0" class="absolute -top-1 -right-1 bg-blue-600 text-white text-xs rounded-full w-5 h-5 flex items-center justify-center">{{ activeFilters }}</span>
          </button>
        </div>
        
        <div class="flex items-center gap-2">
          <div class="flex-1 relative">
            <select v-model="statusFilter" class="filter-input w-full px-3 py-2 pr-8 border border-gray-300 rounded-lg bg-white text-sm text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-500 cursor-pointer transition-colors duration-300 appearance-none">
              <option value="for-sale">For Sale</option>
              <option value="sold">Sold</option>
            </select>
            <img :src="dropdownIcon" alt="Dropdown" class="absolute right-2 top-1/2 transform -translate-y-1/2 pointer-events-none" style="width: 12px; height: 12px;">
          </div>
          <div class="flex-1 relative">
            <select v-model="priceFilter" class="filter-input w-full px-3 py-2 pr-8 border border-gray-300 rounded-lg bg-white text-sm text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-500 cursor-pointer transition-colors duration-300 appearance-none">
              <option value="">Price</option>
              <option value="under-200k">Under $200,000</option>
              <option value="200k-500k">$200,000 - $500,000</option>
              <option value="500k-1m">$500,000 - $1,000,000</option>
              <option value="over-1m">Over $1,000,000</option>
            </select>
            <img :src="dropdownIcon" alt="Dropdown" class="absolute right-2 top-1/2 transform -translate-y-1/2 pointer-events-none" style="width: 12px; height: 12px;">
          </div>
          <button @click="saveSearch" class="px-4 py-2 bg-blue-600 hover:bg-blue-700 text-white rounded-lg text-sm font-medium transition-colors whitespace-nowrap">
            Save Search
          </button>
        </div>
      </div>

      <div class="hidden lg:flex flex-row items-center gap-4">
        <div class="flex-1 min-w-[300px] max-w-[600px] relative flex items-center">
          <input
            type="text"
            v-model="searchQuery"
            placeholder="Search location..."
            class="filter-input w-full pl-4 pr-20 py-2.5 border border-blue-500 rounded-full focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 text-sm transition-colors duration-300"
          />
          <button
            v-if="searchQuery"
            @click="searchQuery = ''"
            class="absolute right-12 top-1/2 transform -translate-y-1/2 text-gray-400 hover:text-gray-600 z-10"
          >
            <img :src="closeIcon" alt="Close" style="width: 32px; height: 32px;">
          </button>
          <button class="absolute right-1 top-1/2 transform -translate-y-1/2 rounded-full flex items-center justify-center transition-colors">
            <img :src="searchIcon" alt="Search" style="width: 32px; height: 32px;">
          </button>
        </div>

        <div class="flex items-center gap-4 flex-nowrap">
          <div class="relative">
            <select v-model="statusFilter" class="filter-input px-4 py-2.5 pr-8 border border-gray-300 rounded-lg bg-white text-sm text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-500 cursor-pointer transition-colors duration-300 whitespace-nowrap appearance-none">
              <option value="for-sale">For Sale</option>
              <option value="sold">Sold</option>
            </select>
            <img :src="dropdownIcon" alt="Dropdown" class="absolute right-2 top-1/2 transform -translate-y-1/2 pointer-events-none" style="width: 12px; height: 12px;">
          </div>
          <div class="relative">
            <select v-model="priceFilter" class="filter-input px-4 py-2.5 pr-8 border border-gray-300 rounded-lg bg-white text-sm text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-500 cursor-pointer transition-colors duration-300 whitespace-nowrap appearance-none">
              <option value="">Price</option>
              <option value="under-200k">Under $200,000</option>
              <option value="200k-500k">$200,000 - $500,000</option>
              <option value="500k-1m">$500,000 - $1,000,000</option>
              <option value="over-1m">Over $1,000,000</option>
            </select>
            <img :src="dropdownIcon" alt="Dropdown" class="absolute right-2 top-1/2 transform -translate-y-1/2 pointer-events-none" style="width: 12px; height: 12px;">
          </div>
          <div class="relative">
            <select v-model="bedsFilter" class="filter-input px-4 py-2.5 pr-8 border border-gray-300 rounded-lg bg-white text-sm text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-500 cursor-pointer transition-colors duration-300 whitespace-nowrap appearance-none">
              <option value="">Beds & Baths</option>
              <option value="1">1+ Bed</option>
              <option value="2">2+ Beds</option>
              <option value="3">3+ Beds</option>
              <option value="4">4+ Beds</option>
              <option value="5">5+ Beds</option>
            </select>
            <img :src="dropdownIcon" alt="Dropdown" class="absolute right-2 top-1/2 transform -translate-y-1/2 pointer-events-none" style="width: 12px; height: 12px;">
          </div>
          <div class="relative">
            <select v-model="propertyTypeFilter" class="filter-input px-4 py-2.5 pr-8 border border-gray-300 rounded-lg bg-white text-sm text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-500 cursor-pointer transition-colors duration-300 whitespace-nowrap appearance-none">
              <option value="">Property Type</option>
              <option value="House">House</option>
              <option value="Condo">Condo</option>
              <option value="Townhouse">Townhouse</option>
              <option value="Multi-family">Multi-family</option>
              <option value="Land">Land</option>
            </select>
            <img :src="dropdownIcon" alt="Dropdown" class="absolute right-2 top-1/2 transform -translate-y-1/2 pointer-events-none" style="width: 12px; height: 12px;">
          </div>
          <button class="px-4 py-2.5 border border-gray-300 rounded-lg bg-white text-sm text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-blue-500 flex items-center gap-2 whitespace-nowrap">
            <img :src="filterIcon" alt="Filter" style="width: 16px; height: 16px;">
            <span>Filters</span>
          </button>
          <button class="px-4 py-2.5 border border-gray-300 rounded-full bg-white text-sm text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-blue-500 flex items-center gap-2 whitespace-nowrap">
            <img :src="saveIcon" alt="Saved" style="width: 16px; height: 16px;">
            <span>Saved</span>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { inject, computed } from 'vue'
import closeIcon from '../assets/images/close.svg';
import searchIcon from '../assets/images/search.svg';
import filterIcon from '../assets/images/filter.svg';
import saveIcon from '../assets/images/saved.svg';
import dropdownIcon from '../assets/images/drop_down.svg';

const filters = inject('filters', {
  searchQuery: { value: '' },
  statusFilter: { value: 'for-sale' },
  priceFilter: { value: '' },
  bedsFilter: { value: '' },
  propertyTypeFilter: { value: '' }
})

const searchQuery = filters.searchQuery
const statusFilter = filters.statusFilter
const priceFilter = filters.priceFilter
const bedsFilter = filters.bedsFilter
const propertyTypeFilter = filters.propertyTypeFilter

const activeFilters = computed(() => {
  let count = 0
  if (priceFilter.value) count++
  if (bedsFilter.value) count++
  if (propertyTypeFilter.value) count++
  return count
})

const saveSearch = () => {
  console.log('Saving search with filters:', {
    query: searchQuery.value,
    status: statusFilter.value,
    price: priceFilter.value,
    beds: bedsFilter.value,
    propertyType: propertyTypeFilter.value
  })
}
</script>

<style scoped>
.filter-input:hover {
  border-color: #3b82f6;
}

.filter-input:focus {
  border-color: #3b82f6;
}

/* Hide default dropdown arrow for all browsers */
select.filter-input {
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
}

select.filter-input::-ms-expand {
  display: none;
}
</style>

