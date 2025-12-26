<script setup>
import Header from './components/header/Header.vue'
import SearchBar from './components/SearchBar.vue'
import MapView from './components/MapView.vue'
import PropertyListing from './components/PropertyListing.vue'
import { ref, computed, provide } from 'vue'

const originalProperties = ref([
  {
    id: 1,
    daysOnHouzeo: 6,
    propertyType: 'House For Sale',
    price: 3349000,
    beds: 4,
    baths: 3,
    sqft: 998,
    address: '2856 Meadow Park Ave, Henderson, NV 89052',
    city: 'Henderson',
    state: 'NV',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 2300,
    status: 'for-sale'
  },
  {
    id: 2,
    daysOnHouzeo: 12,
    propertyType: 'Condo For Sale',
    price: 1250000,
    beds: 2,
    baths: 2,
    sqft: 1450,
    address: '4521 Riverside Drive, Austin, TX 78704',
    city: 'Austin',
    state: 'TX',
    brokerage: 'Sotheby\'s International Realty',
    views: 1800,
    status: 'for-sale'
  },
  {
    id: 3,
    daysOnHouzeo: 10,
    propertyType: 'Multi-family home For Sale',
    price: 2890000,
    beds: 5,
    baths: 4,
    sqft: 3200,
    address: '7892 Hill Country Blvd, Austin, TX 78731',
    city: 'Austin',
    state: 'TX',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 3200,
    status: 'for-sale'
  },
  {
    id: 4,
    daysOnHouzeo: 8,
    propertyType: 'House For Sale',
    price: 1895000,
    beds: 3,
    baths: 2.5,
    sqft: 2100,
    address: '1234 South Lamar Blvd, Austin, TX 78704',
    city: 'Austin',
    state: 'TX',
    brokerage: 'Sotheby\'s International Realty',
    views: 2100,
    status: 'for-sale'
  },
  {
    id: 5,
    daysOnHouzeo: 15,
    propertyType: 'House For Sale',
    price: 4250000,
    beds: 6,
    baths: 5,
    sqft: 4800,
    address: '5678 Westlake Hills Drive, Austin, TX 78746',
    city: 'Austin',
    state: 'TX',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 4500,
    status: 'for-sale'
  },
  {
    id: 6,
    daysOnHouzeo: 5,
    propertyType: 'Condo For Sale',
    price: 875000,
    beds: 1,
    baths: 1,
    sqft: 850,
    address: '9012 Downtown Square, Austin, TX 78701',
    city: 'Austin',
    state: 'TX',
    brokerage: 'Sotheby\'s International Realty',
    views: 1500,
    status: 'for-sale'
  },
  {
    id: 7,
    daysOnHouzeo: 3,
    propertyType: 'House For Sale',
    price: 2200000,
    beds: 4,
    baths: 3.5,
    sqft: 2800,
    address: '3456 River Oaks Blvd, Nashville, TN 37215',
    city: 'Nashville',
    state: 'TN',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 1900,
    status: 'for-sale'
  },
  {
    id: 8,
    daysOnHouzeo: 20,
    propertyType: 'Townhouse For Sale',
    price: 650000,
    beds: 2,
    baths: 2,
    sqft: 1200,
    address: '7890 Music City Way, Nashville, TN 37203',
    city: 'Nashville',
    state: 'TN',
    brokerage: 'Sotheby\'s International Realty',
    views: 1200,
    status: 'for-sale'
  },
  {
    id: 9,
    daysOnHouzeo: 7,
    propertyType: 'House For Sale',
    price: 1500000,
    beds: 3,
    baths: 2,
    sqft: 1800,
    address: '2345 Lake Travis Dr, Austin, TX 78734',
    city: 'Austin',
    state: 'TX',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 2100,
    status: 'for-sale'
  },
  {
    id: 10,
    daysOnHouzeo: 14,
    propertyType: 'Condo For Sale',
    price: 450000,
    beds: 1,
    baths: 1,
    sqft: 750,
    address: '5678 Downtown Ave, Henderson, NV 89014',
    city: 'Henderson',
    state: 'NV',
    brokerage: 'Sotheby\'s International Realty',
    views: 980,
    status: 'for-sale'
  },
  {
    id: 11,
    daysOnHouzeo: 9,
    propertyType: 'House For Sale',
    price: 3200000,
    beds: 5,
    baths: 4,
    sqft: 3500,
    address: '1234 Barton Springs Rd, Austin, TX 78704',
    city: 'Austin',
    state: 'TX',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 2800,
    status: 'for-sale'
  },
  {
    id: 12,
    daysOnHouzeo: 4,
    propertyType: 'Multi-family home For Sale',
    price: 1800000,
    beds: 4,
    baths: 3,
    sqft: 2400,
    address: '8901 Broadway St, Nashville, TN 37203',
    city: 'Nashville',
    state: 'TN',
    brokerage: 'Sotheby\'s International Realty',
    views: 1650,
    status: 'for-sale'
  },
  {
    id: 13,
    daysOnHouzeo: 2,
    propertyType: 'House For Sale',
    price: 2750000,
    beds: 5,
    baths: 4.5,
    sqft: 4200,
    address: '4567 Sunset Blvd, Los Angeles, CA 90028',
    city: 'Los Angeles',
    state: 'CA',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 3200,
    status: 'for-sale'
  },
  {
    id: 14,
    daysOnHouzeo: 25,
    propertyType: 'House For Sale',
    price: 1950000,
    beds: 4,
    baths: 3,
    sqft: 3100,
    address: '7890 Ocean Drive, Miami, FL 33139',
    city: 'Miami',
    state: 'FL',
    brokerage: 'Sotheby\'s International Realty',
    views: 2800,
    status: 'for-sale'
  },
  {
    id: 15,
    daysOnHouzeo: 45,
    propertyType: 'House For Sale',
    price: 1650000,
    beds: 3,
    baths: 2.5,
    sqft: 2200,
    address: '123 Main Street, Seattle, WA 98101',
    city: 'Seattle',
    state: 'WA',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 1950,
    status: 'for-sale'
  },
  {
    id: 16,
    daysOnHouzeo: 60,
    propertyType: 'House For Sale',
    price: 850000,
    beds: 3,
    baths: 2,
    sqft: 1800,
    address: '2345 Elm Avenue, Denver, CO 80202',
    city: 'Denver',
    state: 'CO',
    brokerage: 'Sotheby\'s International Realty',
    views: 1450,
    status: 'for-sale'
  },
  {
    id: 17,
    daysOnHouzeo: 90,
    propertyType: 'House For Sale',
    price: 2100000,
    beds: 4,
    baths: 3,
    sqft: 2800,
    address: '5678 Park Avenue, New York, NY 10021',
    city: 'New York',
    state: 'NY',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 4200,
    status: 'sold'
  },
  {
    id: 18,
    daysOnHouzeo: 75,
    propertyType: 'House For Sale',
    price: 1450000,
    beds: 3,
    baths: 2,
    sqft: 2000,
    address: '3456 Maple Street, Chicago, IL 60601',
    city: 'Chicago',
    state: 'IL',
    brokerage: 'Sotheby\'s International Realty',
    views: 2100,
    status: 'sold'
  },
  {
    id: 19,
    daysOnHouzeo: 55,
    propertyType: 'House For Sale',
    price: 3200000,
    beds: 6,
    baths: 5,
    sqft: 5200,
    address: '8901 Beverly Hills Dr, Beverly Hills, CA 90210',
    city: 'Beverly Hills',
    state: 'CA',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 5800,
    status: 'sold'
  },
  {
    id: 20,
    daysOnHouzeo: 1,
    propertyType: 'Condo For Sale',
    price: 1250000,
    beds: 2,
    baths: 2,
    sqft: 1350,
    address: '100 Ocean View Blvd, San Francisco, CA 94102',
    city: 'San Francisco',
    state: 'CA',
    brokerage: 'Sotheby\'s International Realty',
    views: 2400,
    status: 'for-sale'
  },
  {
    id: 21,
    daysOnHouzeo: 18,
    propertyType: 'Condo For Sale',
    price: 680000,
    beds: 1,
    baths: 1,
    sqft: 900,
    address: '567 Market Street, Boston, MA 02108',
    city: 'Boston',
    state: 'MA',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 1650,
    status: 'for-sale'
  },
  {
    id: 22,
    daysOnHouzeo: 30,
    propertyType: 'Condo For Sale',
    price: 950000,
    beds: 2,
    baths: 2.5,
    sqft: 1500,
    address: '789 Downtown Plaza, Portland, OR 97201',
    city: 'Portland',
    state: 'OR',
    brokerage: 'Sotheby\'s International Realty',
    views: 1800,
    status: 'for-sale'
  },
  {
    id: 23,
    daysOnHouzeo: 85,
    propertyType: 'Condo For Sale',
    price: 1850000,
    beds: 3,
    baths: 2,
    sqft: 1800,
    address: '200 Central Park South, New York, NY 10019',
    city: 'New York',
    state: 'NY',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 3500,
    status: 'sold'
  },
  {
    id: 24,
    daysOnHouzeo: 70,
    propertyType: 'Condo For Sale',
    price: 750000,
    beds: 1,
    baths: 1,
    sqft: 850,
    address: '456 Harbor View, San Diego, CA 92101',
    city: 'San Diego',
    state: 'CA',
    brokerage: 'Sotheby\'s International Realty',
    views: 1900,
    status: 'sold'
  },
  {
    id: 25,
    daysOnHouzeo: 50,
    propertyType: 'Condo For Sale',
    price: 1100000,
    beds: 2,
    baths: 2,
    sqft: 1400,
    address: '321 Waterfront Ave, Seattle, WA 98121',
    city: 'Seattle',
    state: 'WA',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 2200,
    status: 'sold'
  },
  {
    id: 26,
    daysOnHouzeo: 11,
    propertyType: 'Townhouse For Sale',
    price: 720000,
    beds: 3,
    baths: 2.5,
    sqft: 1650,
    address: '1234 Historic District, Charleston, SC 29401',
    city: 'Charleston',
    state: 'SC',
    brokerage: 'Sotheby\'s International Realty',
    views: 1750,
    status: 'for-sale'
  },
  {
    id: 27,
    daysOnHouzeo: 22,
    propertyType: 'Townhouse For Sale',
    price: 580000,
    beds: 2,
    baths: 2,
    sqft: 1300,
    address: '5678 Garden Street, Savannah, GA 31401',
    city: 'Savannah',
    state: 'GA',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 1400,
    status: 'for-sale'
  },
  {
    id: 28,
    daysOnHouzeo: 35,
    propertyType: 'Townhouse For Sale',
    price: 890000,
    beds: 3,
    baths: 3,
    sqft: 1900,
    address: '9012 Urban Loft Way, Philadelphia, PA 19102',
    city: 'Philadelphia',
    state: 'PA',
    brokerage: 'Sotheby\'s International Realty',
    views: 2100,
    status: 'for-sale'
  },
  {
    id: 29,
    daysOnHouzeo: 80,
    propertyType: 'Townhouse For Sale',
    price: 1250000,
    beds: 4,
    baths: 3.5,
    sqft: 2400,
    address: '3456 Georgetown Row, Washington, DC 20007',
    city: 'Washington',
    state: 'DC',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 2900,
    status: 'sold'
  },
  {
    id: 30,
    daysOnHouzeo: 65,
    propertyType: 'Townhouse For Sale',
    price: 680000,
    beds: 2,
    baths: 2,
    sqft: 1450,
    address: '7890 Brick Lane, Baltimore, MD 21202',
    city: 'Baltimore',
    state: 'MD',
    brokerage: 'Sotheby\'s International Realty',
    views: 1600,
    status: 'sold'
  },
  {
    id: 31,
    daysOnHouzeo: 48,
    propertyType: 'Townhouse For Sale',
    price: 950000,
    beds: 3,
    baths: 2.5,
    sqft: 1750,
    address: '2345 Victorian Court, San Francisco, CA 94117',
    city: 'San Francisco',
    state: 'CA',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 2300,
    status: 'sold'
  },
  {
    id: 32,
    daysOnHouzeo: 16,
    propertyType: 'Multi-family home For Sale',
    price: 2200000,
    beds: 6,
    baths: 5,
    sqft: 3800,
    address: '4567 Investment Property Blvd, Atlanta, GA 30309',
    city: 'Atlanta',
    state: 'GA',
    brokerage: 'Sotheby\'s International Realty',
    views: 2600,
    status: 'for-sale'
  },
  {
    id: 33,
    daysOnHouzeo: 28,
    propertyType: 'Multi-family home For Sale',
    price: 1950000,
    beds: 5,
    baths: 4,
    sqft: 3400,
    address: '7890 Duplex Drive, Dallas, TX 75201',
    city: 'Dallas',
    state: 'TX',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 2400,
    status: 'for-sale'
  },
  {
    id: 34,
    daysOnHouzeo: 40,
    propertyType: 'Multi-family home For Sale',
    price: 1650000,
    beds: 4,
    baths: 3,
    sqft: 2800,
    address: '1234 Rental Property Way, Phoenix, AZ 85001',
    city: 'Phoenix',
    state: 'AZ',
    brokerage: 'Sotheby\'s International Realty',
    views: 1950,
    status: 'for-sale'
  },
  {
    id: 35,
    daysOnHouzeo: 95,
    propertyType: 'Multi-family home For Sale',
    price: 2800000,
    beds: 8,
    baths: 6,
    sqft: 4800,
    address: '5678 Apartment Complex Rd, Houston, TX 77002',
    city: 'Houston',
    state: 'TX',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 3800,
    status: 'sold'
  },
  {
    id: 36,
    daysOnHouzeo: 72,
    propertyType: 'Multi-family home For Sale',
    price: 2100000,
    beds: 6,
    baths: 5,
    sqft: 3600,
    address: '9012 Fourplex Street, Las Vegas, NV 89101',
    city: 'Las Vegas',
    state: 'NV',
    brokerage: 'Sotheby\'s International Realty',
    views: 2700,
    status: 'sold'
  },
  {
    id: 37,
    daysOnHouzeo: 58,
    propertyType: 'Multi-family home For Sale',
    price: 1750000,
    beds: 5,
    baths: 4,
    sqft: 3000,
    address: '3456 Triplex Avenue, Orlando, FL 32801',
    city: 'Orlando',
    state: 'FL',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 2200,
    status: 'sold'
  },
  {
    id: 38,
    daysOnHouzeo: 6,
    propertyType: 'Land For Sale',
    price: 450000,
    beds: 0,
    baths: 0,
    sqft: 0,
    address: '100 Acres Mountain View, Aspen, CO 81611',
    city: 'Aspen',
    state: 'CO',
    brokerage: 'Sotheby\'s International Realty',
    views: 1200,
    status: 'for-sale'
  },
  {
    id: 39,
    daysOnHouzeo: 19,
    propertyType: 'Land For Sale',
    price: 320000,
    beds: 0,
    baths: 0,
    sqft: 0,
    address: '50 Acres Lakefront Property, Lake Tahoe, CA 96150',
    city: 'Lake Tahoe',
    state: 'CA',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 980,
    status: 'for-sale'
  },
  {
    id: 40,
    daysOnHouzeo: 32,
    propertyType: 'Land For Sale',
    price: 280000,
    beds: 0,
    baths: 0,
    sqft: 0,
    address: '25 Acres Countryside Estate, Napa Valley, CA 94558',
    city: 'Napa Valley',
    state: 'CA',
    brokerage: 'Sotheby\'s International Realty',
    views: 850,
    status: 'for-sale'
  },
  {
    id: 41,
    daysOnHouzeo: 88,
    propertyType: 'Land For Sale',
    price: 650000,
    beds: 0,
    baths: 0,
    sqft: 0,
    address: '200 Acres Development Land, Scottsdale, AZ 85251',
    city: 'Scottsdale',
    state: 'AZ',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 2100,
    status: 'sold'
  },
  {
    id: 42,
    daysOnHouzeo: 68,
    propertyType: 'Land For Sale',
    price: 380000,
    beds: 0,
    baths: 0,
    sqft: 0,
    address: '75 Acres Ocean View Lot, Malibu, CA 90265',
    city: 'Malibu',
    state: 'CA',
    brokerage: 'Sotheby\'s International Realty',
    views: 1650,
    status: 'sold'
  },
  {
    id: 43,
    daysOnHouzeo: 52,
    propertyType: 'Land For Sale',
    price: 520000,
    beds: 0,
    baths: 0,
    sqft: 0,
    address: '150 Acres Ranch Property, Jackson Hole, WY 83001',
    city: 'Jackson Hole',
    state: 'WY',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 1900,
    status: 'sold'
  },
  {
    id: 44,
    daysOnHouzeo: 8,
    propertyType: 'Condo For Sale',
    price: 185000,
    beds: 1,
    baths: 1,
    sqft: 650,
    address: '1234 Affordable Living St, Austin, TX 78702',
    city: 'Austin',
    state: 'TX',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 1200,
    status: 'for-sale'
  },
  {
    id: 45,
    daysOnHouzeo: 15,
    propertyType: 'Condo For Sale',
    price: 165000,
    beds: 1,
    baths: 1,
    sqft: 580,
    address: '5678 Budget Friendly Ave, Austin, TX 78721',
    city: 'Austin',
    state: 'TX',
    brokerage: 'Sotheby\'s International Realty',
    views: 980,
    status: 'for-sale'
  },
  {
    id: 46,
    daysOnHouzeo: 22,
    propertyType: 'House For Sale',
    price: 195000,
    beds: 2,
    baths: 1,
    sqft: 1100,
    address: '9012 Starter Home Blvd, Austin, TX 78723',
    city: 'Austin',
    state: 'TX',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 1450,
    status: 'for-sale'
  },
  {
    id: 47,
    daysOnHouzeo: 5,
    propertyType: 'Townhouse For Sale',
    price: 175000,
    beds: 2,
    baths: 1.5,
    sqft: 950,
    address: '3456 Entry Level Way, Austin, TX 78741',
    city: 'Austin',
    state: 'TX',
    brokerage: 'Sotheby\'s International Realty',
    views: 1100,
    status: 'for-sale'
  },
  {
    id: 48,
    daysOnHouzeo: 12,
    propertyType: 'Condo For Sale',
    price: 145000,
    beds: 1,
    baths: 1,
    sqft: 520,
    address: '7890 Compact Living Dr, Austin, TX 78744',
    city: 'Austin',
    state: 'TX',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 850,
    status: 'for-sale'
  },
  {
    id: 49,
    daysOnHouzeo: 18,
    propertyType: 'House For Sale',
    price: 189000,
    beds: 2,
    baths: 2,
    sqft: 1200,
    address: '2345 Value Home St, Austin, TX 78745',
    city: 'Austin',
    state: 'TX',
    brokerage: 'Sotheby\'s International Realty',
    views: 1300,
    status: 'for-sale'
  },
  {
    id: 50,
    daysOnHouzeo: 30,
    propertyType: 'Condo For Sale',
    price: 125000,
    beds: 1,
    baths: 1,
    sqft: 480,
    address: '4567 Economy Condo Ave, Austin, TX 78746',
    city: 'Austin',
    state: 'TX',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 750,
    status: 'for-sale'
  },
  {
    id: 51,
    daysOnHouzeo: 7,
    propertyType: 'House For Sale',
    price: 165000,
    beds: 2,
    baths: 1,
    sqft: 1050,
    address: '8901 Affordable Housing Rd, Austin, TX 78747',
    city: 'Austin',
    state: 'TX',
    brokerage: 'Sotheby\'s International Realty',
    views: 1150,
    status: 'for-sale'
  },
  {
    id: 52,
    daysOnHouzeo: 25,
    propertyType: 'Townhouse For Sale',
    price: 155000,
    beds: 2,
    baths: 1.5,
    sqft: 880,
    address: '1234 Budget Townhome Ln, Austin, TX 78748',
    city: 'Austin',
    state: 'TX',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 950,
    status: 'for-sale'
  },
  {
    id: 53,
    daysOnHouzeo: 40,
    propertyType: 'Condo For Sale',
    price: 135000,
    beds: 1,
    baths: 1,
    sqft: 550,
    address: '5678 Low Cost Living St, Austin, TX 78749',
    city: 'Austin',
    state: 'TX',
    brokerage: 'Sotheby\'s International Realty',
    views: 680,
    status: 'for-sale'
  },
  {
    id: 54,
    daysOnHouzeo: 10,
    propertyType: 'House For Sale',
    price: 179000,
    beds: 3,
    baths: 2,
    sqft: 1350,
    address: '9012 First Time Buyer Blvd, Austin, TX 78750',
    city: 'Austin',
    state: 'TX',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 1600,
    status: 'for-sale'
  },
  {
    id: 55,
    daysOnHouzeo: 3,
    propertyType: 'Condo For Sale',
    price: 195000,
    beds: 2,
    baths: 1,
    sqft: 750,
    address: '3456 Modern Affordable Way, Austin, TX 78751',
    city: 'Austin',
    state: 'TX',
    brokerage: 'Sotheby\'s International Realty',
    views: 1400,
    status: 'for-sale'
  },
  {
    id: 56,
    daysOnHouzeo: 20,
    propertyType: 'House For Sale',
    price: 148000,
    beds: 2,
    baths: 1,
    sqft: 980,
    address: '7890 Starter Property Dr, Austin, TX 78752',
    city: 'Austin',
    state: 'TX',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 1100,
    status: 'for-sale'
  },
  {
    id: 57,
    daysOnHouzeo: 35,
    propertyType: 'Condo For Sale',
    price: 115000,
    beds: 1,
    baths: 1,
    sqft: 450,
    address: '2345 Minimalist Living Ave, Austin, TX 78753',
    city: 'Austin',
    state: 'TX',
    brokerage: 'Sotheby\'s International Realty',
    views: 620,
    status: 'for-sale'
  },
  {
    id: 58,
    daysOnHouzeo: 14,
    propertyType: 'Townhouse For Sale',
    price: 168000,
    beds: 2,
    baths: 2,
    sqft: 1020,
    address: '4567 Cozy Home St, Austin, TX 78754',
    city: 'Austin',
    state: 'TX',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 1050,
    status: 'for-sale'
  },
  {
    id: 59,
    daysOnHouzeo: 6,
    propertyType: 'House For Sale',
    price: 185000,
    beds: 2,
    baths: 1.5,
    sqft: 1150,
    address: '8901 Entry Level Home Rd, Austin, TX 78756',
    city: 'Austin',
    state: 'TX',
    brokerage: 'Sotheby\'s International Realty',
    views: 1250,
    status: 'for-sale'
  },
  {
    id: 60,
    daysOnHouzeo: 28,
    propertyType: 'Condo For Sale',
    price: 155000,
    beds: 1,
    baths: 1,
    sqft: 600,
    address: '1234 Urban Living Blvd, Austin, TX 78757',
    city: 'Austin',
    state: 'TX',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 890,
    status: 'for-sale'
  },
  {
    id: 61,
    daysOnHouzeo: 9,
    propertyType: 'House For Sale',
    price: 172000,
    beds: 2,
    baths: 2,
    sqft: 1080,
    address: '5678 Family Starter Home Way, Austin, TX 78758',
    city: 'Austin',
    state: 'TX',
    brokerage: 'Sotheby\'s International Realty',
    views: 1180,
    status: 'for-sale'
  },
  {
    id: 62,
    daysOnHouzeo: 16,
    propertyType: 'Condo For Sale',
    price: 142000,
    beds: 1,
    baths: 1,
    sqft: 510,
    address: '9012 Compact Condo Ln, Austin, TX 78759',
    city: 'Austin',
    state: 'TX',
    brokerage: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
    views: 720,
    status: 'for-sale'
  }
])

