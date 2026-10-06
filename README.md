# Cinema template

A complete starting point for a cinema: listings, showtimes, seat booking, payments and tickets with QR codes, with a public website, a back office for staff and a customer app.

| Folder | What it is | Stack | Runs on |
| --- | --- | --- | --- |
| [`backend`](backend) | API shared by the website, admin and app | Django, Django REST Framework | `localhost:8000` |
| [`website`](website) | Public website: programme, film pages, seat selection, payment, tickets | Astro | `localhost:4321` |
| [`admin`](admin) | Back office: daily figures, programme, films, bookings, check-in | Angular | `localhost:4200` |
| [`customer-app`](customer-app) | Mobile app for customers | Flutter | iOS and Android |

Live demo of the website: [mattoznav.github.io/templates-cinema-website](https://mattoznav.github.io/templates-cinema-website/), a static showcase published from the website repository with GitHub Pages. It runs without the backend: the programme is captured at build time, and accounts and bookings stay in the visitor's browser.

Each folder is a Git submodule with its own repository and its own README with more detail. The demo cinema, "Northlight Cinema", is fictional. Film facts come from Wikidata and synopses from Wikipedia, credited on every film; posters are generated.

## Requirements

| Tool | Version | Needed for |
| --- | --- | --- |
| Git | any recent version | cloning the template with its submodules |
| Python | 3.12 or newer | `backend` |
| Node.js and npm | Node 22.22 or newer (or 24.15+) | `website` and `admin` |
| Flutter | 3.44 or newer | `customer-app`, plus Xcode (iOS) or Android Studio (Android) |

There is no database server to install and no payment account to create: the data are CSV files loaded into SQLite, and a built-in fake payment provider simulates the whole payment flow. Stripe test mode can be switched on later (see the backend README).

## Install and run

### 1. Clone

```bash
git clone --recurse-submodules https://github.com/mattoznav/templates-cinema.git
cd templates-cinema
```

If you cloned without `--recurse-submodules`, run `git submodule update --init --recursive`.

### 2. Backend (first terminal)

```bash
cd backend
python3 -m venv .venv
.venv/bin/pip install -r requirements.txt
cp .env.example .env
```

Open `.env` and set `DEMO_ADMIN_PASSWORD` to a password of your choice: it creates the staff account `admin@example.com`. Then build the database and start the API:

```bash
.venv/bin/python manage.py bootstrap
.venv/bin/python manage.py runserver
```

`bootstrap` builds the local database from the CSV files; run it again at any time to reset everything.

### 3. Website (second terminal)

```bash
cd website
npm install
cp .env.example .env
npm run dev
```

### 4. Admin (third terminal)

```bash
cd admin
npm install
npm start
```

### 5. Customer app (optional)

Start an iOS simulator or an Android emulator, then:

```bash
cd customer-app
flutter pub get
flutter run
```

The app finds the backend on this computer by itself (`localhost` on the iOS simulator, `10.0.2.2` on the Android emulator).

## Using it

| What | Where | Sign in with |
| --- | --- | --- |
| Website | http://localhost:4321 | Register with any email address, or use the staff account |
| Admin | http://localhost:4200 | `admin@example.com` and your `DEMO_ADMIN_PASSWORD`: see the programme, films, bookings and check in tickets |
| API | http://localhost:8000/api/ | |
| Django admin | http://localhost:8000/admin/ | the staff account |
| Customer app | simulator or emulator | Register, or use the staff account |

Payments use the fake provider: no card is needed, and the flow (pending, confirmed, refused) is the same as with Stripe.

## Tests and checks

| Part | Command |
| --- | --- |
| backend | `.venv/bin/python manage.py test` |
| website | `npm run check` |
| admin | `npm run build` |
| customer-app | `flutter test` |

## License

The code is released under the [MIT License](LICENSE). The movie synopses in the backend data come from Wikipedia and stay under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/); the other movie facts come from Wikidata under CC0.

Part of the [`templates`](https://github.com/mattoznav/templates) collection.
