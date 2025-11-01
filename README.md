# Talabat E-Commerce API

Welcome to the **Talabat E-Commerce API** - A comprehensive RESTful API built with ASP.NET Core 6.0 for managing an e-commerce platform. This API provides complete functionality for product management, user authentication, shopping baskets, order processing, and payment integration.

---

## Table of Contents
1. [Project Overview](#project-overview)
2. [Architecture](#architecture)
3. [Technology Stack](#technology-stack)
4. [Project Structure](#project-structure)
5. [Features](#features)
6. [Getting Started](#getting-started)
7. [Configuration](#configuration)
8. [API Endpoints](#api-endpoints)
9. [Authentication](#authentication)
10. [Database Setup](#database-setup)
11. [Services & Integrations](#services--integrations)
12. [Error Handling](#error-handling)
13. [Testing](#testing)
14. [Support](#support)

---

## Project Overview

This project is a full-featured e-commerce API that implements a **layered architecture (Onion Architecture)** pattern, separating concerns into distinct layers for better maintainability, testability, and scalability.

### Key Capabilities:
- **Product Management**: Browse, search, and filter products with pagination
- **User Authentication**: JWT-based authentication with ASP.NET Core Identity
- **Shopping Basket**: Redis-based basket management for storing user cart items
- **Order Management**: Complete order processing workflow
- **Payment Integration**: Stripe payment processing
- **User Profile Management**: User addresses and profile information

---

## Architecture

The project follows **Clean Architecture (Onion Architecture)** principles with the following layers:

### Layer Structure:

1. **Talabat.APIS** (Presentation Layer)
   - Controllers
   - DTOs (Data Transfer Objects)
   - Middleware
   - Extension Methods for service configuration
   - Error handling

2. **Talabat.Core** (Domain Layer)
   - Business models and entities
   - Repository interfaces
   - Service interfaces
   - Specifications pattern for querying

3. **Talabat.Repository** (Infrastructure Layer)
   - Entity Framework Core DbContext
   - Repository implementations
   - Database migrations
   - Data seeding

4. **Talabat.Services** (Application Services Layer)
   - Business logic implementations
   - Token services
   - Payment services
   - Order services

---

## Technology Stack

### Core Technologies:
- **.NET 6.0** - Framework version
- **ASP.NET Core Web API** - API framework
- **Entity Framework Core 6.0.33** - ORM for database operations
- **SQL Server** - Primary database
- **Redis** - In-memory cache for basket storage
- **ASP.NET Core Identity** - Authentication and user management

### Key Packages:
- **AutoMapper 12.0.1** - Object-to-object mapping
- **JWT Bearer Authentication** - Token-based authentication
- **Stripe.net 46.2.0** - Payment processing
- **StackExchange.Redis 2.8.16** - Redis client
- **Swashbuckle.AspNetCore 6.5.0** - Swagger/OpenAPI documentation

---

## Project Structure

```
Talabat-E-Commerce/
│
├── Talabat.APIS/                    # Presentation Layer
│   ├── Controllers/                 # API Controllers
│   │   ├── AccountController.cs     # User authentication & profile
│   │   ├── BasketController.cs      # Shopping basket operations
│   │   ├── OrderController.cs       # Order management
│   │   ├── PaymentsController.cs    # Payment processing
│   │   ├── ProductsController.cs    # Product catalog
│   │   ├── ApiBaseController.cs     # Base controller with common functionality
│   │   ├── BuggyController.cs      # Error testing endpoints
│   │   └── ErrorsController.cs     # Error handling
│   │
│   ├── DTOs/                        # Data Transfer Objects
│   │   ├── AddressDto.cs
│   │   ├── BasketItemDto.cs
│   │   ├── CustomerBasketDto.cs
│   │   ├── LoginDto.cs
│   │   ├── OrderDto.cs
│   │   ├── OrderItemDto.cs
│   │   ├── OrderToReturnDto.cs
│   │   ├── ProductDto.cs
│   │   ├── RegisterDto.cs
│   │   └── UserDto.cs
│   │
│   ├── Errors/                      # Error response models
│   │   ├── ErrorResponse.cs
│   │   ├── ExceptionResponse.cs
│   │   └── ValidationErrorResponse.cs
│   │
│   ├── Extension Methods/           # Service configuration extensions
│   │   ├── ApplicationServicesExtension.cs
│   │   ├── ConfigureDatabasesExtension.cs
│   │   ├── ConfigureRedisExtension.cs
│   │   ├── IdentityServicesExtension.cs
│   │   ├── SwaggerExtension.cs
│   │   └── UserMangerExtensions.cs
│   │
│   ├── Helpers/                     # Helper classes
│   │   ├── OrderPictureUrlResolver.cs
│   │   ├── PaginationResponse.cs
│   │   └── ProductPictureUrlResolver.cs
│   │
│   ├── Middlewares/                 # Custom middleware
│   │   └── ExceptionMiddleware.cs  # Global exception handling
│   │
│   ├── Profiles/                    # AutoMapper profiles
│   │   └── MappingProfiles.cs
│   │
│   ├── Program.cs                   # Application entry point
│   ├── appsettings.json             # Configuration file
│   └── wwwroot/                     # Static files (product images)
│
├── Talabat.Core/                    # Domain Layer
│   ├── Models/                      # Domain models
│   │   ├── Address.cs
│   │   ├── AppUser.cs
│   │   ├── BaseEntity.cs
│   │   ├── BasketItem.cs
│   │   ├── CustomerBasket.cs
│   │   ├── Product.cs
│   │   ├── ProductBrand.cs
│   │   └── ProductType.cs
│   │
│   ├── Order Aggregate/             # Order aggregate root
│   │   ├── Address.cs
│   │   ├── DeliveryMethod.cs
│   │   ├── Order.cs
│   │   ├── OrderItem.cs
│   │   ├── OrderStatus.cs
│   │   └── ProductItemOrdered.cs
│   │
│   ├── Repositories/                # Repository interfaces
│   │   ├── IBasketRepository.cs
│   │   ├── IGenericRepository.cs
│   │   └── IUnitOfWork.cs
│   │
│   ├── Services/                    # Service interfaces
│   │   ├── IOrderService.cs
│   │   ├── IPaymentService.cs
│   │   └── ITokenServices.cs
│   │
│   └── Specifications/              # Specification pattern
│       ├── BaseSpecification.cs
│       ├── ISpecification.cs
│       ├── Order Specifications/
│       ├── ProductBrandsSpecification.cs
│       ├── ProductSpecificationParams.cs
│       ├── ProductTypesSpecification.cs
│       └── ProductWithBrandAndTypeSpecification.cs
│
├── Talabat.Repository/              # Infrastructure Layer
│   ├── Data/                        # Database context & configurations
│   │   ├── API1DbContext.cs        # Main database context
│   │   ├── Configurations/         # Entity configurations (Fluent API)
│   │   ├── DataSeed/               # Seed data files
│   │   └── Migrations/             # EF Core migrations
│   │
│   ├── Identity/                    # Identity database
│   │   ├── ApplicationIdentityDbContext.cs
│   │   └── Migrations/
│   │
│   ├── BasketRepository.cs         # Redis basket implementation
│   ├── GenericRepository.cs        # Generic repository pattern
│   ├── SpecificationEvaluator.cs   # Specification query builder
│   └── UnitOfWork.cs               # Unit of Work pattern
│
└── Talabat.Services/                # Application Services
    ├── OrderService.cs              # Order business logic
    ├── PaymentService.cs            # Payment processing
    └── TokenServices.cs             # JWT token generation
```

---

## Features

### 1. Product Management
- **Get Products**: Retrieve paginated product list with filtering and sorting
- **Get Product by ID**: Fetch detailed product information
- **Get Product Types**: Retrieve available product categories
- **Get Product Brands**: Retrieve available product brands
- **Product Specifications**: Uses specification pattern for complex queries
- **Pagination**: Built-in pagination support for large datasets
- **Image Handling**: Product images served from wwwroot

### 2. User Authentication & Authorization
- **User Registration**: Create new user accounts with validation
- **User Login**: JWT token-based authentication
- **Get Current User**: Retrieve authenticated user information
- **Email Validation**: Check if email already exists
- **JWT Token Service**: Secure token generation and validation
- **Role-Based Access**: ASP.NET Core Identity integration

### 3. Shopping Basket
- **Get Basket**: Retrieve user's shopping basket from Redis
- **Update/Create Basket**: Add or update items in basket
- **Delete Basket**: Remove basket from Redis
- **Redis Integration**: Fast in-memory storage for basket data
- **Basket Persistence**: Basket stored with unique ID

### 4. Order Management
- **Create Order**: Place new orders with validation
- **Get User Orders**: Retrieve all orders for authenticated user
- **Get Order by ID**: Fetch specific order details
- **Get Delivery Methods**: Available delivery options
- **Order Status Tracking**: Track order status (Pending, Processing, etc.)
- **Order Aggregation**: Uses DDD aggregate pattern for order management

### 5. Payment Processing
- **Stripe Integration**: Payment intent creation and management
- **Payment Intent Creation**: Create payment intents for orders
- **Secure Payment Processing**: Stripe API integration
- **Payment Status Updates**: Track payment status

### 6. User Profile Management
- **Get User Address**: Retrieve user's saved address
- **Update User Address**: Modify user address information
- **User Profile**: Extended user information with display name

### 7. Error Handling
- **Global Exception Middleware**: Centralized error handling
- **Validation Errors**: Detailed validation error responses
- **Error Response Models**: Standardized error response format
- **HTTP Status Codes**: Proper status code handling

### 8. Database Management
- **Entity Framework Core**: Code-first database approach
- **Database Migrations**: Version-controlled database schema
- **Data Seeding**: Initial data population
- **Two Database Contexts**: Separate contexts for main and identity data
- **Fluent API Configuration**: Entity relationship configuration

---

## Getting Started

### Prerequisites

- **.NET 6.0 SDK** or later
- **SQL Server** (LocalDB or SQL Server Express)
- **Redis Server** (for basket functionality)
- **Visual Studio 2022** or **Visual Studio Code** (recommended)

### Installation Steps

1. **Clone the Repository**
   ```bash
   git clone <repository-url>
   cd Talabat-E-Commerce
   ```

2. **Update Connection Strings**

   Edit `Talabat.APIS/appsettings.json` and update the connection strings:
   ```json
   {
     "ConnectionStrings": {
       "DefaultConnection": "Server=YOUR_SERVER;Database=Talabat;Trusted_Connection=True;",
       "RedisConnection": "localhost",
       "IdentityConnection": "Server=YOUR_SERVER;Database=TalabatIdentity;Trusted_Connection=True;"
     }
   }
   ```

3. **Install Redis**

   - Windows: Download from [Redis for Windows](https://github.com/microsoftarchive/redis/releases)
   - Or use Docker: `docker run -d -p 6379:6379 redis`

4. **Restore NuGet Packages**
   ```bash
   dotnet restore
   ```

5. **Run Database Migrations**

   The migrations are automatically applied when the application starts (see `Program.cs`). Alternatively, you can run:
   ```bash
   cd Talabat.Repository
   dotnet ef database update --project ../Talabat.Repository --startup-project ../Talabat.APIS
   ```

6. **Run the Application**
   ```bash
   cd Talabat.APIS
   dotnet run
   ```

7. **Access Swagger UI**

   Navigate to: `https://localhost:7078/swagger` (or the port configured in `launchSettings.json`)

---

## Configuration

### appsettings.json Structure

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "SQL Server connection for main database",
    "RedisConnection": "Redis server connection (default: localhost)",
    "IdentityConnection": "SQL Server connection for Identity database"
  },
  "JWT": {
    "Key": "Your secret key (32+ characters)",
    "Issuer": "Token issuer URL",
    "Audience": "Token audience",
    "ExpiresInDays": "Token expiration period"
  },
  "StripeSettings": {
    "PublishableKey": "Stripe publishable key",
    "SecretKey": "Stripe secret key"
  },
  "ApiBaseUrl": "Base URL of your API",
  "ClientBaseUrl": "Frontend application URL (for CORS)"
}
```

### Important Configuration Notes:

- **JWT Key**: Must be at least 256 bits (32 characters) with a mix of characters
- **CORS Policy**: Configured to allow requests from `ClientBaseUrl`
- **Database Connections**: Two separate databases for separation of concerns
- **Stripe Keys**: Use test keys for development, production keys for live

---

## API Endpoints

### Products

#### Get All Products
```
GET /api/Product
Query Parameters:
  - PageNumber (int)
  - PageSize (int)
  - Sort (string)
  - ProductTypeId (int)
  - ProductBrandId (int)
  - Search (string)
```
**Response**: Paginated list of products with metadata

#### Get Product by ID
```
GET /api/Product/{id}
```
**Response**: Single product with details

#### Get Product Types
```
GET /api/Product/Types?sort={sort}
```
**Response**: List of product types

#### Get Product Brands
```
GET /api/Product/Brands?sort={sort}
```
**Response**: List of product brands

---

### Account (Authentication)

#### Register User
```
POST /api/Account/Register
Body:
{
  "displayName": "string",
  "email": "string",
  "password": "string",
  "phoneNumber": "string"
}
```
**Response**: User object with JWT token

#### Login
```
POST /api/Account/Login
Body:
{
  "email": "string",
  "password": "string"
}
```
**Response**: User object with JWT token

#### Get Current User
```
GET /api/Account/CurrentUser
Headers: Authorization: Bearer {token}
```
**Response**: Current authenticated user

#### Get User Address
```
GET /api/Account/CurrentUserAddress
Headers: Authorization: Bearer {token}
```
**Response**: User's saved address

#### Update User Address
```
PUT /api/Account/Address
Headers: Authorization: Bearer {token}
Body:
{
  "firstName": "string",
  "lastName": "string",
  "street": "string",
  "city": "string",
  "state": "string",
  "zipCode": "string",
  "country": "string"
}
```

#### Check Email Exists
```
GET /api/Account/EmailExists?email={email}
```
**Response**: Boolean indicating if email exists

---

### Basket

#### Get Basket
```
GET /api/Basket?basketId={basketId}
```
**Response**: Customer basket (creates new if doesn't exist)

#### Update/Create Basket
```
POST /api/Basket
Body:
{
  "id": "string",
  "items": [
    {
      "id": "int",
      "productName": "string",
      "price": "decimal",
      "quantity": "int",
      "pictureUrl": "string",
      "brand": "string",
      "type": "string"
    }
  ]
}
```
**Response**: Updated/created basket

#### Delete Basket
```
DELETE /api/Basket?basketId={basketId}
```
**Response**: Boolean indicating success

---

### Orders

#### Create Order
```
POST /api/Order
Headers: Authorization: Bearer {token}
Body:
{
  "basketId": "string",
  "deliveryMethodId": "int",
  "shippingAddress": {
    "firstName": "string",
    "lastName": "string",
    "street": "string",
    "city": "string",
    "state": "string",
    "zipCode": "string",
    "country": "string"
  }
}
```
**Response**: Created order

#### Get User Orders
```
GET /api/Order
Headers: Authorization: Bearer {token}
```
**Response**: List of user's orders

#### Get Order by ID
```
GET /api/Order/{id}
Headers: Authorization: Bearer {token}
```
**Response**: Order details

#### Get Delivery Methods
```
GET /api/Order/DeliveryMethods
Headers: Authorization: Bearer {token}
```
**Response**: Available delivery methods

---

### Payments

#### Create/Update Payment Intent
```
POST /api/Payment/{basketId}
Headers: Authorization: Bearer {token}
```
**Response**: Basket with payment intent

---

## Authentication

The API uses **JWT (JSON Web Tokens)** for authentication.

### How It Works:

1. **Registration/Login**: User provides credentials
2. **Token Generation**: Server generates JWT token using `ITokenServices`
3. **Token Response**: Token returned to client
4. **Subsequent Requests**: Client includes token in `Authorization` header:
   ```
   Authorization: Bearer {your_token_here}
   ```
5. **Token Validation**: Server validates token on each protected endpoint

### Token Structure:

- **Algorithm**: HS256 (HMAC SHA-256)
- **Issuer**: Configured in `appsettings.json`
- **Audience**: Configured in `appsettings.json`
- **Expiration**: Configurable (default: 2 days)
- **Claims**: Email, DisplayName, and other user information

### Protected Endpoints:

Most endpoints require authentication except:
- Product listing
- User registration
- User login
- Email existence check

---

## Database Setup

### Database Contexts

The project uses **two separate database contexts**:

1. **API1DbContext**: Main application database
   - Products
   - ProductBrands
   - ProductTypes
   - Orders
   - OrderItems
   - DeliveryMethods

2. **ApplicationIdentityDbContext**: Identity database
   - Users (AppUser)
   - Roles
   - UserClaims
   - UserAddresses

### Migrations

Migrations are automatically applied on application startup. To create new migrations:

```bash
# For main database
dotnet ef migrations add MigrationName --project Talabat.Repository --startup-project Talabat.APIS --context API1DbContext

# For identity database
dotnet ef migrations add MigrationName --project Talabat.Repository --startup-project Talabat.APIS --context ApplicationIdentityDbContext
```

### Data Seeding

Initial data is seeded automatically:
- **Products**: From `DataSeed/Products.json`
- **Brands**: From `DataSeed/Brands.json`
- **Types**: From `DataSeed/Types.json`
- **Delivery Methods**: From `DataSeed/Delivery.json`
- **Default Users**: Seeded via `IdentityDataSeedInitializer`

---

## Services & Integrations

### 1. Redis Service (Basket)

- **Purpose**: Store shopping basket data in memory
- **Implementation**: `BasketRepository` using StackExchange.Redis
- **Benefits**: Fast access, automatic expiration, scalable

### 2. Stripe Payment Service

- **Purpose**: Process payments securely
- **Implementation**: `PaymentService` using Stripe.net SDK
- **Features**:
  - Payment intent creation
  - Payment status tracking
  - Webhook support (commented out in code)

### 3. Token Service

- **Purpose**: Generate and validate JWT tokens
- **Implementation**: `TokenServices` using JWT Bearer
- **Features**: Secure token generation with user claims

### 4. Order Service

- **Purpose**: Handle order business logic
- **Implementation**: `OrderService`
- **Features**:
  - Order creation
  - Order validation
  - Order retrieval
  - Basket to order conversion

### 5. Unit of Work Pattern

- **Purpose**: Manage database transactions and repositories
- **Implementation**: `UnitOfWork` class
- **Benefits**: Transaction management, repository coordination

---

## Error Handling

### Error Response Types:

1. **ErrorResponse**: Standard error response
   ```json
   {
     "statusCode": 404,
     "message": "Not Found"
   }
   ```

2. **ValidationErrorResponse**: Validation errors
   ```json
   {
     "statusCode": 400,
     "message": "Validation Errors",
     "errors": ["Error1", "Error2"]
   }
   ```

3. **ExceptionResponse**: Server exceptions
   ```json
   {
     "statusCode": 500,
     "message": "Internal Server Error",
     "details": "Exception details (development only)"
   }
   ```

### Error Middleware:

`ExceptionMiddleware` catches all unhandled exceptions and returns appropriate responses based on environment (detailed in development, generic in production).

### HTTP Status Codes:

- **200 OK**: Successful request
- **400 Bad Request**: Validation errors or bad input
- **401 Unauthorized**: Missing or invalid authentication
- **404 Not Found**: Resource not found
- **500 Internal Server Error**: Server error

---

## Testing

### Using Swagger UI

1. Navigate to `/swagger` endpoint
2. Use "Authorize" button to set JWT token
3. Test endpoints interactively

### Postman Collection

Create a Postman collection with:
1. Register/Login requests to get token
2. Set token as Bearer token in collection variables
3. Test all endpoints

### Error Testing

`BuggyController` provides endpoints to test error handling:
- Not Found errors
- Server errors
- Bad Request errors
- Unauthorized errors

---

## Development Best Practices

### Code Patterns Used:

1. **Repository Pattern**: Abstraction for data access
2. **Unit of Work Pattern**: Transaction management
3. **Specification Pattern**: Complex query encapsulation
4. **DTO Pattern**: Data transfer objects for API responses
5. **Dependency Injection**: Constructor injection throughout
6. **Extension Methods**: Clean service configuration
7. **AutoMapper**: Object mapping between layers
8. **Fluent API**: Entity configuration

### Architecture Benefits:

- **Separation of Concerns**: Each layer has distinct responsibilities
- **Testability**: Interfaces allow easy mocking
- **Maintainability**: Changes isolated to specific layers
- **Scalability**: Easy to extend and modify
- **Reusability**: Generic repository and specifications

---

## Support

For issues, questions, or contributions:

1. Check the Swagger documentation at `/swagger`
2. Review the code comments for detailed explanations
3. Check error responses for troubleshooting information
4. Verify database and Redis connections
5. Ensure all configuration values are set correctly

---

## License

This project is for educational purposes. Ensure you configure your own:
- Database connection strings
- JWT secrets
- Stripe API keys
- Redis connection

---

## Future Enhancements

Potential improvements:
- Email confirmation for user registration
- Password reset functionality
- Order status webhooks
- Product reviews and ratings
- Admin dashboard endpoints
- Advanced search and filtering
- Caching strategies
- Rate limiting
- API versioning
- Comprehensive unit and integration tests
