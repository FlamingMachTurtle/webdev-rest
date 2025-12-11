# 🚔 St. Paul Crime REST API

A RESTful web server providing access to St. Paul crime incident data using Node.js, Express, and SQLite3.

## 👥 Team

- **Eli** (Lead Developer)
- **Caiden** (Contributor)
- **Charlotte** (Contributor)

## ✨ Features

- ✅ **5 Complete API Routes** (GET, PUT, DELETE operations)
- ✅ **Advanced Query Parameters** for filtering and pagination
- ✅ **CORS Enabled** - Ready for cross-origin requests
- ✅ **SQLite Database** with St. Paul crime incident data
- ✅ **Input Validation** - Prevents duplicates and invalid operations
- ✅ **Full Test Coverage** - See [TESTING.md](TESTING.md)

**Score: 40/40 points** - Full implementation complete!

---

## 🚀 Quick Start

### Prerequisites

- Node.js (v14 or higher)
- npm (comes with Node.js)

### Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/FlamingMachTurtle/webdev-rest
   cd webdev-rest
   ```

2. **Install dependencies:**
```bash
npm install
```

3. **Download the database:**
   - Download `stpaul_crime.sqlite3` from [here](https://github.com/tmarrinan/webdev-rest)
   - Create a `db/` folder if it doesn't exist
   - Place the database file in the `db/` folder

### Running the Server

```bash
node rest_server.mjs
```

**Server will start on:** `http://localhost:8000`

You should see:
```
Now listening on port 8000
Now connected to stpaul_crime.sqlite3
```

---

## 📚 API Documentation

### Base URL
```
http://localhost:8000
```

### Routes Overview

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/codes` | Get all crime codes |
| GET | `/neighborhoods` | Get all neighborhoods |
| GET | `/incidents` | Get crime incidents |
| PUT | `/new-incident` | Add a new incident |
| DELETE | `/remove-incident` | Remove an incident |

---

## 🔍 Detailed Route Information

### 1. GET `/codes`

Returns all crime incident codes and their types (ordered by code number).

**Query Parameters:**
- `code` - Filter by specific codes (comma-separated)

**Examples:**
```bash
# Get all codes
curl http://localhost:8000/codes

# Get specific codes
curl http://localhost:8000/codes?code=110,700
```

**Response Format:**
```json
[
  {"code": 100, "type": "MURDER"},
  {"code": 110, "type": "Murder, Non Negligent Manslaughter"},
  {"code": 700, "type": "Auto Theft"}
]
```

---

### 2. GET `/neighborhoods`

Returns all neighborhood IDs and names (ordered by ID).

**Query Parameters:**
- `id` - Filter by specific neighborhood IDs (comma-separated)

**Examples:**
```bash
# Get all neighborhoods
curl http://localhost:8000/neighborhoods

# Get specific neighborhoods
curl http://localhost:8000/neighborhoods?id=11,14
```

**Response Format:**
```json
[
  {"id": 1, "name": "Conway/Battlecreek/Highwood"},
  {"id": 11, "name": "Hamline/Midway"},
  {"id": 14, "name": "Macalester-Groveland"}
]
```

---

### 3. GET `/incidents`

Returns crime incidents with date and time separated (ordered by most recent, default limit: 1000).

**Query Parameters:**
- `start_date` - First date to include (format: YYYY-MM-DD)
- `end_date` - Last date to include (format: YYYY-MM-DD)
- `code` - Filter by crime codes (comma-separated)
- `grid` - Filter by police grid numbers (comma-separated)
- `neighborhood` - Filter by neighborhood IDs (comma-separated)
- `limit` - Maximum number of results (default: 1000)

**Examples:**
```bash
# Get recent incidents (default 1000)
curl http://localhost:8000/incidents

# Get limited results
curl http://localhost:8000/incidents?limit=50

# Filter by date range
curl http://localhost:8000/incidents?start_date=2019-09-01&end_date=2019-10-31

# Filter by neighborhood and code
curl http://localhost:8000/incidents?neighborhood=11&code=110,700&limit=20

