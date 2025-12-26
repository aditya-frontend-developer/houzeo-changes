<template>
  <div class="w-full h-full bg-gray-50 relative overflow-hidden">
    <l-map
      ref="map"
      :zoom="zoom"
      :center="center"
      class="w-full h-full"
    >
      <l-tile-layer
        url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
        attribution='&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
        layer-type="base"
        name="OpenStreetMap"
      />
      
      <l-marker
        v-for="(marker, index) in markers"
        :key="index"
        :lat-lng="marker.position"
      >
        <l-popup>
          <div class="text-sm min-w-[200px]">
            <div class="font-semibold text-gray-900 mb-1">{{ marker.address }}</div>
            <div class="text-lg font-bold text-blue-600 mb-1">{{ marker.price }}</div>
            <div class="text-xs text-gray-500">{{ marker.property.beds }} Beds · {{ marker.property.baths }} Baths · {{ marker.property.sqft.toLocaleString() }} sqft</div>
          </div>
        </l-popup>
      </l-marker>
    </l-map>

    <div class="absolute bottom-4 left-4 bg-white/95 backdrop-blur-sm p-3 rounded-lg shadow-lg border border-gray-200 z-[1000]">
      <div class="text-xs font-semibold text-gray-700 mb-2">Property Locations</div>
      <div class="flex flex-col gap-1">
        <div v-if="markers.length > 0" class="flex items-center gap-1">
          <div class="w-3 h-3 rounded-full bg-red-500"></div>
          <span class="text-xs text-gray-600">{{ markers.length }} {{ markers.length === 1 ? 'Property' : 'Properties' }}</span>
        </div>
        <div v-else class="flex items-center gap-1">
          <div class="w-3 h-3 rounded-full bg-gray-400"></div>
          <span class="text-xs text-gray-500">No properties found</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'
import { LMap, LTileLayer, LMarker, LPopup } from '@vue-leaflet/vue-leaflet'
import L from 'leaflet'

import icon from 'leaflet/dist/images/marker-icon.png'
import iconShadow from 'leaflet/dist/images/marker-shadow.png'

let DefaultIcon = L.icon({
  iconUrl: icon,
  shadowUrl: iconShadow,
  iconSize: [25, 41],
  iconAnchor: [12, 41],
  popupAnchor: [0, -41],
  shadowSize: [41, 41],
  shadowAnchor: [12, 41]
})

L.Marker.prototype.options.icon = DefaultIcon

const props = defineProps({
  properties: {
    type: Array,
    default: () => []
  }
})

