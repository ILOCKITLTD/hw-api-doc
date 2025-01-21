### API Documentation

#### Host
**Base URL:** `hw-api.bebabeggie.com`

### Endpoints & Data Formats

#### 1. **Sync**
**Endpoint:** `hw-api.bebabeggie.com/sync`

**Request Data Format:**
```json
{
  "lockerId": "uniqueSerial",
  "compartments": [
    {"size": 0, "position": 1},
    {"size": 0, "position": 2}
  ]
}
```

**Example with Active Session:**
If a compartment (e.g., position 1) has an active session:
```json
{
  "lockerId": "uniqueSerial",
  "compartments": [
    {
      "size": 0,
      "position": 1,
      "session": {
        "phone": "0700824555",
        "start": "1736263096525",
        "end": "-",
        "duration": "-"
      }
    },
    {"size": 0, "position": 2}
  ]
}
```

---

#### 2. **Bill**
**Endpoint:** `hw-api.bebabeggie.com/bill`

**Request Data Format:**
```json
{
  "lockerId": "uniqueSerial",
  "compartment": 3,
  "phone": "0700824555",
  "start": "1736263096525",
  "end": "1736263096525",
  "duration": "120"
}
```

**Behavior After Request:**
- After the bill is sent, the server will send an SMS and the session should end.
- To end the session, the API will send a payload to open the compartment:
```json
{"open": 3}
```
- **Note:** If you receive `{"open": 3}` but no bill was sent for the compartment, just open the compartment without ending its session.

---

#### 3. **Compartment Availability Management**

##### **Mark Compartments Unavailable**
**Payload:**
```json
{"unavailable": [1, 2, 3]}
```
**Action:** Mark compartments 1, 2, and 3 as unavailable.

##### **Open Compartment Without Changing Availability**
**Payloads:**
- Open compartment 1:
  ```json
  {"open": 1}
  ```
- Open compartment 3:
  ```json
  {"open": 3}
  ```
**Action:** Open the specified compartment but do not make it available if it was previously marked as unavailable.

##### **Make Compartments Available**
**Payloads:**
- Make compartments 1 and 3 available:
  ```json
  {"available": [1, 3]}
  ```
- Make compartment 2 available:
  ```json
  {"available": [2]}
  ```
**Action:** Mark the specified compartments as available.

---

### Additional Notes

#### Request Success and Failure
- On a successful request, the response format should be:
  ```json
  {"success": true}
  ```
- On a failed request, the response format should include a failure message:
  ```json
  {"success": false, "message": "Reason why the request failed"}
  ```

#### Data Format Conventions
- **start** and **end:** Represented as timestamps.
- **duration:** Represented in minutes.

#### Compartment Size Key
- `0`: Small
- `1`: Medium
- `2`: Large

