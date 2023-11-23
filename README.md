# Ipsator_E-Commerce-Application

- The E-Commerce Application is built using Java and Spring Boot, with security, scalability, and ease of maintenance. The backend uses Spring Data JPA to interact with a MySQL database, making it easy to manage and store important entities such as users, products, categories, orders, and more. User authentication is handled by Auth0, providing secure and reliable means of REST APIs.

- The APIs are well-documented and easily accessible through Swagger UI, making it simple for developers to test and understand the various endpoints. Overall, this project provides secure Rest APIs to create a scalable platform for businesses to sell their products to customers.
  

# Installation & Run

Before running the API server, update the database configuration inside the [application.properties](https://github.com/Avanishipsator/Ipsator_Ecommerce/edit/main/resources/application.properties) file. You can set the port number, username, and password as per your local database configuration.

```
    server.port=8888
    spring.datasource.url=jdbc:mysql://localhost:3306/masaidb;
    spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
    spring.datasource.username=root
    spring.datasource.password=root
```

# Features
## Admin:
- **Login**: Admins can log in to access administrative functions.
- **Users**: Admins can manage user accounts and their roles.
- **Address**: Admins can manage user addresses, including creation and updates.
- **Categories**: Admins can create, update, and disable product categories.
- **Products**: Admins can add, update, and disable products in different categories.
- **Price & Discount**: Admins can set product prices and discounts.
- **Orders**: Admins can view and manage all customer orders.

## Shopowner:
- **Login**: Shopowners can log in to manage their shops.
- **Categories**: Shopowners can create and manage product categories for their shops.
- **Products**: Shopowners can add, update, and disable products in their categories.
- **Price & Discount**: Shopowners can set prices and discounts for their products.
- **Orders**: Shopowners can view and manage orders specific to their shop.

## User:
- **Registration & Login**: Users can register and log in to access e-commerce features.
- **Fetch Categories and Products**: Users can view product categories and browse products based on categories.
- **Cart**: Users can add and delete products to/from their cart.
- **Manage Address**: Users can manage their delivery addresses.
- **Manage Product Quantity**: Users can adjust product quantities in their cart.
- **Order Products**: Users can place orders and select payment methods.
- **View Order Status**: Users can check the status of their orders.

# Entity Classes

This project includes several entity classes that define the data structure of the application. These entities are used to represent and store data in the underlying database. Below are the entity classes and their descriptions:

## Role

- **Entity Name:** Role
- **Table Name:** roles
- **Attributes:**
  - `roleId`: Role ID
  - `roleName`: Role name


## User

- **Entity Name:** User
- **Table Name:** users
- **Attributes:**
  - `userId` (Auto-generated)
  - `firstName`: First name (Size: 5-20, Pattern: Alphabets only)
  - `lastName`: Last name (Size: 5-20, Pattern: Alphabets only)
  - `mobileNumber`: Mobile number (Size: 10, Pattern: Numeric)
  - `email`: Email (Unique, Not Null)
  - `password`: Password
  - `roles`: Set of roles (Many-to-Many relationship with `Role` entity)
  - `addresses`: List of addresses (Many-to-Many relationship with `Address` entity, mappedBy `users`)
  - `cart`: Cart associated with the user (One-to-One relationship, mapped by `user` in `Cart` class)


## Address

- **Entity Name:** Address
- **Table Name:** addresses
- **Attributes:**
  - `addressId` (Auto-generated)
  - `street`: Street name (NotBlank, min 5 characters)
  - `buildingName`: Building name (NotBlank, min 5 characters)
  - `city`: City name (NotBlank, min 4 characters)
  - `state`: State name (NotBlank, min 2 characters)
  - `country`: Country name (NotBlank, min 2 characters)
  - `pincode`: Pincode (NotBlank, min 6 characters)
- **Relationships:**
  - Many-to-Many with `User` entity (Mapped by `addresses` in `User` class)


## Cart

- **Entity Name:** Cart
- **Table Name:** carts
- **Attributes:**
  - `cartId` (Auto-generated)
  - `user`: User associated with the cart (One-to-One relationship)
  - `cartItems`: List of cart items (One-to-Many relationship with `CartItem` entity)
  - `totalPrice`: Total price of items in the cart (Double)

## CartItem

- **Entity Name:** CartItem
- **Table Name:** cart_items
- **Attributes:**
  - `cartItemId` (Auto-generated)
  - `cart`: Cart to which the item belongs (Many-to-One relationship)
  - `product`: Product associated with the item (Many-to-One relationship)
  - `quantity`: Quantity of the product
  - `discount`: Discount applied to the product
  - `productPrice`: Price of the product


## Wishlist

- **Entity Name:** Wishlist
- **Table Name:** wishlist
- **Attributes:**
  - `wishlistId` (Auto-generated)
  - `name`: Name of the wishlist (Default: "Default")
- **Relationships:**
  - Many-to-One with `User` entity (Mapped by `user` in `Wishlist` class)
  - Many-to-Many with `Product` entity (JoinTable: `wishlist_product`)

## Category

- **Entity Name:** Category
- **Table Name:** categories
- **Attributes:**
  - `categoryId` (Auto-generated)
  - `categoryName`: Category name (NotBlank, min 5 characters)
  - `enabled`: Indicates if the category is enabled
- **Relationships:**
  - One-to-Many with `Product` entity (Mapped by `category` in `Product` class)


## Product

- **Entity Name:** Product
- **Table Name:** products
- **Attributes:**
  - `productId` (Auto-generated)
  - `productName`: Name of the product (NotBlank, min 3 characters)
  - `image`: Product image
  - `description`: Product description (NotBlank, min 6 characters)
  - `quantity`: Quantity of the product
  - `price`: Price of the product
  - `discount`: Discount applied to the product
  - `specialPrice`: Special price of the product
  - `category`: Category to which the product belongs (Many-to-One relationship, mapped by `category` in `Category` class)
  - `enabled`: Indicates if the product is enabled
- **Relationships:**
  - One-to-Many with `CartItem` entity (Mapped by `product` in `CartItem` class)
  - One-to-Many with `OrderItem` entity (Mapped by `product` in `OrderItem` class)


## Order

- **Entity Name:** Order
- **Table Name:** orders
- **Attributes:**
  - `orderId` (Auto-generated)
  - `email`: Email associated with the order (Email, Not Null)
  - `orderItems`: List of order items (One-to-Many relationship with `OrderItem` entity, mappedBy `order`)
  - `orderDate`: Date when the order was placed
  - `paymentMethod`: Payment method for the order (Enum: `PaymentMethod`)
  - `payment`: Payment details (One-to-One relationship with `Payment` entity, mapped by `order`)
  - `totalAmount`: Total amount of the order

## OrderItem

- **Entity Name:** OrderItem
- **Table Name:** order_items
- **Attributes:**
  - `orderItemId` (Auto-generated)
  - `product`: Product associated with the order item (Many-to-One relationship, mapped by `products` in `Product` class)
  - `order`: Order to which the item belongs (Many-to-One relationship, mapped by `order` in `Order` class)
  - `quantity`: Quantity of the product
  - `discount`: Discount applied to the product
  - `orderedProductPrice`: Price of the ordered product
  - `orderStatus`: Status of the order (Enum: `DeliveryStatus`)

## Payment

- **Entity Name:** Payment
- **Table Name:** payments
- **Attributes:**
  - `paymentId` (Auto-generated)
  - `order`: Order associated with the payment (One-to-One relationship, mapped by `payment` in `Order` class)
  - `paymentMethod`: Payment method (Enum: `PaymentMethod`)


## Feedback

- **Entity Name:** Feedback
- **Table Name:** feedback
- **Attributes:**
  - `feedbackId` (Auto-generated)
  - `orderId`: Identifier for the associated order
  - `orderItemId`: Identifier for the associated order item
  - `feedbackMessage`: User's feedback message


## Refund

- **Entity Name:** Refund
- **Table Name:** refund
- **Attributes:**
  - `refundId` (Auto-generated)
  - `orderId`: Identifier for the associated order
  - `orderItemId`: Identifier for the associated order item
  - `refundAmount`: Amount to be refunded
  - `reason`: Reason for the refund
- **Relationships:**
  - One-to-Many with `RefundHistory` entity (Mapped by `refund` in `RefundHistory` class)

## RefundHistory

- **Entity Name:** RefundHistory
- **Table Name:** refund_history
- **Attributes:**
  - `refundHistoryId` (Auto-generated)
  - `refund`: Refund associated with the history (Many-to-One relationship, mapped by `refund` in `Refund` class)
  - `refundstatus`: Status of the refund (Enum: `RefundStatus`)
  - `refundDate`: Date of the refund



## Enums

This project includes several enums that define specific sets of constant values used in the application.

### DeliveryStatus

- Enum Name: DeliveryStatus
- Values:
  - `PACKED`
  - `READY_FOR_DELIVERY`
  - `DELIVERED`
  - `CANCEL`

### PaymentMethod

- Enum Name: PaymentMethod
- Values:
  - `CREDIT_CARD`
  - `DEBIT_CARD`
  - `UPI`
  - `NET_BANKING`
  - `CASH_ON_DELIVERY`

### RefundStatus

- Enum Name: RefundStatus
- Values:
  - `PENDING`
  - `APPROVED`
  - `REJECTED`

These entity classes define the data structure of the application and are used for database interactions.


# Security
- The API is secured using JSON Web Tokens (JWT) handled by Auth0. To access the API, you will need to obtain a JWT by authenticating with the /login endpoint. The JWT should then be passed in the Authorize option available in the Swagger-ui.

  ### Example:
  - Authorization: <your_jwt>

# Technologies:
- Java 17 or above
- Spring Boot 3.0
- Maven
- MySQL
- Spring Data JPA
- Spring Security
- JSON Web Tokens (JWT)
- Auth0
- Swagger UI

# Running the app
1. Clone the repository: git clone https://github.com/Avanishipsator/Ipsator_Ecommerce.git
2. Import the project into STS:
  - Click File > Import...
  - Select Maven > Existing Maven Projects and click Next
  - Browse to the project directory and click Finish
3. Update the values in application.properties with your MySQL database connection details.
4. Run the app: Right-click the project in the Package Explorer and click Run As > Spring Boot App.

# API documentation
- API documentation is available via Swagger UI at http://localhost:8888/swagger-ui/index.html


# ER-Diagram


![document](https://github.com/Avanishipsator/Ipsator_Ecommerce/assets/144343086/17d9b160-c518-4da2-aa36-4501415038a5)

# Swagger-ui

![Swagger-UI](https://github.com/Avanishipsator/Ipsator_Ecommerce/assets/144343086/eec79179-039c-4d65-8f13-ab2ed4516c13)


# API Controllers
![2](https://github.com/Avanishipsator/Ipsator_Ecommerce/assets/144343086/7dbb58b3-ccf2-448a-9035-2112510766f7)
![3](https://github.com/Avanishipsator/Ipsator_Ecommerce/assets/144343086/e1c4fbc8-dbb5-41b8-842f-8d940f9425b6)
![4](https://github.com/Avanishipsator/Ipsator_Ecommerce/assets/144343086/0d05ca40-431b-458b-96e7-4ba1c799c2ce)

![5](https://github.com/Avanishipsator/Ipsator_Ecommerce/assets/144343086/e98d43ef-a9aa-4768-ab66-a1854f2537a6)
![6](https://github.com/Avanishipsator/Ipsator_Ecommerce/assets/144343086/398e3a1f-957d-4a86-b9e8-be03ff517bb0)
![7](https://github.com/Avanishipsator/Ipsator_Ecommerce/assets/144343086/c814e14d-f501-4b56-8147-186d4a9a0ca6)
![8](https://github.com/Avanishipsator/Ipsator_Ecommerce/assets/144343086/0e4357f4-ab00-4c24-9c07-5b9fb4a9e5d3)
![9](https://github.com/Avanishipsator/Ipsator_Ecommerce/assets/144343086/5e16efea-ca32-4cc1-9dbc-fbf7e1315568)
![10](https://github.com/Avanishipsator/Ipsator_Ecommerce/assets/144343086/f713d743-3fc3-4d87-86ac-debe350f7aeb)


## Author

- [Avanish Mani Tripathi](https://github.com/avanishmani)


# Thank you for choosing the Ipsator E-Commerce Application for your business. Happy selling!