const cityCoordinates = {
  'Austin': [30.2672, -97.7431],
  'Henderson': [36.0395, -114.9817],
  'Nashville': [36.1627, -86.7816],
  'Los Angeles': [34.0522, -118.2437],
  'Miami': [25.7617, -80.1918],
  'Seattle': [47.6062, -122.3321],
  'Denver': [39.7392, -104.9903],
  'New York': [40.7128, -74.0060],
  'Chicago': [41.8781, -87.6298],
  'Beverly Hills': [34.0736, -118.4004],
  'San Francisco': [37.7749, -122.4194],
  'Boston': [42.3601, -71.0589],
  'Portland': [45.5152, -122.6784],
  'San Diego': [32.7157, -117.1611],
  'Charleston': [32.7765, -79.9311],
  'Savannah': [32.0809, -81.0912],
  'Philadelphia': [39.9526, -75.1652],
  'Washington': [38.9072, -77.0369],
  'Baltimore': [39.2904, -76.6122],
  'Atlanta': [33.7490, -84.3880],
  'Dallas': [32.7767, -96.7970],
  'Phoenix': [33.4484, -112.0740],
  'Houston': [29.7604, -95.3698],
  'Las Vegas': [36.1699, -115.1398],
  'Orlando': [28.5383, -81.3792],
  'Aspen': [39.1911, -106.8175],
  'Lake Tahoe': [39.0968, -120.0324],
  'Napa Valley': [38.2975, -122.2869],
  'Scottsdale': [33.4942, -111.9211],
  'Malibu': [34.0259, -118.7798],
  'Jackson Hole': [43.4799, -110.7624],
  'Detroit': [42.3314, -83.0458],
  'Minneapolis': [44.9778, -93.2650],
  'Milwaukee': [43.0389, -87.9065],
  'Cleveland': [41.4993, -81.6944],
  'Cincinnati': [39.1031, -84.5120],
  'Indianapolis': [39.7684, -86.1581],
  'Columbus': [39.9612, -82.9988],
  'Kansas City': [39.0997, -94.5786],
  'St. Louis': [38.6270, -90.1994],
  'Memphis': [35.1495, -90.0490],
  'New Orleans': [29.9511, -90.0715],
  'Tampa': [27.9506, -82.4572],
  'Jacksonville': [30.3322, -81.6557],
  'Charlotte': [35.2271, -80.8431],
  'Raleigh': [35.7796, -78.6382],
  'Richmond': [37.5407, -77.4360],
  'Virginia Beach': [36.8529, -75.9780],
  'Norfolk': [36.8468, -76.2852],
  'Buffalo': [42.8864, -78.8784],
  'Rochester': [43.1566, -77.6088],
  'Albany': [42.6526, -73.7562],
  'Hartford': [41.7658, -72.6734],
  'Providence': [41.8240, -71.4128],
  'Worcester': [42.2626, -71.8023],
  'Springfield': [42.1015, -72.5898],
  'Pittsburgh': [40.4406, -79.9959],
  'Allentown': [40.6084, -75.4902],
  'Trenton': [40.2206, -74.7597],
  'Newark': [40.7357, -74.1724],
  'Jersey City': [40.7178, -74.0431],
  'Bridgeport': [41.1865, -73.1952],
  'Stamford': [41.0534, -73.5387],
  'New Haven': [41.3083, -72.9279],
  'Sacramento': [38.5816, -121.4944],
  'Fresno': [36.7378, -119.7871],
  'Oakland': [37.8044, -122.2712],
  'San Jose': [37.3382, -121.8863],
  'Santa Ana': [33.7455, -117.8677],
  'Anaheim': [33.8353, -117.9145],
  'Riverside': [33.9533, -117.3962],
  'Long Beach': [33.7701, -118.1937],
  'Glendale': [34.1425, -118.2551],
  'Bakersfield': [35.3733, -119.0187],
  'Stockton': [37.9577, -121.2908],
  'Modesto': [37.6391, -120.9969],
  'Santa Barbara': [34.4208, -119.6982],
  'Santa Monica': [34.0195, -118.4912],
  'Pasadena': [34.1478, -118.1445],
  'Irvine': [33.6846, -117.8265],
  'San Bernardino': [34.1083, -117.2898],
  'Ontario': [34.0633, -117.6509],
  'Corona': [33.8753, -117.5664],
  'Tucson': [32.2226, -110.9747],
  'Mesa': [33.4152, -111.8315],
  'Chandler': [33.3062, -111.8413],
  'Tempe': [33.4255, -111.9400],
  'Glendale': [33.5387, -112.1860],
  'Scottsdale': [33.4942, -111.9211],
  'Peoria': [33.5806, -112.2374],
  'Surprise': [33.6292, -112.3679],
  'Yuma': [32.6927, -114.6277],
  'Flagstaff': [35.1983, -111.6513],
  'Albuquerque': [35.0844, -106.6504],
  'Santa Fe': [35.6870, -105.9378],
  'Las Cruces': [32.3199, -106.7637],
  'El Paso': [31.7619, -106.4850],
  'Lubbock': [33.5779, -101.8552],
  'Amarillo': [35.2220, -101.8313],
  'Oklahoma City': [35.4676, -97.5164],
  'Tulsa': [36.1540, -95.9928],
  'Wichita': [37.6872, -97.3301],
  'Omaha': [41.2565, -95.9345],
  'Lincoln': [40.8136, -96.7026],
  'Des Moines': [41.5868, -93.6250],
  'Cedar Rapids': [41.9778, -91.6656],
  'Davenport': [41.5236, -90.5776],
  'Sioux Falls': [43.5446, -96.7311],
  'Fargo': [46.8772, -96.7898],
  'Bismarck': [46.8083, -100.7837],
  'Billings': [45.7833, -108.5007],
  'Boise': [43.6150, -116.2023],
  'Spokane': [47.6588, -117.4260],
  'Tacoma': [47.2529, -122.4443],
  'Vancouver': [45.6387, -122.6615],
  'Eugene': [44.0521, -123.0868],
  'Salem': [44.9429, -123.0351],
  'Bend': [44.0582, -121.3153],
  'Reno': [39.5296, -119.8138],
  'Carson City': [39.1638, -119.7674],
  'Salt Lake City': [40.7608, -111.8910],
  'Provo': [40.2338, -111.6585],
  'Ogden': [41.2230, -111.9738],
  'Colorado Springs': [38.8339, -104.8214],
  'Aurora': [39.7294, -104.8319],
  'Fort Collins': [40.5853, -105.0844],
  'Boulder': [40.0150, -105.2705],
  'Pueblo': [38.2544, -104.6091],
  'Grand Junction': [39.0639, -108.5506],
  'Cheyenne': [41.1400, -104.8197],
  'Casper': [42.8666, -106.3131],
  'Rapid City': [43.0755, -103.2020],
  'Pierre': [44.3683, -100.3504],
  'Minot': [48.2325, -101.2963],
  'Grand Forks': [47.9253, -97.0329],
  'Duluth': [46.7867, -92.1005],
  'Rochester': [44.0216, -92.4699],
  'St. Paul': [44.9537, -93.0900],
  'Madison': [43.0731, -89.4012],
  'Green Bay': [44.5192, -88.0198],
  'Milwaukee': [43.0389, -87.9065],
  'Kenosha': [42.5847, -87.8212],
  'Rockford': [42.2711, -89.0940],
  'Peoria': [40.6936, -89.5890],
  'Springfield': [39.7817, -89.6501],
  'Bloomington': [40.4842, -88.9937],
  'Champaign': [40.1164, -88.2434],
  'Evansville': [37.9748, -87.5558],
  'Fort Wayne': [41.0793, -85.1394],
  'South Bend': [41.6764, -86.2520],
  'Gary': [41.5934, -87.3464],
  'Toledo': [41.6528, -83.5379],
  'Akron': [41.0814, -81.5190],
  'Canton': [40.7989, -81.3784],
  'Youngstown': [41.0998, -80.6495],
  'Dayton': [39.7589, -84.1916],
  'Springfield': [39.9242, -83.8088],
  'Lima': [40.7426, -84.1052],
  'Mansfield': [40.7584, -82.5154],
  'Erie': [42.1292, -80.0851],
  'Reading': [40.3356, -75.9269],
  'Scranton': [41.4090, -75.6624],
  'Wilkes-Barre': [41.2459, -75.8813],
  'Allentown': [40.6084, -75.4902],
  'Bethlehem': [40.6259, -75.3705],
  'Lancaster': [40.0379, -76.3055],
  'York': [39.9626, -76.7277],
  'Harrisburg': [40.2737, -76.8844],
  'Altoona': [40.5187, -78.3947],
  'Johnstown': [40.3267, -78.9217],
  'State College': [40.7934, -77.8600],
  'Williamsport': [41.2412, -77.0011],
  'Erie': [42.1292, -80.0851],
  'Pittsburgh': [40.4406, -79.9959],
  'Wheeling': [40.0640, -80.7209],
  'Charleston': [38.3498, -81.6326],
  'Huntington': [38.4192, -82.4452],
  'Parkersburg': [39.2667, -81.5615],
  'Morgantown': [39.6295, -79.9559],
  'Martinsburg': [39.4562, -77.9639],
  'Hagerstown': [39.6418, -77.7200],
  'Frederick': [39.4143, -77.4105],
  'Gaithersburg': [39.1434, -77.2014],
  'Rockville': [39.0840, -77.1528],
  'Bethesda': [38.9847, -77.0947],
  'Silver Spring': [38.9907, -77.0261],
  'College Park': [38.9807, -76.9369],
  'Annapolis': [38.9784, -76.4922],
  'Salisbury': [38.3607, -75.5994],
  'Ocean City': [38.3365, -75.0849],
  'Dover': [39.1582, -75.5244],
  'Wilmington': [39.7391, -75.5398],
  'Newark': [39.6837, -75.7497],
  'Middletown': [39.4496, -75.7163],
  'Smyrna': [39.2998, -75.6046],
  'Milford': [38.9126, -75.4280],
  'Georgetown': [38.6901, -75.3855],
  'Lewes': [38.7746, -75.1394],
  'Rehoboth Beach': [38.7210, -75.0760],
  'Bethany Beach': [38.5396, -75.0552],
  'Fenwick Island': [38.4604, -75.0513],
  'Ocean View': [38.5451, -75.0896],
  'Selbyville': [38.4604, -75.2207],
  'Laurel': [38.5565, -75.5713],
  'Seaford': [38.6412, -75.6110],
  'Bridgeville': [38.7432, -75.6044],
  'Harrington': [38.9237, -75.5777],
  'Camden': [39.1134, -75.5463],
  'Felton': [39.0084, -75.5778],
  'Frederica': [39.0090, -75.4663],
  'Kent County': [39.2000, -75.5000],
  'New Castle County': [39.7000, -75.6000],
  'Sussex County': [38.7000, -75.4000]
}

