# User AUTH API SPEC

## Register User

Endpoint : POST /api/auth/register

Request Body :

```json
{
    "name" : "New Owner",
    "username" : "newownerkasir",
    "email" : "newownerkasir@mail.com",
    "msisdn" : "+6281122334455",
    "password" : "password" // Hash pass in backend
}
```

Response Body (Success, 201) :

```json
{
    "statusCode": 201,
    "statusMessage": "Created",
    "statusDescription": "Resource created successfully",
    "result": {
        "successMessage": "User register successfully",
        "data": {
            "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        }
    }
}
```

Response Body (Failed, 422) :

```json
{
    "statusCode": 422,
    "statusMessage": "Unprocessable Entity",
    "statusDescription": "Validation failed for the given request",
    "result": {
        "errorCode": "21",
        "errorMessage": "Validation failed",
        "errors": {
            "email": [
                "The email field must be a valid email address."
            ],
            "msisdn": [
                "The msisdn field must be a string."
            ]
        },
    }
}
```

## Login User

Endpoint : POST /api/auth/login

Request Body :

```json
{
    "email": "userkasir@mail.com",
    "password": "passwordkasir" // Hash pass
}
```

Response Body (Success, 200) :

```json
{
    "statusCode": 200,
    "statusMessage": "OK",
    "statusDescription": "Request processed successfully",
    "result": {
        "data" : {
            "access_token": "stringToken",
            "token_type": "bearer",
            "expired_at": 72000, // millieseconds
            "user": {
                "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                "roles": [
                    "owner"
                ],
                "flags": {
                    "has_subscription": false, //false kalau user baru register
                    "has_outlet": false //false kalau user baru register
                }
            }
        }
    }
}
```

Response Body (Failed, 401) :

```json
{
    "statusCode": 401,
    "statusMessage": "Unauthorized",
    "statusDescription": "Authentication failed due to invalid credentials",
    "result": {
        "errorCode": "1",
        "errorMessage": "Invalid credentials | Invalid username/email or password"
    }
}
```

## Get User

Endpoint : GET /api/users/current

Request Header :

- Authorization: Bearer token
- Accept: application/json

// JWT (tymon/jwt-auth)

Response Body (Success, 200) :

```json
{
    "statusCode": 200,
    "statusMessage": "OK",
    "statusDescription": "Request processed successfully",
    "result": {
        "data": {
            "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "username": "userkasir",
            "name": "Nama Kasir",
            "email": "userkasir@email.com",
            "msisdn": "+6281122334455",
            "is_active": true,
            "subscriptions": [
                {
                    "subscription_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                    "subscription_type": 1,
                    "subscription_outlets": [
                        {
                            "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                            "name": "Nama Toko",
                            "address": "Jln Nama Jalan No.99, Kota, Provinsi, Negara",
                            "msisdn": "+6281122334455",
                            "is_active": true,
                            "created_at": "2025-12-17T13:13:31.178830Z",
                            "updated_at": "2025-12-17T13:13:31.178830Z"
                        }
                    ],
                    "subscription_status": "active",
                    "valid_from": "2025-12-17T13:13:31.178830Z",
                    "valid_to": "2026-01-17T13:13:31.178830Z",
                }
            ],
            "roles" : [
                {
                    "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                    "name": "Owner Kasir",
                    "slug": "owner",
                    "permissions" : [
                        {
                            "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
                            "name": "Super Admin",
                            "slug": "*",
                            "description": "deskripsi permission",
                            "created_at": "2025-12-17T13:13:31.178830Z",
                            "updated_at": "2025-12-17T13:13:31.178830Z"
                        }
                    ]
                }
            ],
            "created_at": "2025-12-17T13:13:31.178830Z",
            "updated_at": "2025-12-17T13:13:31.178830Z"
        }
    }
}
```

Response Body (Failed, 401) :

```json
{
    "statusCode": 401,
    "statusMessage": "Unauthorized",
    "statusDescription": "Authentication token expired or invalid or not not provided",
    "result": {
        "errorCode": "2 or 3 or 4",
        "errorMessage": "Token expired or invalid or not provided"
    }
}
```

Response Body (Failed, 404) :

```json
{
    "statusCode": 404,
    "statusMessage": "Not Found",
    "statusDescription": "The requested resource was not found on the server",
    "result": {
        "errorCode": "28",
        "errorMessage": "Data not found"
    }
}
```

## Logout User

Endpoint : DELETE /api/auth/logout

Request Header :

- Authorization: Bearer token
- Accept: application/json

Response Body (Success, 200) :

```json
{
    "statusCode": 200,
    "statusMessage": "OK",
    "statusDescription": "Logout completed successfully",
    "result": {
        "successMessage": "Logout successfully"
    }
}
```

Response Body (Failed, 401) :

```json
{
    "statusCode": 401,
    "statusMessage": "Unauthorized",
    "statusDescription": "Authentication token expired or invalid or not not provided",
    "result": {
        "errorCode": "2 or 3 or 4",
        "errorMessage": "Token expired or invalid or not provided"
    }
}
```