const searchQuery = ref('')
const statusFilter = ref('for-sale')
const priceFilter = ref('')
const bedsFilter = ref('')
const propertyTypeFilter = ref('')
const sortBy = ref('New Listing')

provide('filters', {
  searchQuery,
  statusFilter,
  priceFilter,
  bedsFilter,
  propertyTypeFilter
})

const properties = computed(() => {
  let filtered = [...originalProperties.value]
  
  if (statusFilter.value) {
    filtered = filtered.filter(p => p.status === statusFilter.value)
  }
  
  if (searchQuery.value && searchQuery.value.trim()) {
    const query = searchQuery.value.toLowerCase().trim()
    filtered = filtered.filter(p => {
      const address = p.address.toLowerCase()
      const city = p.city.toLowerCase()
      const state = p.state.toLowerCase()
      const cityState = `${city}, ${state}`.toLowerCase()
      const stateCity = `${state}, ${city}`.toLowerCase()
      
      return address.includes(query) || 
             city.includes(query) || 
             state.includes(query) ||
             cityState.includes(query) ||
             stateCity.includes(query) ||
             `${city} ${state}`.includes(query) ||
             `${state} ${city}`.includes(query)
    })
  }
  
  if (priceFilter.value) {
    filtered = filtered.filter(p => {
      switch (priceFilter.value) {
        case 'under-200k':
          return p.price < 200000
        case '200k-500k':
          return p.price >= 200000 && p.price <= 500000
        case '500k-1m':
          return p.price > 500000 && p.price <= 1000000
        case 'over-1m':
          return p.price > 1000000
        default:
          return true
      }
    })
  }
  
  if (bedsFilter.value) {
    const minBeds = parseInt(bedsFilter.value.replace(/\D/g, '')) || 0
    filtered = filtered.filter(p => p.beds >= minBeds)
  }
  
  if (propertyTypeFilter.value) {
    const typeMap = {
      'House': 'House For Sale',
      'Condo': 'Condo For Sale',
      'Townhouse': 'Townhouse For Sale',
      'Multi-family': 'Multi-family home For Sale',
      'Land': 'Land For Sale'
    }
    const typeToMatch = typeMap[propertyTypeFilter.value] || propertyTypeFilter.value
    filtered = filtered.filter(p => p.propertyType.toLowerCase().includes(typeToMatch.toLowerCase()))
  }
  
  switch (sortBy.value) {
    case 'Price: Low to High':
      return filtered.sort((a, b) => a.price - b.price)
    case 'Price: High to Low':
      return filtered.sort((a, b) => b.price - a.price)
    case 'Square Feet':
      return filtered.sort((a, b) => b.sqft - a.sqft)
    case 'Days on Market':
      return filtered.sort((a, b) => a.daysOnHouzeo - b.daysOnHouzeo)
    case 'New Listing':
    default:
      return filtered.sort((a, b) => a.daysOnHouzeo - b.daysOnHouzeo)
  }
})

