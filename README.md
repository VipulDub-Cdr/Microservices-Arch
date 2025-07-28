## MICROSERVICE ARCHITECTURE AND IMPLEMENTATION

![Alt text](./images/image.png)

### The Four Services:
    1. API 1 – Handles user registration via POST (port 5000)

    2. API 2 – Handles user data retrieval via GET (port 5001)

    3. PostgreSQL – Stores user data permanently

    4. Redis – Caches user data for faster read access

### Workflow Explanation
    Step 1: POST Request to Register User

        The user sends a POST request to API 1 at port 5000.

        API 1 receives the request and stores the user data in the PostgreSQL database.

        Optionally, the same data can also be cached in Redis, although the diagram shows caching only during GET requests.

    Step 2: GET Request to Fetch User

        The user sends an HTTP GET request to API 2 at port 5001 using the endpoint /user/<name>.

        API 2 first checks Redis for the requested user data:

            If there is a cache hit, Redis returns the data directly.

            If there is a cache miss, Redis queries PostgreSQL, retrieves the data, stores it in Redis for future use, and returns it to API 2.

        API 2 then sends the response back to the user.

Dockerized Setup (All Four Services Containerized)

Each component runs as a separate Docker container:

    1. api1 container (e.g., built using Flask, FastAPI, or Express) - handles POST requests.

    2. api2 container - handles GET requests and caching logic.

    3. postgres container - stores persistent user data.

    4. redis container - provides in-memory caching for faster retrieval.


Register a user by sending a POST request to API 1 running on port 5000

Run the command:
    curl -X POST -H "Content-Type: application/json" http://<ip>:5000/register -d '{"name":"name","info":"info"}'
