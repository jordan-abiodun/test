# Event service server

- Service accepts `restful api` and `graphql` requests to perform CRUD operations in account database
- Management of event database


| key               | Description                                                                     |
| ----------------- | --------------------------------------------------------------------------------|
|                   |                                                                                 |

# Platform Service

Platform Service is a Django-based API backend service that provides robust CRUD operations, model management, and asynchronous task processing using Celery. This service is secured with an additional `App-Secret-Key` header for all requests.

## Table of Contents
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
  - [Requirements](#requirements)
  - [Installation](#installation)
  - [Environment Variables](#environment-variables)
  - [Running the Project](#running-the-project)
  - [Running Celery](#running-celery)
- [API Documentation](#api-documentation)
  - [Request Headers](#request-headers)
  - [Authentication](#authentication)
  - [Endpoints](#endpoints)
    - [CRUD Operations](#crud-operations)
    - [Pagination](#pagination)
    - [Query Parameters](#query-parameters)
- [Models](#models)
  - [UserModel](#usermodel)
  - [Other Models](#other-models)
- [Celery Tasks](#celery-tasks)
- [Testing](#testing)
- [Contributing](#contributing)
- [License](#license)

## Project Structure

```bash
platform_service/
├── api/
│   ├── views/               # Custom views (CRUD operations, generic, and APIViews)
│   ├── serializers/         # Serializers for all models
│   ├── pagination/          # Pagination logic
│   ├── tasks/               # Celery tasks used in the views
│   ├── tests/               # Unit tests
│   └── urls.py              # API URL routing
├── main/
│   ├── settings.py          # Django settings
│   ├── urls.py              # Main URL configuration
│   ├── celery.py            # Celery setup
│   └── views.py             # CSRF, invalid route handling
├── platform/
│   ├── models/              # All model definitions
│   ├── signals/             # Django signals for triggering tasks
│   └── tasks/               # Celery tasks triggered by signals
Getting Started
Requirements
Python 3.x
Django 4.x
Celery 5.x
RabbitMQ or Redis (for Celery broker)
Installation
Clone the repository:

bash
Copy code
git clone https://github.com/your-repo/platform-service.git
cd platform-service
Install the required packages:

bash
Copy code
pip install -r requirements.txt
Set up your environment variables. You can create a .env file at the root of your project:

bash
Copy code
SECRET_KEY=your_secret_key
DEBUG=True
DATABASE_URL=your_database_url
APP_SECRET_KEY=your_app_secret_key
Environment Variables
Variable	Description	Example
SECRET_KEY	Django secret key	supersecretkey
DEBUG	Enable or disable debug mode	True
DATABASE_URL	Database connection URL	postgres://...
APP_SECRET_KEY	Secret key for request validation	your_app_key
Running the Project
Apply migrations:

bash
Copy code
python manage.py migrate
Run the development server:

bash
Copy code
python manage.py runserver
Running Celery
Make sure RabbitMQ or Redis is running, then run Celery:

bash
Copy code
celery -A platform_service worker -l info
API Documentation
Request Headers
All requests must include the following headers:

Header	Required	Description
App-Secret-Key	Yes	Validates the incoming request
Content-Type	Yes	Defines the content type (JSON)
Authorization	Optional	For endpoints that require auth
Endpoints
CRUD Operations
For all models, CRUD operations are implemented using Django function-based views and APIViews. Non-GET methods (POST, PUT, DELETE) trigger asynchronous Celery tasks to handle the operations.

Example usage of CRUD operations:

bash
Copy code
GET /api/users/        # List all users
GET /api/users/<id>/   # Get specific user
POST /api/users/       # Create a new user
PUT /api/users/<id>/   # Update user information
DELETE /api/users/<id>/# Delete a user
Pagination
All list endpoints are paginated. Use the following parameters to navigate the results:

Parameter	Type	Description
page	Integer	The page number to retrieve
page_size	Integer	Number of items per page
Query Parameters
All list views support filtering, searching, and ordering.

Parameter	Type	Description
search	String	Search term for full-text search
ordering	String	Field name to order results by
filterset_fields	Various	Field-specific filtering
Models
UserModel
The UserModel represents the users in the system.

Field	Type	Description
id	Integer	Unique identifier
username	String	Username of the user
email	String	Email address
created_at	DateTime	Time of account creation
updated_at	DateTime	Last update to the account
More detailed documentation for other models can be found in the platform/models folder.

Celery Tasks
All non-GET requests (POST, PUT, DELETE) use Celery for asynchronous task processing. Below are common tasks that are triggered by user operations:

Task Name	Description
api.tasks.create_user	Asynchronously creates a new user
api.tasks.update_user	Asynchronously updates a user's information
platform.tasks.sync_data	Synchronizes platform data across services
Testing
To run tests, use the following command:

bash
Copy code
python manage.py test
Contributing
We welcome contributions to improve the Platform Service. To contribute:

Fork the repository.
Create a feature branch (git checkout -b new-feature).
Commit your changes (git commit -am 'Add new feature').
Push to the branch (git push origin new-feature).
Create a new Pull Request.
License
This project is licensed under the MIT License.

