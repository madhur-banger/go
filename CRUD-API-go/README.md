

# Go Movie API

This is a simple RESTful API for managing a collection of movies. The API supports basic CRUD (Create, Read, Update, Delete) operations on movie resources and is built using the Go programming language. It leverages the `gorilla/mux` router for handling HTTP routes.

## Table of Contents
1. [Overview](#overview)
2. [Features](#features)
3. [Project Structure](#project-structure)
4. [Technologies Used](#technologies-used)
5. [Installation](#installation)
6. [API Endpoints](#api-endpoints)
7. [Code Walkthrough](#code-walkthrough)
8. [Example Requests](#example-requests)
9. [Future Enhancements](#future-enhancements)

## Overview

The **Go Movie API** allows clients to interact with a collection of movies through a RESTful API. The API can handle requests to retrieve all movies, retrieve a single movie by ID, create new movies, update existing ones, and delete movies.

## Features

- **Retrieve all movies**: Fetch the entire movie collection.
- **Retrieve a movie by ID**: Fetch a specific movie by its unique ID.
- **Create a new movie**: Add a new movie to the collection.
- **Update a movie**: Modify details of an existing movie by its ID.
- **Delete a movie**: Remove a movie from the collection by its ID.
- **In-memory storage**: The movies are stored in memory and will be reset when the server restarts.

## Project Structure

```
├── main.go         # Main Go file containing all the code
├── go.mod          # Go module file that tracks dependencies
└── README.md       # Project documentation
```

## Technologies Used

- **Go**: The programming language used for building the API.
- **Gorilla Mux**: A powerful HTTP router and dispatcher for matching incoming requests to their respective handlers.
- **Standard Libraries**: Various Go standard libraries including:
  - `encoding/json`: To encode/decode data to/from JSON.
  - `net/http`: To handle HTTP requests and responses.
  - `math/rand`: To generate random IDs.
  - `log`: For logging errors and status messages.

## Installation

### Prerequisites
- **Go 1.16+** installed on your machine. You can download it from [here](https://golang.org/dl/).

### Steps

1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/go-movie-api.git
   cd go-movie-api
   ```

2. Install dependencies:
   ```bash
   go mod tidy
   ```

3. Run the server:
   ```bash
   go run main.go
   ```

4. The API will start on `http://localhost:8000`.

## API Endpoints

Here’s a list of available API endpoints:

| HTTP Method | Endpoint            | Description                  |
|-------------|---------------------|------------------------------|
| GET         | `/movies`            | Fetch all movies             |
| GET         | `/movies/{id}`       | Fetch a single movie by ID   |
| POST        | `/movies`            | Create a new movie           |
| PUT         | `/movies/{id}`       | Update an existing movie     |
| DELETE      | `/movies/{id}`       | Delete a movie by ID         |

### Detailed Endpoint Descriptions

1. **GET `/movies`**  
   Fetches all the movies from the collection.

   **Response**:  
   - 200 OK  
   - JSON array of movies:
     ```json
     [
       {
         "id": "1",
         "isbn": "43552",
         "title": "Movie One",
         "director": {
           "firstname": "John",
           "lastname": "Doe"
         }
       },
       {
         "id": "2",
         "isbn": "35543",
         "title": "Movie Two",
         "director": {
           "firstname": "Steve",
           "lastname": "Smith"
         }
       }
     ]
     ```

2. **GET `/movies/{id}`**  
   Fetches a movie by its unique ID.

   **Response**:  
   - 200 OK (if movie exists)  
   - JSON object of the requested movie:
     ```json
     {
       "id": "1",
       "isbn": "43552",
       "title": "Movie One",
       "director": {
         "firstname": "John",
         "lastname": "Doe"
       }
     }
     ```

   - 404 Not Found (if movie doesn't exist)

3. **POST `/movies`**  
   Creates a new movie. The movie data must be sent in the request body as JSON.

   **Request Body**:
   ```json
   {
     "isbn": "98657",
     "title": "New Movie",
     "director": {
       "firstname": "Jane",
       "lastname": "Doe"
     }
   }
   ```

   **Response**:  
   - 201 Created  
   - JSON object of the created movie:
     ```json
     {
       "id": "3",
       "isbn": "98657",
       "title": "New Movie",
       "director": {
         "firstname": "Jane",
         "lastname": "Doe"
       }
     }
     ```

4. **PUT `/movies/{id}`**  
   Updates an existing movie by ID. The updated movie data must be sent in the request body as JSON.

   **Request Body**:
   ```json
   {
     "isbn": "11223",
     "title": "Updated Movie",
     "director": {
       "firstname": "John",
       "lastname": "Smith"
     }
   }
   ```

   **Response**:  
   - 200 OK  
   - JSON object of the updated movie.

5. **DELETE `/movies/{id}`**  
   Deletes a movie by its ID.

   **Response**:  
   - 200 OK  
   - JSON array of the remaining movies.

## Code Walkthrough

1. **Struct Definitions**

   ```go
   type Movie struct {
       ID       string    `json:"id"`
       Isbn     string    `json:"isbn"`
       Title    string    `json:"title"`
       Director *Director `json:"director"`
   }

   type Director struct {
       Firstname string `json:"firstname"`
       Lastname  string `json:"lastname"`
   }
   ```

   The `Movie` struct represents a movie object, with fields like ID, ISBN, title, and a pointer to the `Director` struct. The `Director` struct holds the director's first and last name.

   **Note**: The backticks with `json:"..."` provide the key names for JSON encoding/decoding.

2. **Global Variables**

   ```go
   var movies []Movie
   ```

   This is a global slice that holds the list of movies in memory.

3. **Handler Functions**

   - `getMovies`: Retrieves all movies.
   - `getMovie`: Retrieves a movie by ID.
   - `createMovie`: Creates a new movie.
   - `updateMovie`: Updates a movie by ID.
   - `deleteMovies`: Deletes a movie by ID.

4. **Router and Server**

   The `mux.NewRouter()` from the `gorilla/mux` package is used to define routes and map them to handler functions.

   ```go
   r.HandleFunc("/movies", getMovies).Methods("GET")
   r.HandleFunc("/movies/{id}", getMovie).Methods("GET")
   r.HandleFunc("/movies", createMovie).Methods("POST")
   r.HandleFunc("/movies/{id}", updateMovie).Methods("PUT")
   r.HandleFunc("/movies/{id}", deleteMovies).Methods("DELETE")
   ```

   Finally, the server is started using:

   ```go
   log.Fatal(http.ListenAndServe(":8000", r))
   ```

## Example Requests

Here’s how you can test the API using `curl`.

1. **Get all movies**:
   ```bash
   curl http://localhost:8000/movies
   ```

2. **Get a movie by ID**:
   ```bash
   curl http://localhost:8000/movies/1
   ```

3. **Create a new movie**:
   ```bash
   curl -X POST -H "Content-Type: application/json" -d '{"isbn":"12345", "title":"New Movie", "director":{"firstname":"Jane", "lastname":"Doe"}}' http://localhost:8000/movies
   ```

4. **Update an existing movie**:
   ```bash
   curl -X PUT -H "Content-Type: application/json" -d '{"isbn":"54321", "title":"Updated Movie", "director":{"firstname":"John", "lastname":"Smith"}}' http://localhost:8000/movies/1
   ```

5. **Delete a movie**:
   ```bash
   curl -X DELETE http://localhost:8000/movies/1
   ```

