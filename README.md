# Global Insights Dashboard

A mini‑project for the **Service‑Oriented Computing** module.

The Global Insights Dashboard is a web application that lets a user select a country and immediately view consolidated information from multiple public APIs:

- Country details (population, region, capital, flag)
- Current weather
- Latest COVID‑19 statistics
- Currency exchange rates

The combined snapshot can be saved to a MongoDB database for later viewing.

---

## Table of Contents

- [Features](#features)
- [System Architecture](#system-architecture)
- [Technology Stack](#technology-stack)
- [Public APIs Used](#public-apis-used)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
  - [Backend Setup](#backend-setup)
  - [Frontend Setup](#frontend-setup)
- [How to Use](#how-to-use)
- [Backend API Endpoints](#backend-api-endpoints)
- [Data Model](#data-model)
- [Security Features](#security-features)
- [Possible Improvements](#possible-improvements)

---

## Features

**Client (Frontend)**

- Country dropdown populated from REST Countries API.
- Fetch and display for the selected country:
  - Country info and flag
  - Current weather
  - COVID‑19 statistics
  - Exchange rates (using the country’s main currency as base)
- Merge all fetched data into a single JSON snapshot.
- Save snapshot to backend with:
  - Bearer token (JWT) for user authentication.
  - Application‑level API key for extra protection.
- View a list of previously saved snapshots.

**Server (Backend)**

- User registration and login with JWT tokens.
- Secure endpoints to create and fetch country data snapshots.
- MongoDB storage for users and snapshots, with indexes.
- Basic production‑style middleware: CORS, Helmet, logging, rate limiting.

---

## System Architecture

The application follows a simple client‑server architecture:

- **Frontend (Static Web App)**
  - HTML + CSS + Vanilla JavaScript.
  - Calls external public APIs directly from the browser.
  - After merging results, posts the combined JSON to the backend.

- **Backend (Node.js + Express)**
  - Provides REST endpoints under `/api/*`.
  - Validates the incoming snapshot and stores it in MongoDB with a reference to the authenticated user.
  - Enforces JWT authentication and API‑key based authorization.

- **Database (MongoDB / MongoDB Atlas)**
  - Stores user accounts.
  - Stores snapshot records with country, weather, COVID, and exchange‑rate data.

---

## Technology Stack

**Frontend**

- HTML5, CSS3
- Vanilla JavaScript
  - `apiClient.js` – calls backend + public APIs
  - `main.js` – app logic and event handling
  - `ui.js` – DOM updates and rendering

**Backend**

- Node.js
- Express.js
- MongoDB with Mongoose
- JSON Web Tokens (`jsonwebtoken`)
- Password hashing with `bcryptjs`
- Middleware:
  - `cors`, `helmet`, `morgan`, `express-rate-limit`

---

## Public APIs Used

3. How to explain this in your report
You can write something like:

The project integrates four public web APIs: RESTCountries, OpenWeatherMap, disease.sh COVID‑19 API, and open.er-api.com for exchange rates.
Among these, only OpenWeatherMap requires an API key, which is configured in the frontend as OPENWEATHERMAP_API_KEY.
In addition, the backend defines its own application-level API key (API_KEY in .env), which is used to protect the /records endpoints but is not related to any external service.

So yes:

4 public APIs are used ✅
1 external key (OpenWeatherMap) ✅
1 internal app key for your backend ✅
You don’t need 4 different external keys.


1. **REST Countries API**  
   - URL: `https://restcountries.com/v3.1/all`  
   - Purpose: get country name, ISO code, capital, region, population, flag URLs, and coordinates.

2. **OpenWeatherMap API**  
   - URL: `https://api.openweathermap.org/data/2.5/weather`  
   - Purpose: get current weather (temperature, feels like, humidity, conditions) using latitude and longitude.

3. **COVID‑19 Data API (disease.sh)**  
   - URL: `https://disease.sh/v3/covid-19/countries/{country}`  
   - Purpose: get latest COVID‑19 stats per country (total cases, today’s cases, deaths, recovered).

4. **Exchange Rate API (open.er-api.com)**  
   - URL: `https://open.er-api.com/v6/latest/{baseCurrency}`  
   - Purpose: get real‑time currency exchange rates using the country’s main currency as the base.

---

## Project Structure

```text
global-insights-dashboard/
  backend/
    server.js
    .env
    package.json
    src/
      config/
        db.js
      controllers/
        authController.js
        recordController.js
      middlewares/
        authMiddleware.js
        apiKeyMiddleware.js
        errorHandler.js
      models/
        User.js
        Record.js
      routes/
        authRoutes.js
        recordRoutes.js
      utils/
        logger.js  (optional helper)
  frontend/
    index.html
    css/
      styles.css
    js/
      main.js
      apiClient.js
      ui.js
  README.md