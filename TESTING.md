# Testing the REST API

Server should be running on port 8000. Start with `node rest_server.mjs`

## Test GET Routes

**Get all crime codes:**
```bash
curl http://localhost:8000/codes
```

**Get all neighborhoods:**
```bash
curl http://localhost:8000/neighborhoods
```

**Get last 1000 incidents:**
```bash
curl http://localhost:8000/incidents
```

## Test PUT Route (Add Incident)

**Add new incident:**
```bash
curl -X PUT http://localhost:8000/new-incident \
  -H "Content-Type: application/json" \
  -d '{
    "case_number": "TEST12345",
    "date": "2024-01-15",
    "time": "14:30:00",
    "code": 9954,
    "incident": "Test Incident",
    "police_grid": 100,
    "neighborhood_number": 5,
    "block": "XXX TEST ST"
  }'
```

Should return: `OK`

**Try adding same case_number again (should fail):**
```bash
curl -X PUT http://localhost:8000/new-incident \
  -H "Content-Type: application/json" \
  -d '{
    "case_number": "TEST12345",
    "date": "2024-01-15",
    "time": "14:30:00",
    "code": 9954,
    "incident": "Test Incident",
    "police_grid": 100,
    "neighborhood_number": 5,
    "block": "XXX TEST ST"
  }'
```

Should return: `Case number already exists` (status 500)

## Test DELETE Route (Remove Incident)

**Delete the test incident:**
```bash
curl -X DELETE http://localhost:8000/remove-incident \
  -H "Content-Type: application/json" \
  -d '{"case_number": "TEST12345"}'
```

Should return: `OK`

**Try deleting again (should fail):**
```bash
curl -X DELETE http://localhost:8000/remove-incident \
  -H "Content-Type: application/json" \
  -d '{"case_number": "TEST12345"}'
```

Should return: `Case number does not exist` (status 500)

## Expected Results

All 5 routes should work:
- GET /codes - returns JSON array of codes
- GET /neighborhoods - returns JSON array of neighborhoods
- GET /incidents - returns JSON array with date/time split
- PUT /new-incident - adds to DB, rejects duplicates
- DELETE /remove-incident - removes from DB, rejects non-existent

This gets you 30/40 points (C-grade). Query parameters worth 10 more points.