const propertyCount = computed(() => properties.value.length)

const locationTitle = computed(() => {
  if (searchQuery.value && searchQuery.value.trim()) {
    return `${searchQuery.value} real estate & homes for sale`
  }
  return 'All Properties - Real Estate & Homes for Sale'
})
</script>

<template>
  <div class="min-h-screen bg-gray-50">
    
    <Header />

    
    <SearchBar />

    
    <div class="max-w-[1920px] mx-auto">
      
      <div class="flex flex-col lg:flex-row h-[calc(100vh-140px)] lg:h-[calc(100vh-180px)] min-h-[600px]">
        
        <div class="hidden lg:block w-1/2 h-full border-r border-gray-200 flex-shrink-0">
          <MapView :properties="properties" />
        </div>

        
        <div class="w-full lg:w-1/2 overflow-y-auto bg-white">
          <div class="p-3 sm:p-4 lg:p-6">
            
            <div class="flex flex-col sm:flex-row sm:items-center sm:justify-between mb-4 sm:mb-6 gap-3 sm:gap-4">
              <div>
                <h1 class="text-xl sm:text-2xl lg:text-3xl font-bold text-gray-900 mb-1">
                  {{ locationTitle }}
                </h1>
                <p class="text-gray-600 text-xs sm:text-sm lg:text-base">{{ propertyCount }} {{ propertyCount === 1 ? 'Home' : 'Homes' }}</p>
              </div>
              
              
              <div class="flex items-center gap-2 flex-shrink-0">
                <span class="text-xs sm:text-sm text-gray-600 whitespace-nowrap">Sort by:</span>
                <select 
                  v-model="sortBy"
                  class="px-3 sm:px-4 py-2 border border-gray-300 rounded-lg bg-white text-xs sm:text-sm text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-500 cursor-pointer w-full sm:w-auto"
                >
                  <option>New Listing</option>
                  <option>Price: Low to High</option>
                  <option>Price: High to Low</option>
                  <option>Square Feet</option>
                  <option>Days on Market</option>
                </select>
              </div>
            </div>

            
            <div class="grid grid-cols-1 lg:grid-cols-1 xl:grid-cols-2 gap-4 sm:gap-5 lg:gap-6">
              <PropertyListing
                v-for="(property, index) in properties"
                :key="`${property.address}-${index}`"
                v-bind="property"
                :startImageIndex="index % 4"
              />
            </div>

            
            <div class="mt-8 text-center">
              <button class="px-6 py-3 bg-blue-600 hover:bg-blue-700 text-white rounded-lg font-medium transition-colors">
                Load More Properties
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
