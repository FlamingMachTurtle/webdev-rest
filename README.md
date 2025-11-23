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

## API Routes

- `GET /codes` - get all crime codes
- `GET /neighborhoods` - get all neighborhoods  
- `GET /incidents` - get last 1000 incidents
- `PUT /new-incident` - add new incident to database
- `DELETE /remove-incident` - remove incident from database

## Work Distribution

**Basic Implementation (30/40 pts - DONE):**
- Eli: `/codes` GET route, project setup, .gitignore, package.json
- Caiden: `/incidents` GET route, `/remove-incident` DELETE route
- Charlotte: `/neighborhoods` GET route, `/new-incident` PUT route

**Query Parameters (10/40 pts - TODO):**
- See TODO comments in code for what needs to be added
- Worth 2+2+6 = 10 points total

