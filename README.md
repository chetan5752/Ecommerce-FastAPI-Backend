
# 🛒 Ecommerce FastAPI Backend

A full-featured backend for an ecommerce platform built with FastAPI, PostgreSQL, SQLAlchemy ORM, and Google OAuth 2.0 for authentication. Product images are stored in a simulated S3 environment using LocalStack.

---

## 🚀 Features

- FastAPI backend with modular architecture
- User authentication via Google OAuth 2.0
- PostgreSQL database using SQLAlchemy ORM
- JWT-based access/refresh token authentication
- Product CRUD with image upload to S3 (LocalStack)
- Role-based access (user/admin)
- LocalStack for S3-compatible object storage
- Alembic for database migrations
- Pydantic for request validation

---

## 🧱 Project Structure

```
.
├── app/
│   ├── api/
│   │   └── v1/
│   │       ├── auth
|   |       |     ├── endpoints.py
|   |       |     ├── model.py
|   |       |     ├── repository.py
|   |       |     ├── schema.py
|   |       |     └── service.py
│   │       ├── category
|   |       |     ├── endpoints.py
|   |       |     ├── model.py
|   |       |     ├── repository.py
|   |       |     └── schema.py
|   |       ├── product
|   |       |     ├── endpoints.py
|   |       |     ├── model.py
|   |       |     ├── repository.py
|   |       |     └── schema.py
|   |       └── user
|   |             ├── endpoints.py
|   |             ├── model.py
|   |             ├── repository.py
|   |             ├── schema.py
|   |             └── service.py
│   ├── core/
│   │   ├── config.py
│   │   └── security.py
│   ├── services/
│   │   ├── s3_service.py
|   |   ├── email_service.py
|   |   └── mock_email_service.py
│   ├── utils/
│   │   └── utils.py
│   ├── db/
│   │   ├── base.py
│   │   └── session.py
│   └── main.py
├── migrations/
│   └── env.py
├── requirements.txt
├── .env
├── README.md
└── docker-compose.yml
```

---

## 🔑 Authentication

- **Google OAuth 2.0**: Sign in users and generate JWT tokens.
- **JWT tokens**: Used for protected routes.
- **User roles**: Supports admin and customer access control.

---

## 🖼️ Product Images (S3 via LocalStack)

- Upload product images to a mock S3 bucket hosted in **LocalStack**.
- Uses `boto3` SDK to interact with the S3 service.
- LocalStack provides a local cloud stack for development.

### Setup LocalStack

```bash
docker run --rm -it -p 4566:4566 -p 4571:4571 localstack/localstack
```

### .env for S3

```
AWS_ACCESS_KEY_ID=test
AWS_SECRET_ACCESS_KEY=test
S3_BUCKET_NAME=product-images
S3_ENDPOINT=http://localhost:4566
```

---

## 🛠️ Setup Instructions

### Prerequisites
- Python 3.9 or later
- PostgreSQL database for storing user, category and product details
- A virtual environment is recommended.

### 1. Clone & Create Environment

```bash
git clone https://github.com/your-username/authora.git
cd authora
python -m venv venv
source venv/bin/activate
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Setup PostgreSQL

Make sure PostgreSQL is running and `.env` contains:

```
DATABASE_URL=postgresql+asyncpg://postgres:password@localhost:5432/database
```

### 4. Apply Migrations

```bash
alembic upgrade head
```

### 5. Run LocalStack (for S3)

```bash
docker run --rm -it -p 4566:4566 -p 4571:4571 localstack/localstack
```

### 6. Run the App

```bash
uvicorn app.main:app --reload
```

---

## 🔐 Google OAuth Setup

Register an app in [Google Cloud Console](https://console.cloud.google.com/):

```
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret
REDIRECT_URI=http://localhost:8000/api/v1/auth/google/callback
```

---

## 🔄 API Endpoints

### Auth
- `POST /auth/register` — Register User
- `POST /auth/verify-email` — User verify email
- `POST /auth/login` — User login
- `POST /auth/verify-resend-otp` — User Send OTP again for verification
- `POST /auth/forgot-password` — User Forget email Password
- `PUT /auth/reset-password` — User set new password
- `POST /auth/logout` — User Logout
- `GET /api/v1/auth/google/login` — Google Login Redirect
- `GET /api/v1/auth/google/callback` — Google Auth Callback

### User

- `GET /user/info` — User detail
- `PATCH /product/{id}` — Update user detail
- `DELETE /product/{id}` — Delete user by ID

### Category
- `GET /categories` — List categories
- `GET /categories/{id}` — Get category by unique ID.
- `POST /categories` — Create new category
- `PUT /categories/{id}` — Update specific category by its unique ID.
- `DELETE /categories/{id}` — Delete specific category by its unique ID.

### Products

- `GET /products/` — List products
- `GET /product/{id}` — Get product by id
- `POST /product` — Create new product
- `PUT /product/{id}` — Update product detail
- `DELETE /product/{id}` — Delete product by ID

---

## 📂 Upload Product Image Example

```bash
curl -X POST http://localhost:8000/api/v1/product/upload-image \
  -H "Authorization: Bearer <your-jwt-token>" \
  -F "file=@image.png"
```

---

## 📄 License

MIT