const geocodeAddress = (property) => {
  if (property.lat && property.lng) {
    return [property.lat, property.lng]
  }
  
  const baseCoords = cityCoordinates[property.city] || [30.2672, -97.7431]
  
  const streetMatch = property.address.match(/^(\d+)/)
  const streetNumber = streetMatch ? parseInt(streetMatch[1]) : 1000
  
  const addressHash = property.address.split('').reduce((acc, char) => {
    return ((acc << 5) - acc) + char.charCodeAt(0)
  }, 0)
  
  const citySpread = {
    'Austin': 0.15,
    'Los Angeles': 0.3,
    'New York': 0.2,
    'Chicago': 0.25,
    'Miami': 0.2,
    'Seattle': 0.2,
    'San Francisco': 0.15,
    'Houston': 0.3,
    'Dallas': 0.25,
    'Phoenix': 0.25,
    'Atlanta': 0.2,
    'Boston': 0.15,
    'Philadelphia': 0.15,
    'Washington': 0.15,
    'Nashville': 0.15,
    'Denver': 0.2,
    'Portland': 0.15,
    'San Diego': 0.2,
    'Las Vegas': 0.2,
    'Orlando': 0.15,
    'Henderson': 0.1,
    'Beverly Hills': 0.05,
    'Malibu': 0.05,
    'Charleston': 0.1,
    'Savannah': 0.1,
    'Baltimore': 0.15,
    'Scottsdale': 0.15,
    'Aspen': 0.1,
    'Lake Tahoe': 0.1,
    'Napa Valley': 0.1,
    'Jackson Hole': 0.1,
    'Detroit': 0.25,
    'Minneapolis': 0.2,
    'Milwaukee': 0.15,
    'Cleveland': 0.2,
    'Cincinnati': 0.15,
    'Indianapolis': 0.2,
    'Columbus': 0.2,
    'Kansas City': 0.2,
    'St. Louis': 0.2,
    'Memphis': 0.15,
    'New Orleans': 0.2,
    'Tampa': 0.2,
    'Jacksonville': 0.25,
    'Charlotte': 0.2,
    'Raleigh': 0.15,
    'Richmond': 0.15,
    'Virginia Beach': 0.15,
    'Norfolk': 0.15,
    'Buffalo': 0.15,
    'Rochester': 0.15,
    'Albany': 0.1,
    'Hartford': 0.15,
    'Providence': 0.15,
    'Worcester': 0.1,
    'Springfield': 0.1,
    'Pittsburgh': 0.2,
    'Allentown': 0.1,
    'Trenton': 0.1,
    'Newark': 0.15,
    'Jersey City': 0.1,
    'Bridgeport': 0.1,
    'Stamford': 0.1,
    'New Haven': 0.1,
    'Sacramento': 0.2,
    'Fresno': 0.15,
    'Oakland': 0.15,
    'San Jose': 0.2,
    'Santa Ana': 0.1,
    'Anaheim': 0.1,
    'Riverside': 0.15,
    'Long Beach': 0.1,
    'Glendale': 0.1,
    'Bakersfield': 0.15,
    'Stockton': 0.1,
    'Modesto': 0.1,
    'Santa Barbara': 0.1,
    'Santa Monica': 0.05,
    'Pasadena': 0.1,
    'Irvine': 0.1,
    'San Bernardino': 0.15,
    'Ontario': 0.1,
    'Corona': 0.1,
    'Tucson': 0.2,
    'Mesa': 0.15,
    'Chandler': 0.1,
    'Tempe': 0.1,
    'Peoria': 0.1,
    'Surprise': 0.1,
    'Yuma': 0.1,
    'Flagstaff': 0.1,
    'Albuquerque': 0.2,
    'Santa Fe': 0.1,
    'Las Cruces': 0.1,
    'El Paso': 0.2,
    'Lubbock': 0.15,
    'Amarillo': 0.15,
    'Oklahoma City': 0.25,
    'Tulsa': 0.2,
    'Wichita': 0.15,
    'Omaha': 0.2,
    'Lincoln': 0.1,
    'Des Moines': 0.15,
    'Cedar Rapids': 0.1,
    'Davenport': 0.1,
    'Sioux Falls': 0.1,
    'Fargo': 0.1,
    'Bismarck': 0.1,
    'Billings': 0.1,
    'Boise': 0.15,
    'Spokane': 0.15,
    'Tacoma': 0.15,
    'Vancouver': 0.1,
    'Eugene': 0.1,
    'Salem': 0.1,
    'Bend': 0.1,
    'Reno': 0.15,
    'Carson City': 0.1,
    'Salt Lake City': 0.2,
    'Provo': 0.1,
    'Ogden': 0.1,
    'Colorado Springs': 0.2,
    'Aurora': 0.15,
    'Fort Collins': 0.1,
    'Boulder': 0.1,
    'Pueblo': 0.1,
    'Grand Junction': 0.1,
    'Cheyenne': 0.1,
    'Casper': 0.1,
    'Rapid City': 0.1,
    'Pierre': 0.05,
    'Minot': 0.1,
    'Grand Forks': 0.1,
    'Duluth': 0.1,
    'St. Paul': 0.15,
    'Madison': 0.15,
    'Green Bay': 0.1,
    'Kenosha': 0.1,
    'Rockford': 0.1,
    'Peoria': 0.1,
    'Springfield': 0.1,
    'Bloomington': 0.1,
    'Champaign': 0.1,
    'Evansville': 0.1,
    'Fort Wayne': 0.15,
    'South Bend': 0.1,
    'Gary': 0.1,
    'Toledo': 0.15,
    'Akron': 0.15,
    'Canton': 0.1,
    'Youngstown': 0.1,
    'Dayton': 0.15,
    'Lima': 0.1,
    'Mansfield': 0.1,
    'Erie': 0.1,
    'Reading': 0.1,
    'Scranton': 0.1,
    'Wilkes-Barre': 0.1,
    'Bethlehem': 0.1,
    'Lancaster': 0.1,
    'York': 0.1,
    'Harrisburg': 0.1,
    'Altoona': 0.1,
    'Johnstown': 0.1,
    'State College': 0.1,
    'Williamsport': 0.1,
    'Wheeling': 0.1,
    'Huntington': 0.1,
    'Parkersburg': 0.1,
    'Morgantown': 0.1,
    'Martinsburg': 0.1,
    'Hagerstown': 0.1,
    'Frederick': 0.1,
    'Gaithersburg': 0.1,
    'Rockville': 0.1,
    'Bethesda': 0.05,
    'Silver Spring': 0.1,
    'College Park': 0.1,
    'Annapolis': 0.1,
    'Salisbury': 0.1,
    'Ocean City': 0.05,
    'Dover': 0.1,
    'Wilmington': 0.15,
    'Middletown': 0.1,
    'Smyrna': 0.05,
    'Milford': 0.05,
    'Georgetown': 0.05,
    'Lewes': 0.05,
    'Rehoboth Beach': 0.05,
    'Bethany Beach': 0.05,
    'Fenwick Island': 0.05,
    'Ocean View': 0.05,
    'Selbyville': 0.05,
    'Laurel': 0.05,
    'Seaford': 0.05,
    'Bridgeville': 0.05,
    'Harrington': 0.05,
    'Camden': 0.05,
    'Felton': 0.05,
    'Frederica': 0.05,
    'Kent County': 0.1,
    'New Castle County': 0.15,
    'Sussex County': 0.1
  }
  
  const spread = citySpread[property.city] || 0.15
  
  const latOffset = (Math.sin(addressHash) * spread)
  const lngOffset = (Math.cos(addressHash) * spread)
  
  const streetVariation = (streetNumber % 100) / 10000
  
  return [
    baseCoords[0] + latOffset + streetVariation,
    baseCoords[1] + lngOffset + streetVariation
  ]
}

