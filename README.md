# St. Paul Crime REST API

REST server for St. Paul crime data using Node.js, Express, and SQLite3

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

See `TESTING.md` for curl commands to test all routes. All routes are fully implemented and tested.

## API Routes

### Basic Routes
- `GET /codes` - get all crime codes (ordered by code number)
- `GET /neighborhoods` - get all neighborhoods (ordered by id)
- `GET /incidents` - get incidents with date/time split (ordered by most recent, default limit 1000)
- `PUT /new-incident` - add new incident to database (rejects duplicates)
- `DELETE /remove-incident` - remove incident from database (rejects non-existent)

### Query Parameters

**GET /codes**
- `?code=110,700` - filter by specific codes

**GET /neighborhoods**
- `?id=11,14` - filter by specific neighborhood ids

**GET /incidents**
- `?start_date=2019-09-01` - filter by start date
- `?end_date=2019-10-31` - filter by end date
- `?code=110,700` - filter by specific codes
- `?grid=38,65` - filter by police grid numbers
- `?neighborhood=11,14` - filter by neighborhood ids
- `?limit=50` - set max number of results (default 1000)

you can combine the params

## Status

**Full Implementation - 40/40 pts**

all 5 routes with query parameter support done

