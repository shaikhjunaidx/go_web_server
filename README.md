
# Simple Go Web Server

This repository contains a simple Go web server that handles basic HTTP requests.

## Overview

The `main.go` file defines a basic web server with the following functionalities:

1. Serves static files from the `./static` directory.
2. Handles a `/hello` endpoint that responds with a greeting.
3. Handles a `/form` endpoint that processes form submissions and returns the submitted data.

## Code Explanation

```go
package main

import (
	"fmt"
	"log"
	"net/http"
)

func helloHandler(w http.ResponseWriter, r *http.Request) {
	if r.URL.Path != "/hello" {
		http.Error(w, "404 not found", http.StatusNotFound)
		return
	}

	if r.Method != "GET" {
		http.Error(w, "Method is not supported", http.StatusNotFound)
		return
	}

	fmt.Fprintf(w, "Hey there!")
}

func formHandler(w http.ResponseWriter, r *http.Request) {
	if err := r.ParseForm(); err != nil {
		fmt.Fprintf(w, "ParseForm() err: %v", err)
		return
	}
	fmt.Fprintf(w, "POST request was successful\n")

	name := r.FormValue("name")
	phone := r.FormValue("phone")
	email := r.FormValue("email")

	fmt.Fprintf(w, "Name: %s\n", name)
	fmt.Fprintf(w, "Phone: %s\n", phone)
	fmt.Fprintf(w, "Email: %s\n", email)
}

func main() {
	fileServer := http.FileServer(http.Dir("./static"))

	http.Handle("/", fileServer)
	http.HandleFunc("/form", formHandler)
	http.HandleFunc("/hello", helloHandler)

	fmt.Printf("Starting a local server at port 8080\n")

	if err := http.ListenAndServe(":8080", nil); err != nil {
		log.Fatal(err)
	}
}
```

- The `main` package is used to declare the package for this executable program.
- The necessary packages `fmt`, `log`, and `net/http` are imported.

- `helloHandler` function handles requests to the `/hello` endpoint.
- It checks if the URL path is `/hello` and if the request method is `GET`.
- If the path is incorrect or the method is not `GET`, it returns a `404` error.
- If the request is valid, it responds with "Hey there!".

- `formHandler` function handles form submissions to the `/form` endpoint.
- It parses the form data and checks for any errors during parsing.
- If parsing is successful, it extracts `name`, `phone`, and `email` from the form data.
- It responds with a success message and echoes the submitted data back to the client.

- The `main` function sets up and starts the web server.
- `http.FileServer` serves static files from the `./static` directory.
- `http.Handle` and `http.HandleFunc` register the handlers for different endpoints.
- The server listens on port `8080`, and if there is an error, it logs the error and terminates the program.

## Running the Server

To run the server, execute the following command in your terminal:

```bash
go run main.go
```

The server will start and listen on port `8080`. You can access the following endpoints:

- `http://localhost:8080/` - Serves static files from the `./static` directory.
- `http://localhost:8080/hello` - Responds with "Hey there!".
- `http://localhost:8080/form.html` - Processes form submissions and echoes the submitted data.

## Directory Structure

```
.
├── main.go
└── static
    └── [static files]
```

- `main.go` - The main Go file containing the server code.
- `static` - Directory containing static files to be served.

## Conclusion

This simple Go web server demonstrates handling of static files and basic HTTP endpoints. It can be expanded further to include more functionalities as needed.
