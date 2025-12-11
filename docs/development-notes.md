# Notes on query params

still need query parameter filtering for 10 more pts

## what needs to be done

check the TODOs in rest_server.mjs - basically need to add filtering to the 3 GET routes

**codes route (2 pts)** - filter by code like ?code=110,700  
**neighborhoods route (2 pts)** - filter by id like ?id=11,14  
**incidents route (6 pts)** - bunch of filters (dates, codes, grids, neighborhoods, limit)

## how to do it

the query params come in through req.query, so like if someone hits:
`/codes?code=110,700`

then req.query.code will be "110,700"

split it: `let codes = req.query.code.split(',')` gives you ['110', '700']

then modify the SQL query. for lists use IN clause:
```javascript
WHERE code IN (?, ?, ?)
```

for dates just use >= or <=:
```javascript
WHERE date_time >= ?
```

if you have multiple conditions use AND between them

for limit just replace the LIMIT number

## example structure

```javascript
let query = 'SELECT * FROM Table';
let params = [];

if (req.query.code) {
    let codes = req.query.code.split(',');
    query += ' WHERE code IN (' + codes.map(() => '?').join(',') + ')';
    params.push(...codes);
}

db.all(query, params, (err, rows) => {
    // same as before
});
```

the incidents one is the biggest since it has like 6 different params but same idea just keep adding conditions

assignment doc has all the specific query param names if you forget