const markers = computed(() => {
  return props.properties.map((property) => {
    const position = geocodeAddress(property)
    
    return {
      position,
      address: property.address,
      price: new Intl.NumberFormat('en-US', {
        style: 'currency',
        currency: 'USD',
        minimumFractionDigits: 0,
        maximumFractionDigits: 0
      }).format(property.price),
      property
    }
  })
})

const center = computed(() => {
  if (markers.value.length === 0) {
    return [39.8283, -98.5795]
  }
  
  if (markers.value.length === 1) {
    return markers.value[0].position
  }
  
  const avgLat = markers.value.reduce((sum, m) => sum + m.position[0], 0) / markers.value.length
  const avgLng = markers.value.reduce((sum, m) => sum + m.position[1], 0) / markers.value.length
  
  return [avgLat, avgLng]
})

const zoom = ref(4)
const map = ref(null)

watch(() => [props.properties.length, markers.value], ([newLength, newMarkers]) => {
  if (newLength === 0) {
    zoom.value = 4
    return
  }
  
  if (newLength === 1) {
    zoom.value = 13
    return
  }
  
  if (newMarkers && newMarkers.length > 0) {
    const lats = newMarkers.map(m => m.position[0])
    const lngs = newMarkers.map(m => m.position[1])
    
    const minLat = Math.min(...lats)
    const maxLat = Math.max(...lats)
    const minLng = Math.min(...lngs)
    const maxLng = Math.max(...lngs)
    
    const latDiff = maxLat - minLat
    const lngDiff = maxLng - minLng
    const maxDiff = Math.max(latDiff, lngDiff)
    
    if (maxDiff > 15) {
      zoom.value = 4
    } else if (maxDiff > 10) {
      zoom.value = 5
    } else if (maxDiff > 5) {
      zoom.value = 6
    } else if (maxDiff > 2) {
      zoom.value = 7
    } else if (maxDiff > 1) {
      zoom.value = 8
    } else if (maxDiff > 0.5) {
      zoom.value = 9
    } else if (maxDiff > 0.2) {
      zoom.value = 10
    } else {
      zoom.value = 11
    }
  } else {
    zoom.value = 4
  }
}, { immediate: true })
</script>

<style>
.leaflet-container {
  height: 100%;
  width: 100%;
  z-index: 1;
}

.leaflet-control {
  z-index: 1000;
}
</style>

