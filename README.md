# St. Paul Crime REST API

REST server for St. Paul crime data

## Team
- Eli (author)
- Caiden (contributor)
- Charlotte (contributor)

## Setup

1. Clone this repo
2. Download the database from [here](https://github.com/tmarrinan/webdev-rest) and put `stpaul_crime.sqlite3` in a `db/` folder
3. Install dependencies:
```bash
npm install
```

## Run

```bash
node rest_server.mjs
```

Server runs on port 8000

## Testing

See `TESTING.md` for curl commands to test all routes. All 5 basic routes are working and tested.

## API Routes

- `GET /codes` - get all crime codes
- `GET /neighborhoods` - get all neighborhoods  
- `GET /incidents` - get last 1000 incidents
- `PUT /new-incident` - add new incident to database
- `DELETE /remove-incident` - remove incident from database

## Status

**Basic Implementation (30/40 pts - COMPLETE):**
- All 5 core routes implemented and tested
- Database setup and connected
- Project structure and configuration complete

## What's Left to Do (10 pts)

**For Caiden & Charlotte - Pick which tasks you want:**

### Task 1: Filter codes (2 pts)
Add query param to `/codes` route: `?code=110,700`
- Location: `rest_server.mjs` line ~64
- Returns only specified crime codes

### Task 2: Filter neighborhoods (2 pts)
Add query param to `/neighborhoods` route: `?id=11,14`
- Location: `rest_server.mjs` line ~83
- Returns only specified neighborhoods

### Task 3: Filter incidents (6 pts)
Add multiple query params to `/incidents` route:
- `?start_date=2019-09-01` (filter by start date)
- `?end_date=2019-10-31` (filter by end date)
- `?code=110,700` (filter by crime codes)
- `?grid=38,65` (filter by police grid)
- `?neighborhood=11,14` (filter by neighborhoods)
- `?limit=50` (change max results, default 1000)
- Location: `rest_server.mjs` line ~102

**Help:** Check `TEAMMATES_READ_THIS.md` for code examples. All TODOs are marked in the code.

