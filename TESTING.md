# 🧪 Testing Guide

Complete testing commands for all API routes with examples.

## 🚀 Starting the Server

```bash
node rest_server.mjs
```

Expected output:
```
Now listening on port 8000
Now connected to stpaul_crime.sqlite3
```

---

## 📋 Table of Contents

1. [GET /codes](#get-codes)
2. [GET /neighborhoods](#get-neighborhoods)
3. [GET /incidents](#get-incidents)
4. [PUT /new-incident](#put-new-incident)
5. [DELETE /remove-incident](#delete-remove-incident)
6. [Testing Workflow](#testing-workflow)

---

## GET /codes

### Basic Request
```bash
curl http://localhost:8000/codes
```

**Expected:** Array of all crime codes (ordered by code number)

### With Query Parameters

**Filter by specific codes:**
```bash
curl "http://localhost:8000/codes?code=110,700"
```

**Expected:** Only codes 110 and 700

**More examples:**
```bash
# Single code
curl "http://localhost:8000/codes?code=100"

# Multiple codes
curl "http://localhost:8000/codes?code=100,200,300,400"
```

---

## GET /neighborhoods

### Basic Request
```bash
curl http://localhost:8000/neighborhoods
```

**Expected:** Array of all 17 neighborhoods (ordered by ID)

### With Query Parameters

**Filter by specific IDs:**
```bash
curl "http://localhost:8000/neighborhoods?id=11,14"
```

**Expected:** Only neighborhoods 11 (Hamline/Midway) and 14 (Macalester-Groveland)

**More examples:**
```bash
# Single neighborhood
curl "http://localhost:8000/neighborhoods?id=1"

# Multiple neighborhoods
curl "http://localhost:8000/neighborhoods?id=1,5,10,15"
```

---

## GET /incidents

### Basic Request
```bash
curl http://localhost:8000/incidents
```

**Expected:** Array of 1000 most recent incidents (default limit)

### With Query Parameters

#### 1. Limit Results
```bash
# Get only 10 incidents
curl "http://localhost:8000/incidents?limit=10"

# Get 50 incidents
curl "http://localhost:8000/incidents?limit=50"

# Get 100 incidents
curl "http://localhost:8000/incidents?limit=100"
```

#### 2. Filter by Date Range
```bash
# Filter by start date
curl "http://localhost:8000/incidents?start_date=2019-09-01&limit=20"

# Filter by end date
curl "http://localhost:8000/incidents?end_date=2019-10-31&limit=20"

# Filter by date range
curl "http://localhost:8000/incidents?start_date=2019-09-01&end_date=2019-10-31&limit=50"

# Specific month (October 2019)
curl "http://localhost:8000/incidents?start_date=2019-10-01&end_date=2019-10-31"
```

#### 3. Filter by Crime Code
```bash
# Single code (Auto Theft)
curl "http://localhost:8000/incidents?code=700&limit=20"

# Multiple codes
curl "http://localhost:8000/incidents?code=110,700&limit=20"

# Violent crimes (examples)
curl "http://localhost:8000/incidents?code=100,110,120,210,220&limit=50"
```

#### 4. Filter by Police Grid
```bash
# Single grid
curl "http://localhost:8000/incidents?grid=38&limit=20"

# Multiple grids
curl "http://localhost:8000/incidents?grid=38,65&limit=20"

# Multiple grids with date
curl "http://localhost:8000/incidents?grid=38,65,87&start_date=2019-10-01&limit=50"
```

#### 5. Filter by Neighborhood
```bash
# Single neighborhood (Hamline/Midway)
curl "http://localhost:8000/incidents?neighborhood=11&limit=20"

# Multiple neighborhoods
curl "http://localhost:8000/incidents?neighborhood=11,14&limit=20"

# Specific neighborhood with date range
curl "http://localhost:8000/incidents?neighborhood=11&start_date=2019-10-01&end_date=2019-10-31"
```

#### 6. Combining Multiple Filters

**Example 1: Complex query with all parameters**
```bash
curl "http://localhost:8000/incidents?start_date=2019-10-01&end_date=2019-10-31&code=700,110&grid=38,65&neighborhood=11,14&limit=25"
```

**Example 2: Date range + neighborhood + limit**
```bash
curl "http://localhost:8000/incidents?start_date=2019-09-01&end_date=2019-09-30&neighborhood=5&limit=50"
```

**Example 3: Crime type + location**
```bash
curl "http://localhost:8000/incidents?code=700&neighborhood=11&limit=30"
```

**Example 4: Time period + specific grids**
```bash
curl "http://localhost:8000/incidents?start_date=2019-10-15&grid=87,95,100&limit=40"
```

---

## PUT /new-incident

### Add New Incident

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

**Expected Response:** `OK` (status 200)

### Verify It Was Added

```bash
curl "http://localhost:8000/incidents?limit=5"
```

Look for case number "TEST12345" in the results.

### Test Duplicate Prevention (Should Fail)

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

**Expected Response:** `Case number already exists` (status 500)

### Add More Test Data

```bash
# Test incident 2
curl -X PUT http://localhost:8000/new-incident \
  -H "Content-Type: application/json" \
  -d '{
    "case_number": "TEST67890",
    "date": "2024-01-16",
    "time": "09:15:00",
    "code": 700,
    "incident": "Test Auto Theft",
    "police_grid": 87,
    "neighborhood_number": 11,
    "block": "123 MAIN ST"
  }'

# Test incident 3
curl -X PUT http://localhost:8000/new-incident \
  -H "Content-Type: application/json" \
  -d '{
    "case_number": "TEST99999",
    "date": "2024-01-17",
    "time": "18:45:00",
    "code": 9954,
    "incident": "Test Proactive Visit",
    "police_grid": 95,
    "neighborhood_number": 7,
    "block": "456 OAK AVE"
  }'
```

---

## DELETE /remove-incident

### Remove an Incident

```bash
curl -X DELETE http://localhost:8000/remove-incident \
  -H "Content-Type: application/json" \
  -d '{"case_number": "TEST12345"}'
```

**Expected Response:** `OK` (status 200)

### Verify It Was Removed

```bash
curl "http://localhost:8000/incidents?limit=5"
```

Case number "TEST12345" should no longer appear.

### Test Non-Existent Deletion (Should Fail)

```bash
curl -X DELETE http://localhost:8000/remove-incident \
  -H "Content-Type: application/json" \
  -d '{"case_number": "TEST12345"}'
```

**Expected Response:** `Case number does not exist` (status 500)

### Clean Up Other Test Data

```bash
# Remove test incident 2
curl -X DELETE http://localhost:8000/remove-incident \
  -H "Content-Type: application/json" \
  -d '{"case_number": "TEST67890"}'

# Remove test incident 3
curl -X DELETE http://localhost:8000/remove-incident \
  -H "Content-Type: application/json" \
  -d '{"case_number": "TEST99999"}'
```

---

## 🔄 Testing Workflow

### Recommended Testing Order

1. **Start with GET routes to verify data is accessible:**
   ```bash
   curl http://localhost:8000/codes | head -20
   curl http://localhost:8000/neighborhoods
   curl "http://localhost:8000/incidents?limit=5"
   ```

2. **Test query parameters on GET routes:**
   ```bash
   curl "http://localhost:8000/codes?code=110,700"
   curl "http://localhost:8000/neighborhoods?id=11,14"
   curl "http://localhost:8000/incidents?start_date=2019-10-01&limit=10"
   ```

3. **Test PUT operation (add data):**
   ```bash
   curl -X PUT http://localhost:8000/new-incident \
     -H "Content-Type: application/json" \
     -d '{"case_number": "TEST12345", "date": "2024-01-15", "time": "14:30:00", "code": 9954, "incident": "Test", "police_grid": 100, "neighborhood_number": 5, "block": "TEST ST"}'
   ```

4. **Test duplicate prevention:**
   ```bash
   # Run the same PUT command again - should fail
   ```

5. **Test DELETE operation (remove data):**
   ```bash
   curl -X DELETE http://localhost:8000/remove-incident \
     -H "Content-Type: application/json" \
     -d '{"case_number": "TEST12345"}'
   ```

6. **Test non-existent deletion:**
   ```bash
   # Run the same DELETE command again - should fail
   ```

---

## 💡 Testing Tips

1. **Use `| json_pp` for pretty-printed JSON:**
   ```bash
   curl http://localhost:8000/codes | json_pp
   ```

2. **Use `| head` to limit output:**
   ```bash
   curl http://localhost:8000/incidents | head -50
   ```

3. **Save responses to files:**
   ```bash
   curl http://localhost:8000/incidents?limit=100 > incidents.json
   ```

4. **Test in a browser:**
   - Navigate to `http://localhost:8000/codes`
   - Navigate to `http://localhost:8000/incidents?limit=10`

5. **Use Postman or Insomnia for GUI testing**

6. **Check server console output** - each request is logged with query parameters

---

## ✅ Expected Test Results

### All Routes Working:
- ✅ GET /codes returns all codes (or filtered codes)
- ✅ GET /neighborhoods returns all neighborhoods (or filtered)
- ✅ GET /incidents returns incidents (with all filters working)
- ✅ PUT /new-incident adds new data (rejects duplicates)
- ✅ DELETE /remove-incident removes data (rejects non-existent)

### Query Parameters Working:
- ✅ Code filtering on /codes
- ✅ ID filtering on /neighborhoods
- ✅ All 6 parameters on /incidents (start_date, end_date, code, grid, neighborhood, limit)

---

## 🐛 Troubleshooting

**Server not starting?**
- Check if port 8000 is already in use
- Verify database file exists in `db/` folder
- Run `npm install` to ensure dependencies are installed

**Empty results?**
- Check your date ranges (data might not exist for that period)
- Verify filter values are valid (check codes/neighborhoods tables)

**PUT/DELETE not working?**
- Ensure `Content-Type: application/json` header is set
- Check JSON formatting in request body
- Verify case number format matches existing data

---

**Full Implementation Complete - All Routes and Query Parameters Working! 🎉**