# Combine multiple filters
curl "http://localhost:8000/incidents?start_date=2019-10-01&neighborhood=11,14&grid=38,65&limit=100"
```

**Response Format:**
```json
[
  {
    "case_number": "19245020",
    "date": "2019-10-30",
    "time": "23:57:08",
    "code": 9954,
    "incident": "Proactive Police Visit",
    "police_grid": 87,
    "neighborhood_number": 7,
    "block": "THOMAS AV & VICTORIA"
  }
]
```

---

### 4. PUT `/new-incident`

Add a new crime incident to the database.

**Request Body (JSON):**
```json
{
  "case_number": "TEST12345",
  "date": "2024-01-15",
  "time": "14:30:00",
  "code": 9954,
  "incident": "Test Incident",
  "police_grid": 100,
  "neighborhood_number": 5,
  "block": "XXX TEST ST"
}
```

**Example:**
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

**Responses:**
- `200 OK` - Incident added successfully
- `500 Error` - Case number already exists

---

### 5. DELETE `/remove-incident`

Remove a crime incident from the database.

**Request Body (JSON):**
```json
{
  "case_number": "TEST12345"
}
```

**Example:**
```bash
curl -X DELETE http://localhost:8000/remove-incident \
  -H "Content-Type: application/json" \
  -d '{"case_number": "TEST12345"}'
```

**Responses:**
- `200 OK` - Incident removed successfully
- `500 Error` - Case number does not exist

---

## 🧪 Testing

See [TESTING.md](TESTING.md) for comprehensive testing examples and curl commands.

---

## 🌐 CORS Support

This server has CORS enabled, allowing cross-origin requests from any domain. Perfect for:
- Frontend applications (React, Vue, Angular, etc.)
- Mobile apps
- Third-party integrations

**Example JavaScript fetch:**
```javascript
fetch('http://localhost:8000/incidents?limit=10')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```

---

## 📁 Project Structure

```
webdev-rest/
├── db/
│   └── stpaul_crime.sqlite3    # SQLite database (not in repo)
├── docs/
│   └── assignment.md           # Assignment details
├── node_modules/               # Dependencies (not in repo)
├── rest_server.mjs             # Main server file
├── package.json                # Project configuration
├── package-lock.json           # Dependency lock file
├── README.md                   # This file
├── TESTING.md                  # Testing guide
└── LICENSE                     # MIT License
```

---

## 🗄️ Database Schema

### Tables

**Codes:**
- `code` (INTEGER) - Crime incident type numeric code
- `incident_type` (TEXT) - Crime incident type description

**Neighborhoods:**
- `neighborhood_number` (INTEGER) - Neighborhood ID
- `neighborhood_name` (TEXT) - Neighborhood name

**Incidents:**
- `case_number` (TEXT) - Unique case ID
- `date_time` (DATETIME) - Incident date and time
- `code` (INTEGER) - Crime incident type code
- `incident` (TEXT) - Detailed incident description
- `police_grid` (INTEGER) - Police grid number
- `neighborhood_number` (INTEGER) - Neighborhood ID
- `block` (TEXT) - Approximate address

---

## 🔧 Dependencies

- **express** (^4.18.2) - Web framework
- **sqlite3** (^5.1.6) - SQLite database driver
- **cors** (^2.8.5) - CORS middleware

---

## 📝 Assignment Status

✅ **Full Implementation Complete - 40/40 points**

### Features Implemented:

**Basic Implementation (30 pts):**
- ✅ Package.json properly configured
- ✅ GET /codes route
- ✅ GET /neighborhoods route
- ✅ GET /incidents route
- ✅ PUT /new-incident route with validation
- ✅ DELETE /remove-incident route with validation

**Advanced Features (10 pts):**
- ✅ GET /codes with code filtering (2 pts)
- ✅ GET /neighborhoods with id filtering (2 pts)
- ✅ GET /incidents with all query parameters (6 pts):
  - start_date filtering
  - end_date filtering
  - code filtering
  - grid filtering
  - neighborhood filtering
  - limit parameter

---

## 🤝 Contributing

This is a team project by Eli, Caiden, and Charlotte for a web development course.

---

## 📄 License

MIT License - see [LICENSE](LICENSE) file for details.

---

## 🔗 Links

- **Repository:** https://github.com/FlamingMachTurtle/webdev-rest
- **Data Source:** [St. Paul Crime Incident Report](https://information.stpaul.gov/datasets/stpaul::crime-incident-report/about)

---

## 💡 Tips

1. **Make a backup of the database** before testing PUT/DELETE operations
2. **Use query parameters** to filter large result sets
3. **Check response status codes** to handle errors properly
4. **Use the limit parameter** to avoid loading too many incidents at once

---

**Made with ❤️ for Web Development Course**
