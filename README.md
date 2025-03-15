# Library Management System
### This project is on hold presently.
Especially as the job opening has elapsed. In the future, it will be converted into a more generic API, not touted as "specifically made for Cowrywise".

## Overview
This project is a library management system built using FastAPI for the 
Cowrywise Backend Engineer (Infrastructure, API Engineer, Devops) role. 
It consists of two **independent** API services:

1. **Frontend API**: Allows users to browse and borrow books.
2. **Backend/Admin API**: Allows administrators to manage the library's book catalog and user records.

Both services use different data stores and communicate updates effectively.

---

## Features

### Frontend API
Users can:
- Enroll in the library using their email, first name, and last name.
- List all available books.
- Retrieve details of a single book by its ID.
- Filter books by publisher (e.g., Wiley, Apress, Manning) and category (e.g., fiction, technology, science).
- Borrow books by ID and specify the loan duration in days.

### Backend/Admin API
Admins can:
- Add new books to the catalog.
- Remove books from the catalog.
- Fetch a list of enrolled users.
- Fetch a list of users and their borrowed books.
- Fetch a list of books that are currently unavailable and their expected return dates.

---

## Requirements
* The endpoints need not be authenticated.
* The API can be built using any Python framework (I chose [FastAPI](https://fastapi.tiangolo.com))
* Design the models as you deem fit.
* A book that has been lent out should no longer be available in the catalogue.
* The two services should use different data stores.
* Device a way to communicate changes between the two services. i.e when the admin adds a book to the catalogue via the admin api, the frontend api should also be updated with the latest book added by the admin.
* The project should be deployed using Docker containers
* Add necessary unit/integration tests.

---

## Project Structure
```
📦 Cowrywise Backend Assessment
├── 📂 backend_api        # Admin API for book management
│   ├── 📂 tests          # Unit and integration tests
│   ├── main.py           # FastAPI app entry point
│   ├── models.py         # Database models
│   ├── routes.py         # API routes
│   ├── services.py       # Business logic
│   └── Dockerfile        # Docker containerization
├── 📂 frontend_api       # User-facing API
│   ├── 📂 tests          # Unit and integration tests
│   ├── main.py           # FastAPI app entry point
│   ├── models.py         # Database models
│   ├── routes.py         # API routes
│   ├── services.py       # Business logic
│   └── Dockerfile        # Docker containerization
├── .gitignore            # Standard gitignore.
├── docker-compose.yml    # Multi-container setup
├── pyproject.toml        # Project management file (for uv)
└── README.md             # Project documentation
```

---

## Installation & Setup
### Prerequisites
- Python 3.9+
- Docker & Docker Compose

### Running the Application
1. **Clone the repository:**
   ```sh
   git clone https://github.com/your-repo/library-management-system.git
   cd library-management-system
   ```
2. **Run the services using Docker:**
   ```sh
   docker-compose up --build
   ```
3. **Access the APIs:**
   - Frontend API: `http://localhost:8000/docs`
   - Backend/Admin API: `http://localhost:8001/docs`

---

## API Communication
Since the two services have separate databases, they must synchronize changes. Possible approaches:
1. **Message Queue (e.g., Redis, RabbitMQ, Kafka)** – Admin API sends book updates to a queue that the Frontend API listens to.
2. **Webhook Mechanism** – Frontend API subscribes to Admin API updates.
3. **Polling** – Frontend API periodically fetches updates from the Admin API.

I chose to implement the Message Queue route, using RabbitMQ.

---

## Testing
Run tests using `pytest`:
```sh
pytest frontend-api/tests
pytest admin-api/tests
```

---

## Deployment
To deploy to a production environment:
```sh
docker-compose -f docker-compose.prod.yml up --build -d
```
Modify `docker-compose.prod.yml` to configure production database settings.

---

## Contributing
1. Fork the repository.
2. Create a new branch (`feature-branch-name`).
3. Commit changes.
4. Push to your branch.
5. Open a Pull Request.

---

## License
MIT License
