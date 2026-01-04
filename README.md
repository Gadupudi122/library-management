# library-management

Library Management System API
This repository contains a robust RESTful API built with Node.js, Express, and Mongoose (MongoDB). It is designed to manage book inventories, handle stock updates, and perform conditional deletions based on availability.

📖 Table of Contents
Introduction

Architecture

Prerequisites

File Breakdown

API Documentation

Data Validation Rules

Installation & Setup

Testing the Endpoints

Future Improvements

License

🌟 Introduction
The Library Management API allows administrators to track a collection of books, monitor stock levels, and categorize literature. The system uses a NoSQL database to store documents representing books with attributes like author, title, and publishing year.

🏗️ Architecture
The project follows a modular structure to separate concerns:

Database Configuration: Centralized connection logic.

Data Modeling: Schema definitions and constraints.

Routing & Logic: Handling HTTP requests and business logic.

🚦 Prerequisites
Before you begin, ensure you have the following installed:

Node.js (v14 or higher)

MongoDB Community Server (running on localhost:27017)

npm (Node Package Manager)

📁 File Breakdown
1. package.json
The project manifest that manages metadata and external libraries.

Express (^5.2.1): Used for building the web server and API routes.

Mongoose (^9.1.1): An ODM (Object Data Modeling) library for MongoDB and Node.js.

2. db.js
The core database connection file.

Connects to mongodb://127.0.0.1:27017/libraryDB.

Handles connection success and error logging.

Exports the mongoose instance for global use.

3. bookmodel.js
Defines the structure of the data.

Schema Fields: title, author, category, publishedYear, and availableCopies.

Validation: The availableCopies field is enforced to be a minimum of 0.

4. app.js
The main application file where all logic resides.

Configures JSON middleware for parsing requests.

Implements RESTful routes for CRUD operations.

Listens on port 3000.

📡 API Documentation
1. Create Operations
POST /addBooks

Description: Inserts a batch of 7 pre-defined books into the database.

Included Titles: Clean Code, Atomic Habits, Deep Learning, and more.

2. Read Operations
GET /books

Description: Retrieves the complete list of all books in the library.

GET /books/category/:category

Description: Filters books by genre (e.g., /books/category/AI).

GET /books/year/after2015

Description: Fetches all books published strictly after 2015.

3. Update Operations
PUT /books/updateCopies/:id

Description: Updates the inventory count.

Logic: Accepts a positive or negative change value in the body. It prevents the stock from dropping below zero.

PUT /books/changeCategory/:id

Description: Updates the category of a specific book by its ID.

4. Delete Operations
DELETE /books/delete/:id

Constraint: The book will not be deleted if availableCopies is greater than 0.

Purpose: Ensures books currently in stock are not accidentally removed from the catalog.

🛠️ Data Validation Rules
The API implements several business logic checks to maintain data integrity:

Negative Stock Prevention: The update route checks book.availableCopies + change < 0 to block illegal updates.

Schema Enforcement: Mongoose ensures all fields match the defined types (String, Number).

Deletion Safety: Prevents the removal of books that are still physically available in the library.

⚙️ Installation & Setup
Clone the Repository:

Bash

git clone https://github.com/yourusername/library-api.git
cd library-api
Install Dependencies:

Bash

npm install
Configure Files: Rename your files to standard naming conventions:

app (1).js ➡️ app.js

db (1).js ➡️ db.js

bookmodel (1).js ➡️ bookmodel.js

Fix Pathing: Update the require statements in app.js and bookmodel.js to ensure they point to the correct file paths.

Run the Server:

Bash

node app.js
🧪 Testing the Endpoints
You can use tools like Postman, Insomnia, or cURL to test the API.

Example: Seed the Database

Bash

curl -X POST http://localhost:3000/addBooks
Example: Decrease Stock by 1

Bash

curl -X PUT -H "Content-Type: application/json" \
-d '{"change": -1}' \
http://localhost:3000/books/updateCopies/[YOUR_BOOK_ID]
🚀 Future Improvements
Add User Authentication using JWT (JSON Web Tokens).

Implement Search functionality for book titles and authors.

Add a Frontend interface using React or Vue.

Implement Pagination for the /books endpoint.

