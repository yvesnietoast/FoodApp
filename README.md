# FoodApp
personal project I thought I would share
# 🍽️ FoodApp API Documentation

Welcome to the FoodApp API! This Flask-based application allows you to interact with a food database, including user authentication, food item searches, and more.

## 📋 Table of Contents
- [🚀 Getting Started](#-getting-started)
- [🔑 Authentication](#-authentication)
  - [1. Register a New User](#1-register-a-new-user)
  - [2. Login](#2-login)
- [🥗 Food Data Operations](#-food-data-operations)
  - [3. Search for Food Items by Name](#3-search-for-food-items-by-name)
  - [4. Get Food Item by ID](#4-get-food-item-by-id)
  - [5. Get All Food Groups](#5-get-all-food-groups)
  - [6. Get Food Items by Food Group](#6-get-food-items-by-food-group)
  - [7. Get Food Item with Most/Least Calories](#7-get-food-item-with-mostleast-calories)
- [🧪 Test Endpoints](#-test-endpoints)
  - [8. Get User Info](#8-get-user-info)
  - [9. Create User (Test)](#9-create-user-test)
- [🔧 Building and Running](#-building-and-running)
  - [Building the Docker Container](#building-the-docker-container)
  - [Launching the Postgres Server](#launching-the-postgres-server)
  - [Linux Port Forward](#linux-port-forward)
- [ℹ️ Notes](#-notes)

## 🚀 Getting Started

Ensure you have the necessary dependencies installed and that your database is correctly configured. You will also need to set environment variables for your database connection and JWT secret key.

## 🔑 Authentication

These endpoints handle user registration and login.

### 1. Register a New User
- **URL:** `/register`
- **Method:** `POST`
- **Description:** Registers a new user by saving the username, email, and password to the database.
- **Request Body:**
  ```json
  {
    "username": "string",
    "password": "string",
    "email": "string"
  }
  ```
- **Response:**
  - **Success:** Returns the user information.
  - **Error:** Returns a message if the username or email is already in use.
  ```json
  {
    "msg": "Username or email already in use"
  }
  ```

### 2. Login
- **URL:** `/login`
- **Method:** `POST`
- **Description:** Authenticates a user and returns a JWT access token and refresh token.
- **Request Body:**
  ```json
  {
    "username": "string",
    "password": "string"
  }
  ```
- **Response:**
  - **Success:** Returns the `access_token` and `refresh_token`.
  - **Error:** Returns a message if the username or password is incorrect.
  ```json
  {
    "msg": "Bad username or password"
  }
  ```

## 🥗 Food Data Operations

These endpoints manage the food data stored in the database. Most of these endpoints require JWT authentication.

### 3. Search for Food Items by Name
- **URL:** `/FoodApp/search-term/<search_term>`
- **Method:** `GET`
- **Authentication:** JWT required
- **Description:** Searches for food items by name, returning up to 5 matching items.
- **Response:** A list of food items with their `Name` and `ID`.
  ```json
  [
    {"Name": "string", "ID": "integer"}
  ]
  ```

### 4. Get Food Item by ID
- **URL:** `/FoodApp/ID/<ID>`
- **Method:** `GET`
- **Authentication:** JWT required
- **Description:** Retrieves the full details of a food item by its ID.
- **Response:** A JSON object containing details of the food item.
  ```json
  {
    "ID": "integer",
    "Name": "string",
    "Food Group": "string",
    "Calories (g)": "integer",
    "Fat (g)": "float",
    "Protein (g)": "float",
    "Sugars(g)": "float"
  }
  ```

### 5. Get All Food Groups
- **URL:** `/FoodApp/GetFoodGroups`
- **Method:** `GET`
- **Authentication:** JWT required
- **Description:** Retrieves a list of all distinct food groups available in the database.
- **Response:** A list of food groups.
  ```json
  [
    ["string"],
    ...
  ]
  ```

### 6. Get Food Items by Food Group
- **URL:** `/FoodApp/FoodGroup/<foodGroup>`
- **Method:** `GET`
- **Authentication:** JWT required
- **Description:** Retrieves food items that belong to a specific food group.
- **Response:** A list of food items with their `Name` and `ID`.
  ```json
  [
    {"Name": "string", "ID": "integer"}
  ]
  ```

### 7. Get Food Item with Most/Least Calories
- **URL:** `/FoodApp/Cals or /FoodApp/Cals/<most_least>`
- **Method:** `GET`
- **Authentication:** JWT required
- **Description:** Retrieves the food item with the most or least calories, based on the query parameter.
- **Query Parameter:**
  - `most_least`: Optional. Values can be ASC (default, for least calories) or DESC (for most calories).
- **Response:** A JSON object containing details of the food item.
  ```json
  [
    {
      "ID": "integer",
      "Name": "string",
      "Food Group": "string",
      "Calories (g)": "integer",
      "Fat (g)": "float",
      "Protein (g)": "float",
      "Sugars(g)": "float"
    }
  ]
  ```

## 🧪 Test Endpoints

These endpoints are used for testing and do not interact with the database.

### 8. Get User Info
- **URL:** `/get-user/<user_id>`
- **Method:** `GET`
- **Description:** Retrieves test user information based on the provided `user_id`. Supports optional query parameters `name` and `email` to customize the response.
- **Query Parameters:**
  - `name`: Optional. Overrides the name field in the response.
  - `email`: Optional. Overrides the email field in the response.
- **Response:** A JSON object containing the user information.
  ```json
  {
    "user_id": "integer",
    "name": "string",
    "email": "string"
  }
  ```

### 9. Create User (Test)
- **URL:** `/create-user/<user_id>`
- **Method:** `POST`
- **Description:** Echoes back the JSON data sent in the request body.
- **Request Body:** Any JSON object.
- **Response:** The same JSON object that was sent in the request.
  ```json
  {
    "key": "value"
  }
  ```

## 🔧 Building and Running

### Building the Docker Container
- **Build the Docker Image:** `docker image build -t flask_docker .`
- **Run the Docker Container:** `docker run -p 5000:5000 -d flask_docker`

### Launching the Postgres Server
- **Launch Postgres in Docker:** `sudo docker run --name 'POSTGRESSNAME' -e POSTGRES_PASSWORD='PASSWORD' -v postgres:/var/lib/postgresql/data -p5432:5432 -d postgres`
- **Start the Postgres Container:** `docker start postgres`

### Linux Port Forward
- **Check Firewall Rules:** `sudo ufw status`
- **Allow a Port:** `ufw allow PORTNUMBER`

## ℹ️ Notes
- 🔐 **Security:** Replace the `JWT_SECRET_KEY` with a secure key for production use.
- ⚠️ **Error Handling:** The application should be configured with proper error handling and input validation to enhance security and reliability.
