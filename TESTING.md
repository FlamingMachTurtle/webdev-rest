# testing

run the server: `node rest_server.mjs` (port 8000)

## GET routes

**codes:**
```bash
curl http://localhost:8000/codes
```

**neighborhoods:**
```bash
curl http://localhost:8000/neighborhoods
```

**incidents:**
```bash
curl http://localhost:8000/incidents
```

## PUT route

**add incident:**
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

returns `OK`

**try duplicate (should fail with error):**
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

returns `Case number already exists` (500)

## DELETE route

**remove incident:**
```bash
curl -X DELETE http://localhost:8000/remove-incident \
  -H "Content-Type: application/json" \
  -d '{"case_number": "TEST12345"}'
```

returns `OK`

**try again (should fail):**
```bash
curl -X DELETE http://localhost:8000/remove-incident \
  -H "Content-Type: application/json" \
  -d '{"case_number": "TEST12345"}'
```

returns `Case number does not exist` (500)

---

all 5 routes working = 30/40 pts. still need query params for the last 10.

