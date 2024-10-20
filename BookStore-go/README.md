

# Go Bookstore Application

## Description

The Go Bookstore application is a simple RESTful API built using Go that connects to a MySQL database. This application allows users to create, retrieve, update, and delete book records. The API follows best practices for structuring Go applications, making it easy to maintain and extend.

## Features

- Create, Read, Update, and Delete (CRUD) operations for books
- MySQL database connection
- Organized package structure for better code management
- Error handling and validation

## Technologies Used

- Go (Golang)
- MySQL
- Gorilla Mux for routing
- JSON for data exchange

## File Structure

```plaintext
go-bookstore/
│
├── cmd/
│   └── main.go               # Entry point of the application
│
├── pkg/
│   ├── config/               # Configuration for database and application settings
│   │   └── app.go            # Database connection settings
│   │
│   ├── controllers/          # Handlers for HTTP requests
│   │   ├── book_controller.go # CRUD operations for books
│   │   
│   │
│   ├── models/               # Data models representing the database structure
│   │   └── book.go           # Book model definition
│   │
│   ├── routes/               # HTTP routes for the application
│   │   └── book_routes.go     # Route definitions for book-related endpoints
│   │
│   ├── utils/                # Utility functions and helpers
│   │   └── utils.go        # Common helper functions
│
├── go.mod                    # Go module file
├── go.sum                    # Go module checksum file
├── .env                      # Environment variables
└── README.md                 # Project documentation
```

## Installation

### Prerequisites

- Go installed on your machine (version 1.16+)
- MySQL server running
- A MySQL client to manage your database (optional)

### Steps to Set Up

1. **Clone the repository:**

   ```bash
   git clone https://github.com/yourusername/go-bookstore.git
   cd go-bookstore
   ```

2. **Set up your MySQL database:**

   - Create a new database (e.g., `simpleresr`).
   - Update the database connection details in the `.env` file.

   Example `.env` file:
   ```plaintext
   DB_USER=root
   DB_PASSWORD=yourpassword
   DB_NAME=simpleresr
   DB_HOST=127.0.0.1
   DB_PORT=3306
   ```

3. **Install dependencies:**

   Ensure you are in the project directory, then run:

   ```bash
   go mod tidy
   ```

4. **Run the application:**

   ```bash
   go run cmd/main.go
   ```

   The application should now be running on `localhost:9010`.

## Usage

### API Endpoints

- **Create a book**
  - **POST** `/book/`
  - Request body:
    ```json
    {
      "name": "Book Name",
      "author": "Author Name",
      "publication": "Publisher Name"
    }
    ```

- **Get all books**
  - **GET** `/book/`

- **Get a book by ID**
  - **GET** `/book/{bookID}`

- **Update a book**
  - **PUT** `/book/{bookID}`
  - Request body:
    ```json
    {
      "name": "Updated Book Name",
      "author": "Updated Author Name",
      "publication": "Updated Publisher Name"
    }
    ```

- **Delete a book**
  - **DELETE** `/book/{bookID}`

### Example CURL Requests

- Create a book:
  ```bash
  curl -X POST http://localhost:9010/book/ -H "Content-Type: application/json" -d '{"name": "Book Name", "author": "Author Name", "publication": "Publisher Name"}'
  ```

- Get all books:
  ```bash
  curl http://localhost:9010/book/
  ```

## Contributing

Contributions are welcome! Please feel free to submit a pull request or open an issue for any suggestions or improvements.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Author

Madhur Banger  
Email: madhur.cloudevops@gmail.com  
GitHub: [madhur-banger](https://github.com/madhur-banger)

```
