# 🚖 Uber Backend API Documentation

A complete backend API documentation for User and Captain authentication, profile, and logout functionalities.  
All endpoints use **JWT-based authentication** and follow **REST standards**.

---

# 🧑‍✈️ Captain Endpoints Documentation

## 📌 Endpoint: Register Captain  
`POST /captains/register`

### 📘 Description  
Registers a new **captain (driver)** in the system, including their vehicle details.  
Returns the captain data and a JWT token upon successful registration.

---

### 📥 Request Body

```json
{
  "fullname": {
    "firstname": "string (min 3 chars, required)",
    "lastname": "string (min 3 chars, optional)"
  },
  "email": "string (valid email, required)",
  "password": "string (min 6 chars, required)",
  "vehicle": {
    "color": "string (min 3 chars, required)",
    "plate": "string (min 3 chars, required)",
    "capacity": "integer (min 1, required)",
    "vehicleType": "string (car|motorcycle|auto, required)"
  }
}
{
  "fullname": {
    "firstname": "Alice",
    "lastname": "Smith"
  },
  "email": "alice.smith@example.com",
  "password": "strongPassword123",
  "vehicle": {
    "color": "Red",
    "plate": "XYZ1234",
    "capacity": 4,
    "vehicleType": "car"
  }
}
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "captain": {
    "_id": "656f1e2b8c1a2b0012345679",
    "fullname": {
      "firstname": "Alice",
      "lastname": "Smith"
    },
    "email": "alice.smith@example.com",
    "vehicle": {
      "color": "Red",
      "plate": "XYZ1234",
      "capacity": 4,
      "vehicleType": "car"
    }
  }
}


---
## Endpoint: Get User Profile

`GET /users/profile`

### Description
Returns the authenticated user's profile information. Requires a valid JWT token (sent as a cookie or Authorization header).

### Authentication
- Requires authentication (JWT token in cookie or `Authorization: Bearer <token>` header).

### Responses

#### Success
- **Status Code:** `200 OK`
- **Body:**
  ```json
  {
    "_id": "656f1e2b8c1a2b0012345678",
    "fullname": {
      "firstname": "John",
      "lastname": "Doe"
    },
    "email": "john.doe@example.com"
  }
  ```

#### Authentication Error
- **Status Code:** `401 Unauthorized`
- **Body:**
  ```json
  {
    "message": "Authentication required"
  }
  ```

---

## Endpoint: Logout User

`GET /users/logout`

### Description
Logs out the authenticated user by clearing the authentication cookie and blacklisting the JWT token.

### Authentication
- Requires authentication (JWT token in cookie or `Authorization: Bearer <token>` header).

### Responses

#### Success
- **Status Code:** `200 OK`
- **Body:**
  ```json
  {
    "message": "Logged Out"
  }
  ```

#### Authentication Error
- **Status Code:** `401 Unauthorized`
- **Body:**
  ```json
  {
    "message": "Authentication required"
  }
  ```

---

# User Endpoints Documentation


## Endpoint: Register User

`POST /users/register`

### Description
Registers a new user in the system. This endpoint creates a user account with the provided details and returns an authentication token along with the user data upon successful registration.


### Request Body

The request body must be a JSON object with the following structure:

```
{
  "fullname": {
    "firstname": "string (min 3 chars, required)",
    "lastname": "string (min 3 chars, optional)"
  },
  "email": "string (valid email, required)",
  "password": "string (min 6 chars, required)"
}
```


#### Example

```
{
  "fullname": {
    "firstname": "John",
    "lastname": "Doe"
  },
  "email": "john.doe@example.com",
  "password": "securePassword123"
}
```


### Responses

#### Success
- **Status Code:** `201 Created`
- **Body:**
  ```json
  {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "_id": "656f1e2b8c1a2b0012345678",
      "fullname": {
        "firstname": "John",
        "lastname": "Doe"
      },
      "email": "john.doe@example.com"
    }
  }
  ```

#### Validation Error
- **Status Code:** `400 Bad Request`
- **Body:**
  ```json
  {
    "errors": [
      {
        "msg": "First name must be 3 characters long",
        "param": "fullname.firstname",
        "location": "body"
      }
    ]
  }
  ```


---

## Endpoint: User Login

`POST /users/login`

### Description
Authenticates a user with email and password. Returns a JWT token and user data if credentials are valid.

### Request Body

The request body must be a JSON object with the following structure:

```
{
  "email": "string (valid email, required)",
  "password": "string (min 6 chars, required)"
}
```

#### Example

```
{
  "email": "john.doe@example.com",
  "password": "securePassword123"
}
```

### Responses

#### Success
- **Status Code:** `200 OK`
- **Body:**
  ```json
  {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "_id": "656f1e2b8c1a2b0012345678",
      "fullname": {
        "firstname": "John",
        "lastname": "Doe"
      },
      "email": "john.doe@example.com"
    }
  }
  ```

#### Validation Error
- **Status Code:** `400 Bad Request`
- **Body:**
  ```json
  {
    "errors": [
      {
        "msg": "Password must be 6 characters long",
        "param": "password",
        "location": "body"
      }
    ]
  }
  ```

#### Authentication Error
- **Status Code:** `401 Unauthorized`
- **Body:**
  ```json
  {
    "message": "Invalid email or password!"
  }
  ```

---

## Notes
- The `email` must be unique for registration.
- The `password` is stored securely (hashed).
- The response includes a JWT token for authentication.
