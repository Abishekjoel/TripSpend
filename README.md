# TripSpend

A frontend-only travel expense manager. Create trips, log expenses against them, attach receipt and travel photos, and see where your money went by category.

TripSpend has no backend. All data lives in the browser's `localStorage`.

## Features

- **Landing page** with hero, features, trip gallery, and how-it-works sections
- **Sign up / sign in** with a local account system (guest use is also supported)
- **Dashboard** with total trips, total expenses, total spent, and the active trip
- **My Trips** with search, sort (latest, oldest, highest or lowest spent), and create-trip form with optional cover photo
- **Trip Details** with a summary (total spent, expense count, average), a category breakdown chart, and a searchable, filterable, sortable expense list
- **Expenses** with amount, category, date, description, and up to 3 photos each. Photos are compressed in the browser before saving.
- **Edit and delete** for trips and expenses, with confirmation dialogs
- Responsive layout for desktop, tablet, and mobile

## Running locally

No build step or dependencies are required. Serve the folder with any static server:

```bash
# Python 3
python -m http.server 8000
```

Then open <http://localhost:8000/index.html>.

Opening `index.html` directly from disk also works for most features. Serving over HTTP is recommended.

## Project structure

```
index.html            Landing page
register.html         Sign up
login.html            Sign in
dashboard.html        Overview of all trips and spending
trips.html            My Trips list, search, sort, create trip
trip-details.html     Single trip: summary, categories, expenses
css/style.css         All styles
js/auth.js            Local account and session handling
js/store.js           Data layer: trips, expenses, mock seed data, formatters
js/ui.js              Toast and escaping helpers
js/nav.js             Auth-aware navbar
js/main.js            Landing page behavior (nav toggle, scroll shadow)
js/dashboard.js       Dashboard page logic
js/trips.js           My Trips page logic
js/trip-details.js    Trip Details page logic
assets/images/        Logo, hero image, gallery, category and trip photos
```

## Data storage

Data is stored in `localStorage` under these keys:

| Key | Contents |
| --- | --- |
| `tripspend_users` | Registered accounts (passwords are lightly hashed, not secure) |
| `tripspend_session` | Current logged-in user |
| `tripspend_trips` | Trips |
| `tripspend_expenses` | Expenses, each linked to a trip by `tripId` |

When a user is logged in, trip and expense keys are suffixed with their user ID. Guests share the unsuffixed keys.

On first visit, four sample trips and their expenses are seeded so the app is not empty. Seeding runs once, tracked by a `tripspend_seeded` flag. Deleting every trip does not bring the sample data back.

Deleting a trip also deletes its expenses.

## Limitations

- Data is per browser. Clearing site data removes everything.
- Photos count against the browser's localStorage quota (typically around 5 to 10 MB). A full-storage error is shown as a message rather than a crash.
- Account passwords are hashed client-side with a simple non-cryptographic function. This is a prototype, not production auth.
- Font Awesome icons load from a CDN, so icons need internet access on first load.

## Tech

HTML, CSS, and vanilla JavaScript. No frameworks or build tools.
