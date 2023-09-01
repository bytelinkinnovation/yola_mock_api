
## YOLA Electric Mock API Documentation

### Access Token Validation and Authentication

This API uses role-based authentication, allowing only admins to add users but anyone with the test credentials can access the rest of the endpoints using the test login, if you want your personal login, send Credentials to author of Api.

To register, submit email & password to author for signup, or use the default `test` login provided below:
```json
{
    "email": "Test@example.com",
    "password": "password"
}
```

## Login to API Endpoint

- **Endpoint:** `POST https://bytelinkinnovation.com/users/login`
- **Description:** Allow user to login and have access to validate, View, and Generate mock tokens of any amount to customers.
- **Payload:**
```json
// Request body {data}
{
    "email": "Test@example.com",
    "password": "password"
}
```
- **Response:**
```json
{
    "status": 1,
    "message": "login successful",
    "token": "<your_jwt_token_here>" // copy this Bearer token and include it in all your requests headers;
}
```
Copy the *token* provided in the response and use it to make all requests. The login endpoint does not require token authentication.

- JWT token validation has a 1-hour expiration time so cache token for 1hr. if you see access denied during requests, just call the login endpoint again and copy the new token and insert to request header as Bearer.

## Example request with payload that does not require Token. Eg The Login Endpoint 
**Example Endpoint** `POST https://bytelinkinnovation.com/users/login`

**Axios Request** use any programming language of choice.
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
//passing payload
let data = JSON.stringify({
  "transactionId": "98a66210e4-3425-42ee-a907-a3195b77",
  "gateway_transaction_id": "c5f0-40ef-bce5-946810751487",
  "customerId": "98a660e4-3425-42ee-a907-93e2a3195b77",
  "transactionStatus": "success",
  "phoneNumber": "08133335511",
  "paymentRefNumber": "PG123456",
  "meterNumber": "30630113222",
  "amount": "20000.00"
});

let config = {
    method: 'get',
    maxBodyLength: Infinity,
    url: 'https://bytelinkinnovation.com/customers/validate/30405242080',
    headers: {
        // Always place Token afer Bearer
        'Authorization': 'Bearer <your_jwt_token_here>'
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
## Get All Customers Endpoint

**Description:** Retrieves all customers from the database.

- **Endpoint:** `GET https://bytelinkinnovation.com/customers`

```json
{
    "success": 1,
    "data": [
        {
            "customerId": "0408d142-dccd-4ce6-be04-1c992ec49200",
            "customerAccountNo": "261123030301",
            "customerName": "CIVIL DEFENCE OFFICE",
            "customerAddress": "BEHIND GAGI HOSPITAL GAGI AREA",
            "customerState": "SOKOTO",
            "feeder33kV": "33KV POWER STATION",
            "feeder11KV": "11KV DURBAWA",
            "regionalOffice": "Sokoto AO",
            "serviceCenter": "Sokoto AO",
            "meterNumber": "30630115829",
            "customerTransformer": "GAGI 2",
            "created_at": "2023-08-31T12:34:31.000Z",
            "updated_at": "2023-08-31T12:50:11.000Z",
            "phoneNumber": "08045785426"
        },
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
            "meterNumber": "30630113232",
            "customerTransformer": "GAGI 3",
            "created_at": "2023-08-31T12:43:31.000Z",
            "updated_at": "2023-08-31T12:50:22.000Z",
            "phoneNumber": "07145879652"
        }
    ]
}
```
-----
Feel free to reach out if you encounter any difficulty
