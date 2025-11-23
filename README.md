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

## Still TODO (10 pts)

Need to add query parameter filtering to the 3 GET routes. See TODOs in rest_server.mjs

- `/codes` - filter by code param (2 pts)
- `/neighborhoods` - filter by id param (2 pts)
- `/incidents` - add filters for dates, codes, grids, neighborhoods, and limit (6 pts)

Check NOTES.md for examples on how to do the filtering. Pretty straightforward, just modify the SQL queries based on req.query params.

