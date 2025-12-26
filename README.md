# Houzeo Real Estate Property Listing Application

A modern, responsive real estate property listing application built with Vue 3, Vite, Tailwind CSS, and Leaflet maps. This application allows users to search, filter, and view property listings with an interactive map interface.

## Features

- **Property Listings**: Browse properties with detailed information including price, beds, baths, square footage, and address
-**Interactive Map**: View property locations on an interactive Leaflet map
- **Search & Filter**: Search by location, filter by price range, property type, beds/baths, and status
-**Responsive Design**: Optimized for desktop and mobile devices (including iPhone 14 Pro Max)
-**Image Slider**: Property image carousel with navigation arrows and indicators
-**Favorites**: Save favorite properties with heart icon animation
-**Modern UI**: Clean, professional design with smooth animations and transitions
-**Performance**: Lazy loading for images and optimized rendering

## Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js** (version 16.x or higher)
- **npm** (version 7.x or higher) or **yarn**

You can check your versions by running:
```bash
node --version
npm --version
```

## Installation

1. **Clone the repository** (if applicable) or navigate to the project directory:
   ```bash
   cd "vue js"
   ```

2. **Install dependencies**:
   ```bash
   npm install
   ```

   This will install all required packages including:
   - Vue 3
   - Vite
   - Tailwind CSS
   - Leaflet & Vue-Leaflet
   - PostCSS & Autoprefixer

## Running the Development Server

To start the development server:

```bash
npm run dev
```

The application will be available at `http://localhost:5173` (or the port shown in your terminal).

The dev server includes:
- Hot Module Replacement (HMR) for instant updates
- Fast refresh for Vue components
- Source maps for debugging

## Building for Production

To create a production build:

```bash
npm run build
```

The optimized production files will be generated in the `dist` directory.

To preview the production build locally:

```bash
npm run preview
```

## Project Structure

```
vue js/
├── src/
│   ├── components/
│   │   ├── header/
│   │   │   └── Header.vue          # Main navigation header
│   │   ├── MapView.vue             # Interactive Leaflet map component
│   │   ├── PropertyListing.vue     # Property card component with image slider
│   │   └── SearchBar.vue           # Search and filter bar component
│   ├── assets/
│   │   └── images/                  # Image assets (property images, icons, logos)
│   ├── App.vue                     # Main application component
│   ├── main.js                     # Application entry point
│   └── style.css                   # Global styles and Tailwind imports
├── public/                         # Static public assets
├── index.html                      # HTML template
├── package.json                    # Project dependencies and scripts
├── vite.config.js                  # Vite configuration
├── tailwind.config.js              # Tailwind CSS configuration
└── postcss.config.js               # PostCSS configuration
```

## Available Scripts

- `npm run dev` - Start development server
- `npm run build` - Build for production
- `npm run preview` - Preview production build locally

## Technologies Used

- **Vue 3** - Progressive JavaScript framework
- **Vite** - Next-generation frontend build tool
- **Tailwind CSS** - Utility-first CSS framework
- **Leaflet** - Open-source JavaScript library for mobile-friendly interactive maps
- **Vue-Leaflet** - Vue 3 components for Leaflet maps

## Key Features Implementation

### Search Functionality
- Search by city, state, or address
- Real-time filtering as you type
- Empty search shows all properties

### Filter Options
- **Status**: For Sale / Sold
- **Price Range**: Under $200k, $200k-$500k, $500k-$1M, Over $1M
- **Beds & Baths**: Filter by minimum number of bedrooms
- **Property Type**: House, Condo, Townhouse, Multi-family, Land

### Sorting Options
- New Listing (default)
- Price: Low to High
- Price: High to Low
- Square Feet
- Days on Market

### Map Features
- Interactive Leaflet map with OpenStreetMap tiles
- Property markers with popups showing details
- Automatic zoom adjustment based on property count
- Map centers on filtered properties

## Browser Support

- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)
- Mobile browsers (iOS Safari, Chrome Mobile)

## Troubleshooting

### Port Already in Use
If port 5173 is already in use, Vite will automatically try the next available port. Check your terminal for the actual port number.

### Dependencies Installation Issues
If you encounter issues installing dependencies:
```bash
rm -rf node_modules package-lock.json
npm install
```

### Map Not Displaying
Ensure that Leaflet CSS is properly imported. Check `src/main.js` for the Leaflet CSS import.

## Development Notes

- The application uses Vue 3 Composition API with `<script setup>` syntax
- All components are located in the `src/components` directory
- Property data is stored in `App.vue` as a reactive ref
- Filters use Vue's `provide/inject` pattern for state management
- Map markers are dynamically generated based on filtered properties

## License

This project is for demonstration purposes.

## Contact

For questions or issues, please refer to the project documentation or contact the development team.
