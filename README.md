# GoSafe Frontend

GoSafe Frontend is a React + Vite application for safer trip planning and live journey sharing. It helps a traveler discover route options, compare them by safety signals, start a tracked journey, trigger SOS alerts, and share a live tracking link with trusted contacts.

The UI is built for a mobile-first travel flow and integrates with a backend running at `http://localhost:8080`.

## What the App Does

- Authenticated and guest access modes
- Route discovery with map-based visualization
- Safety-ranked route options with route metrics
- Live journey tracking and shareable tracking links
- SOS workflow with countdown, siren, and emergency contact support
- Journey history for saved trips
- Profile management and emergency contact updates
- Password recovery using a security question flow

## Tech Stack

- React 18
- Vite 5
- React Router DOM
- Tailwind CSS
- React Leaflet + Leaflet
- Lucide React icons

## Main User Flows

### 1. Authentication

Users can:

- Register with name, email, password, emergency contact, and a recovery question
- Sign in with an existing account
- Reset a password using the saved security question
- Continue in guest mode with emergency contact details

### 2. Dashboard

The dashboard is the core of the app. From here a traveler can:

- Use current location as the route origin
- Search origin and destination addresses
- Request route alternatives
- Compare route options by:
  - distance
  - duration
  - safety rating
  - lighting level
  - nearby shops
  - crowd level
  - traffic level
- Start a journey and enable live tracking
- Share a live tracking link
- Open the SOS modal if help is needed

### 3. Live Tracking

GoSafe supports two tracking modes:

- Database-backed share links for authenticated users: `/share/:journeyId`
- Query-string based share links for guest users: `/share-tracking?...`

The shared page renders the route and a moving traveler marker on the map.

### 4. SOS

The SOS flow includes:

- 3-second countdown before dispatch
- Optional siren sound using the Web Audio API
- GPS capture from the current map position or browser geolocation
- Server-side SOS trigger request
- WhatsApp deep link to notify the emergency contact

### 5. Journey History

- Logged-in users load history from the backend
- Guest users load history from `localStorage`
- Journey entries can include:
  - route screenshot
  - attached image
  - origin and destination
  - date/time
  - distance
  - duration
  - safety rating

## Project Structure

```text
src/
  components/
    AuthBackground.jsx
    MapView.jsx
    RouteCard.jsx
    SOSButton.jsx
  context/
    AuthContext.jsx
  pages/
    Dashboard.jsx
    ForgotPassword.jsx
    History.jsx
    Login.jsx
    Register.jsx
    ShareTracking.jsx
  App.jsx
  main.jsx
  index.css
```

## Routes

- `/login` - sign in
- `/register` - create account
- `/forgot-password` - password reset flow
- `/dashboard` - protected trip planning and tracking dashboard
- `/history` - protected journey history
- `/share/:journeyId` - share page for saved/authenticated journeys
- `/share-tracking` - share page for guest tracking links

## Backend Expectations

This frontend currently uses a hardcoded API base URL in [src/context/AuthContext.jsx](/E:/GO SAFE V2 DEPLOYMENT/V2-Frontend/src/context/AuthContext.jsx:5):

```js
export const API_BASE_URL = 'http://localhost:8080';
```

The backend is expected to provide endpoints similar to:

- `POST /api/auth/register`
- `POST /api/auth/login`
- `POST /api/auth/forgot-password`
- `POST /api/auth/reset-password`
- `PUT /api/auth/update-profile`
- `GET /api/location/search`
- `GET /api/location/reverse`
- `GET /api/location/shops`
- `GET /api/journeys`
- `POST /api/journeys`
- `DELETE /api/journeys`
- `DELETE /api/journeys/:id`
- `GET /api/journeys/share/:journeyId`
- `POST /api/sos/trigger`

The dashboard also relies on route/geocoding services exposed by the backend, which appears to proxy map/search requests to avoid browser CORS and rate-limit issues.

## Getting Started

### Prerequisites

- Node.js 18+ recommended
- npm
- GoSafe backend running on `http://localhost:8080`

### Install

```bash
npm install
```

### Run the Development Server

```bash
npm run dev
```

The Vite dev server runs on:

```text
http://localhost:5173
```

### Build for Production

```bash
npm run build
```

### Preview the Production Build

```bash
npm run preview
```

## Available Scripts

- `npm run dev` - start the local Vite dev server
- `npm run build` - create a production build
- `npm run preview` - preview the production build locally
- `npm run lint` - run ESLint

## State and Storage

The app stores some session and guest-mode data in `localStorage`, including:

- `gosafe_token`
- `gosafe_user`
- `gosafe_isguest`
- `gosafe_guest_emergency_name`
- `gosafe_guest_emergency_phone`
- `gosafe_guest_history`

## UI Notes

- The app uses a monochrome, map-centric design language with Tailwind utilities and custom CSS animations.
- Leaflet is used for route display, traveler markers, SOS markers, and nearby shop markers.
- Several loading and processing states are encoded into the URL as a `token` query parameter for shareable state awareness.

## Current Limitations

- `API_BASE_URL` is hardcoded instead of being environment-driven
- There is no test setup in this frontend yet
- Some flows depend on browser permissions such as geolocation, audio, and popup handling
- Map and live-share quality depends on backend route and geocoding responses

## Suggested Next Improvements

- Move API configuration to Vite environment variables
- Add component and integration tests
- Add error boundaries and retry UI for network-heavy flows
- Add loading skeletons and empty states for more screens
- Add deployment instructions for staging/production environments

## License

No license file is currently included in this repository.

