
## YOLA Electric Mock API Documentation

### Access Token Validation and Authentication

This API uses role-based authentication, allowing only admins to add users who can access endpoints using their JWT token.

To register, submit an email and password, or use the default login provided below:
```json
{
    "email": "Test@example.com",
    "password": "password"
}
```

## Login to API Endpoint

- **Endpoint:** `POST https://bytelinkinnovation.com/users/login`
- **Payload:**
```json
{
    "email": "Test@example.com",
    "password": "password"
}
```

You can either use the test email and password or your custom email and password submitted to the admin.

- **Response:**
```json
{
    "status": 1,
    "message": "login successful",
    "token": "<your_jwt_token_here>"
}
```
Copy the *token* provided in the response and use it to make requests. The login endpoint does not require token authentication.

- JWT token validation has a 1-hour expiration time.

## Example request with payload and token to Login, applies to all endpoints using axios

```js
const axios = require('axios');
let data = JSON.stringify({
    "email": "Test@example.com",
    "password": "password"
});

let config = {
    method: 'post',
    maxBodyLength: Infinity,
    url: 'https://bytelinkinnovation.com/users/login',
    headers: { 
        'Content-Type': 'application/json'
    },
    data: data
};

axios.request(config)
.then((response) => {
    console.log(JSON.stringify(response.data));
})
.catch((error) => {
    console.log(error);
});
```

### Customer Validation Endpoint

- **Endpoint:** `POST https://bytelinkinnovation.com/customers/validate/{{meterNumber}}`
- **Description:** Validates a customer by passing the meter number as a URL parameter.
- **Payload:** `POST https://bytelinkinnovation.com/customers/validate/30405242080`

- **Response:**
```json
{
    "status": 1,
    "data": [
        {
            "customerId": "876c6c18-ee89-45b0-9d20-d06e73fc80f8",
            "customerAccountNo": "261123880812",
            "customerName": "Mohammed Ak Yerima",
            "customerAddress": "Number 36 birnin kebbi crescent garki abuja",
            "customerState": "Maiduguri",
            "feeder33kV": "33KV POWER STATION",
            "feeder11KV": "11KV DURBAWA",
            "regionalOffice": "Maiduguri AO",
            "serviceCenter": "Maiduguri AO",
            "meterNumber": "30405242080",
            "customerTransformer": "GAGI 3",
            "created_at": "2023-08-31T12:43:31.000Z",
            "updated_at": "2023-09-01T01:52:45.000Z",
            "phoneNumber": "07145879652"
        }
    ]
}
```

## Example Axios Request with Bearer Token

Demonstrates how to pass a token and payload using Axios in a request.

```javascript
const axios = require('axios');

let config = {
    method: 'get',
    maxBodyLength: Infinity,
    url: 'https://bytelinkinnovation.com/customers/validate/30405242080',
    headers: { 
        'Authorization': 'Bearer <your_jwt_token_here>'
    }
};

axios.request(config)
.then((response) => {
    console.log(JSON.stringify(response.data));
})
.catch((error) => {
    console.log(error);
});
```

### Token Generation Endpoint

- **Endpoint:** `POST https://bytelinkinnovation.com/api/token/generate`
- **Description:** Generates a token based on a successful payment transaction.
- **Payload:**
```json
{
    "transactionId": "98a66210e4-3425-42ee-a907-a3195b77",
    "gateway_transaction_id" : "c5f0-40ef-bce5-946810751487",
    "customerId": "98a660e4-3425-42ee-a907-93e2a3195b77",
    "transactionStatus": "success",
    "phoneNumber": "08133335511",
    "paymentRefNumber": "PG123456",
    "meterNumber" :"30630113222",
    "amount": "20000.00"
}
```

- **Response:**
```json
{
    "success": 1,
    "data": [
        {
            "token_id": 13,
            "customer_id": "98a660e4-3425-42ee-a907-93e2a3195b77",
            "meter_number": "30630113222",
            "amount": 20000,
            "token_code": "5229 0580 1831 7295 ",
            "purchase_date": "2023-08-31T19:16:08.000Z",
            "company": "YOLA",
            "created_at": "2023-08-31T19:16:08.000Z",
            "updated_at": "2023-08-31T19:16:08.000Z",
            "totalUnitVended": 150.12,
            "transactionId": "98a66210e4-3425-42ee-a907-a3195b77",
            "gateway_transaction_id": "c5f0-40ef-bce5-946810751487",
            "transactionStatus": "success",
            "paymentRefNumber": "PG123456",
            "phoneNumber": "08133335511",
            "customerName": "Idris Bukar"
        }
    ]
}
```

### All Transaction Retrieval Endpoint

- **Endpoint:** `GET https://bytelinkinnovation.com/api/transactions`
- **Description:** Retrieves all completed transactions.
- **Response:**
```json
{
    "transactions": [
        {
            "token_id": 2,
            "customer_id": "876c6c18-ee89-45b0-9d20-d06e73fc80f8",
            "meter_number": "30630113232",
            "amount": 40000.12,
            "token_code": "2134 1690 0081 2345",
            "purchase_date": "2023-08-31T14:58:14.000Z",
            "company": "YOLA",
            "created_at": "2023-08-31T14:58:14.000Z",
            "updated_at": "2023-08-31T15:58:57.000Z",
            "totalUnitVended": 400.43,
            "transactionId": "ab1c25d6-9e8f-4a1b-85c3-6721d0e9a3a4",
           

 "gateway_transaction_id": "c5f0-40ef-bce5-946900751487",
            "transactionStatus": "success",
            "paymentRefNumber": "PG123313",
            "phoneNumber": "07145879652",
            "customerName": "Mohammed Ak Yerima"
        },
        {
            "token_id": 1,
            "customer_id": "98a660e4-3425-42ee-a907-93e2a3195b77",
            "meter_number": "30630113222",
            "amount": 40000.23,
            "token_code": "1131 1332 1232 1212",
            "purchase_date": "2023-08-31T14:12:25.000Z",
            "company": "YOLA",
            "created_at": "2023-08-31T14:12:25.000Z",
            "updated_at": "2023-08-31T15:58:46.000Z",
            "totalUnitVended": 98.35,
            "transactionId": "fe0e15b2-c5f0-40ef-bce5-946900751487",
            "gateway_transaction_id": "c5f0-40ef-bce5-946900751487",
            "transactionStatus": "success",
            "paymentRefNumber": "PG123456",
            "phoneNumber": "08133335511",
            "customerName": "Idris Bukar"
        }
    ]
}
```

### Customer-Specific Transactions

- **Endpoint:** `GET https://bytelinkinnovation.com/api/customer/transactions/{meterNumber}`
- **Description:** Retrieves completed transactions for a specific customer.
- **Response:**
```json
{
    "transactions": [
        {
            "token_id": 13,
            "customer_id": "98a660e4-3425-42ee-a907-93e2a3195b77",
            "meter_number": "30630113222",
            "amount": 20000,
            "token_code": "5229 0580 1831 7295 ",
            "purchase_date": "2023-08-31T19:16:08.000Z",
            "company": "YOLA",
            "created_at": "2023-08-31T19:16:08.000Z",
            "updated_at": "2023-08-31T19:16:08.000Z",
            "totalUnitVended": 150.12,
            "transactionId": "98a66210e4-3425-42ee-a907-a3195b77",
            "gateway_transaction_id": "c5f0-40ef-bce5-946810751487",
            "transactionStatus": "success",
            "paymentRefNumber": "PG123456",
            "phoneNumber": "08133335511",
            "customerName": "Idris Bukar"
        }
    ]
}
```

-----
Feel free to reach out if you encounter any difficulty
