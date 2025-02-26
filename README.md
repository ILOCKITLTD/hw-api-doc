# 📚 API Documentation

## 🌐 Host
**Base URL:** `https://hw-api.bebabeggie.com`

---

## 🚀 Workflow Overview
This API manages locker compartments, including setup, synchronization, session management, and compartment availability.

---

### 1. Initial Setup
On first boot:
- Assign all compartments as `small`.
- Send a request to `/setup` with the locker details.

#### 📥 Request to `/setup`
```json
{
  "lockerId": "some-id",
  "compartments": 4
}
```

#### 📤 SMS Instruction for Compartment Sizes
You will receive an SMS to configure compartment sizes:
```json
{
  "action": "setup",
  "lockers": ["s", "m", "l", "xl", "sc"]
}
```
**Key:**  
- `s` = Small  
- `m` = Medium  
- `l` = Large  
- `xl` = Extra Large  
- `sc` = Special Case  

---

### 2. Synchronize Active Compartments
When you receive an SMS with the action `sync-active`, make a request to report active compartments.

#### 📥 Request to `/sync-active`
```json
{
  "lockerId": "some-id",
  "active": [
    {
      "position": 0,
      "phone": "0700824555",
      "start": "1736263096525"
    },
    {
      "position": 3,
      "phone": "0700824555",
      "start": "1736263096525"
    }
  ]
}
```

---

### 3. End Session and Billing
When a customer completes their session, make a request to `/end-session` to notify the server.

#### 📥 Request to `/end-session`
```json
{
  "lockerId": "some-id",
  "compartment": {
    "position": 0,
    "phone": "0700824555",
    "start": "1736263096525",
    "end": "17489654386487"
  }
}
```

#### 📤 SMS Instruction to End Session
After calculating the duration and sending an STK push to the customer, you will receive an SMS to end the session:
```json
{
  "action": "end-session",
  "compartment": 6
}
```
**Action:** End the session for compartment 6 and open it.

---

### 4. Compartment Availability Management
You will receive an SMS to update compartment availability.

#### 📥 SMS Instruction for Availability
```json
{
  "action": "availability",
  "lockers": [1, 1, 0, 0, 1]
}
```
**Key:**  
- `0` = Not Available  
- `1` = Available  

#### 📥 SMS Instruction to Open Compartments
```json
{
  "action": "open",
  "lockers": [1, 0, 0, 0, 1]
}
```
**Action:** Open compartments marked with `1`.

---

## 📝 Additional Notes

### ✅ Request Success and Failure
- On a successful request, the response format will be:
  ```json
  {"success": true}
  ```
- On a failed request, the response format will include a failure message:
  ```json
  {"success": false, "message": "Reason why the request failed"}
  ```

### 🔄 Retry Mechanism
If a request fails due to a temporary issue (e.g., network error), retry the request after a short delay (e.g., 5 seconds). Avoid excessive retries to prevent overloading the server.

### 📡 Rate Limiting
The API enforces rate limiting to ensure fair usage. If you exceed the allowed number of requests, you will receive a `429 Too Many Requests` response. Wait for the specified `Retry-After` period before making further requests.

### 🔐 Security
All API requests must be made over HTTPS. Ensure that sensitive data (e.g., phone numbers) is encrypted in transit.

### 📄 Versioning
The API is versioned to ensure backward compatibility. The current version is `v1`. Future updates will be released under new version numbers (e.g., `v2`).

### 📧 Support
For any issues or questions, contact support at [support@bebabeggie.com](mailto:support@bebabeggie.com).

---
