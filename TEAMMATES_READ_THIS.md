# Query Parameter Implementation

You guys need to add query parameter filtering to get the remaining 10 points.

## What to do

Look for the TODO comments in `rest_server.mjs`. They tell you exactly what query params to add.

## Quick reference

**Parsing comma-separated values:**
```javascript
// if req.query.code exists and has value like "110,700"
if (req.query.code) {
    let codes = req.query.code.split(',');
    // codes is now ['110', '700']
}
```

**Adding WHERE clauses:**
```javascript
// start with base query
let query = 'SELECT * FROM Table';
let params = [];

// add conditions
if (req.query.code) {
    let codes = req.query.code.split(',');
    query += ' WHERE code IN (' + codes.map(() => '?').join(',') + ')';
    params.push(...codes);
}

// execute
db.all(query, params, (err, rows) => { ... });
```

**Multiple WHERE conditions:**
Use `WHERE` for first condition, `AND` for rest.

**Changing LIMIT:**
```javascript
let limit = req.query.limit || 1000; // default 1000
query += ' LIMIT ' + limit;
```

**Date filtering:**
```javascript
if (req.query.start_date) {
    query += ' WHERE date_time >= ?';
    params.push(req.query.start_date);
}
```

## Point breakdown

- `/codes` code filter: **2 pts**
- `/neighborhoods` id filter: **2 pts**
- `/incidents` all filters: **6 pts**

Split it however you want. Just don't break what's already working.

