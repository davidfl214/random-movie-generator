# Random Movie Generator

## Description

The random movie generator is a web application developed using Vue.js that allows users to discover random films, based on some criteria like genre, minimum rating and relase date range. The application uses The Movie Database (TMDB) API for retrieving film information.

## Features

- Film genre selection, based on the ones that TMDB API provides
- Minimum rating filter (1-5 stars)
- Relase date range selection
- Random movie search based on that criteria
- Film details visualization like title, description, relase date, poster and rating

## Technology stack

- Vue.js 3 (Composition API)
- Vite (build tool)
- Tailwind CSS (styles)
- Lucide Vue Next (icons)
- TMDB API

## Requirements

- Node.js (version 14 or higher)
- NPM or Yarn
- TMDB API key

## Installation

1. Clone this repository:
   ```
   git clone https://github.com/davidfl214/random-movie-generator
   ```

2. Go to the repository folder:
   ```
   cd random-movie-generator
   ```

3. Install dependencies:
   ```
   npm install
   ```
   or if you use yarn:
   ```
   yarn install
   ```

4. Create an `.env` file in the root directory and add your TMDB API key:
   ```
   VITE_TMDB_API_KEY=your_key_here
   ```

## Usage

1. Start the development server:
   ```
   npm run dev
   ```
   or with Yarn:
   ```
   yarn dev
   ```

2. Open your browser and visit `http://localhost:5173` (or the port that Vite shows you).

3. Use the interface to select your search criteria and click "Search movie" to get a random recommendation.

## Production build

For build production, execute:

```
npm run build
```
or with Yarn: 
```
yarn build
```

This will generate an optimized version of the app in the `dist` directory

## Contributing

Contributions are appreciated. Please open an issue for discussing major changes before creating a pull request.

Project link: [https://github.com/davidfl214/random-movie-generator](https://github.com/davidfl214/random-movie-generator)