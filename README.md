## **📌 User Stories / API Test Plan – Shopify App on GitHub Marketplace**  

### **🚀 Overview**  
This document outlines the **API testing approach** for the **Shopify App on GitHub Marketplace**. The goal is to ensure smooth integration, proper data exchange, and adherence to expected functionalities.  

---

## **🔧 Tech Stack**

![Shopify](https://img.shields.io/badge/-Shopify-96bf48?logo=shopify)
![GitHub Marketplace](https://img.shields.io/badge/-GitHub%20Marketplace-181717?logo=github)
![REST API](https://img.shields.io/badge/-REST%20API-FF6F00?logo=api)
![JSON](https://img.shields.io/badge/-JSON-292929?logo=json)
![Postman](https://img.shields.io/badge/-Postman-FF6C37?logo=postman)
![Confluence](https://img.shields.io/badge/-Confluence-172B4D?logo=confluence)
![JavaScript](https://img.shields.io/badge/-JavaScript-F7DF1E?logo=javascript)
![Node.js](https://img.shields.io/badge/-Node.js-339933?logo=node.js)

---

## **🔁 Workflow Diagram (Mermaid)**

```mermaid
flowchart TD
  A(["Shopify Store Owner"]) --> B(["Login w/ API Key"])
  B --> C(["Token Generated"])
  C --> D(["GET /api/products"])
  C --> E(["PUT /api/products/update"])
  D --> F(["GitHub Marketplace App"])
  E --> F
  style A fill:#f0f8ff,stroke:#333,stroke-width:1px
  style B fill:#d3eaff,stroke:#333,stroke-width:1px
  style C fill:#add8e6,stroke:#333,stroke-width:1px
  style D fill:#b0e0e6,stroke:#333,stroke-width:1px
  style E fill:#b0e0e6,stroke:#333,stroke-width:1px
  style F fill:#e6f7ff,stroke:#333,stroke-width:1px
  D -->|"Product List"| F
  E -->|"Price Confirmation"| F
```

---

## **🔹 1. User Story: Authenticate Shopify Store**  

### **📖 User Story**  
**As a** Shopify store owner,  
**I want** to authenticate my store with the GitHub Marketplace app,  
**So that** I can securely sync my product data.  

### **📌 Acceptance Criteria & Test Scenarios**  

| **Criteria** | **Test Scenario** | **Pass/Fail** |
|-------------|------------------|-------------|
| The API should accept a valid Shopify store URL and API key. | Send a **POST** request with valid credentials. | ✅ Pass if API returns `200 OK` with `{"message": "Authentication successful"}`. ❌ Fail if API returns `401 Unauthorized` or incorrect response. |
| Invalid credentials should return an error message. | Send a **POST** request with incorrect credentials. | ✅ Pass if API returns `403 Forbidden` with `{"error": "Invalid API key"}`. ❌ Fail if authentication proceeds. |

### **📤 API Request & Response**  

#### **🔹 Request (POST /api/authenticate)**
```json
{
  "shop_url": "example.myshopify.com",
  "api_key": "valid_api_key_here"
}
```

#### **🔹 Response (Success - 200 OK)**
```json
{
  "message": "Authentication successful",
  "token": "generated_auth_token_here"
}
```

#### **🔹 Response (Error - 403 Forbidden)**
```json
{
  "error": "Invalid API key"
}
```

---

## **🔹 2. User Story: Retrieve Product Listings**  

### **📖 User Story**  
**As a** Shopify store owner,  
**I want** to fetch my store's product listings,  
**So that** I can view and manage products in the GitHub Marketplace app.  

### **📌 Acceptance Criteria & Test Scenarios**  

| **Criteria** | **Test Scenario** | **Pass/Fail** |
|-------------|------------------|-------------|
| The API should return a list of products when given a valid authentication token. | Send a **GET** request with a valid token. | ✅ Pass if API returns `200 OK` with product data. ❌ Fail if response is empty or incorrect. |
| Expired or missing tokens should return an authentication error. | Send a **GET** request without a token. | ✅ Pass if API returns `401 Unauthorized`. ❌ Fail if data is returned. |

### **📤 API Request & Response**  

#### **🔹 Request (GET /api/products)**
```http
GET /api/products HTTP/1.1
Authorization: Bearer generated_auth_token_here
Content-Type: application/json
```

#### **🔹 Response (Success - 200 OK)**
```json
{
  "products": [
    {
      "id": 1,
      "name": "Wireless Keyboard",
      "price": 79.99,
      "stock": 20
    },
    {
      "id": 2,
      "name": "Gaming Mouse",
      "price": 49.99,
      "stock": 15
    }
  ]
}
```

#### **🔹 Response (Error - 401 Unauthorized)**
```json
{
  "error": "Invalid or expired token"
}
```

---

## **🔹 3. User Story: Update Product Pricing**  

### **📖 User Story**  
**As a** Shopify store owner,  
**I want** to update my product prices through the GitHub Marketplace app,  
**So that** I can sync pricing changes across platforms.  

### **📌 Acceptance Criteria & Test Scenarios**  

| **Criteria** | **Test Scenario** | **Pass/Fail** |
|-------------|------------------|-------------|
| The API should allow updating product prices. | Send a **PUT** request with valid credentials and new price. | ✅ Pass if API returns `200 OK` with updated price. ❌ Fail if update is not reflected. |
| Invalid product ID should return an error. | Send a **PUT** request with a non-existent product ID. | ✅ Pass if API returns `404 Not Found`. ❌ Fail if product is updated incorrectly. |

### **📤 API Request & Response**  

#### **🔹 Request (PUT /api/products/update)**
```json
{
  "auth_token": "generated_auth_token_here",
  "product_id": 1,
  "new_price": 74.99
}
```

#### **🔹 Response (Success - 200 OK)**
```json
{
  "message": "Product price updated",
  "product": {
    "id": 1,
    "name": "Wireless Keyboard",
    "price": 74.99
  }
}
```

#### **🔹 Response (Error - 404 Not Found)**
```json
{
  "error": "Product ID not found"
}
```

---

# **🛠 Testing Execution Plan**  

1️⃣ **Setup API Authentication**  
✅ Verify Shopify store authentication.  
✅ Test invalid credentials.  

2️⃣ **Retrieve Product Listings**  
✅ Fetch product data with a valid token.  
✅ Validate error response for missing/expired tokens.  

3️⃣ **Update Product Pricing**  
✅ Test valid price updates.  
✅ Verify error handling for invalid product IDs.  

---

## **📌 Additional Recommendations**  

### 🔒 **Security Best Practices**
- Always pass authentication tokens in the `Authorization` header, not in the body.
- Use HTTPS for all endpoints.
- Rotate API keys periodically and use scopes for fine-grained access control.

### 🔁 **Rate Limiting**
- Consider testing with frequent calls to confirm if rate limiting or throttling occurs.
- Include headers in response like `X-RateLimit-Remaining` for observability.

### ⚠️ **Error Logging and Monitoring**
- Capture logs of failed API calls with timestamps.
- Monitor for abnormal patterns like spikes in 500-series responses.

### 🔐 **Token Lifecycle**
- Document time-to-live (TTL) for auth tokens.
- Handle refresh token workflows if applicable.

---

## **📂 Next Steps: GitHub Repository Setup**  

### **🔹 Step 1: Create a New Repository**  
1️⃣ Go to [GitHub](https://github.com/) and log in.  
2️⃣ Click **"New repository"**.  
3️⃣ Name it **`shopify-api-test-plan`**.  
4️⃣ Add a **README.md** and paste this document.  
5️⃣ Click **"Create repository"**.  

### **🔹 Step 2: Share Your GitHub Link**  
Once uploaded, copy your **GitHub repository URL** and share it on Confluence and LinkedIn.  

---

🚀 **Do you approve this format?** Let me know if you'd like modifications before finalizing it for GitHub! 🔥












