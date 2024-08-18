

# Platform Service

Platform Service is a Django-based API backend service that provides robust CRUD operations, model management, and asynchronous task processing using Celery. This service is secured with an additional `App-Secret-Key` header for all except `GET` requests.


## Project Structure

> [!NOTE]
> Note all files and folders are listed, specifically most django generated files

platform_service/
├── api/
│   ├── apps.py                # App Congig
│   ├── custom_views.py        # Custom views (CRUD operations, generic, and APIViews)
│   ├── exceptions/py          # Custom error exceptions 
│   ├── serializers.py         # Serializers for all models
│   ├── pagination.py          # Pagination logic
│   ├── tasks.py               # Celery tasks used in the views
│   ├── tests.py               # Unit tests
│   └── urls.py                # API URL routing
│   ├── utils.py               # rest framework utility functions
│   ├── views.py               # Model views 

├── library/
│   ├── func/                  # Helper functions
│   ├── middleware/            # DJango middlewares
│   ├── enum.py                # Model enums


├── main/
│   ├── settings.py          # Django settings
│   ├── urls.py              # Main URL configuration
│   ├── celery.py            # Celery setup
│   └── views.py             # CSRF, invalid route handling, tasks

├── platform/
│   ├── models/              # All model definitions
│   ├── signals/             # Django signals for triggering tasks
│   └── tasks/               # Celery tasks triggered by signals

## Getting Started

### Requirements
- Python 3.x
- Django 4.x
- Celery 5.x
- RabbitMQ (for Celery broker)
- MongoDb
- Postgres

### Installation

1. Clone the repository:
   
        git clone https://github.com/reccely/official-platform-service.git
        cd platform-service

2. Install the required packages:
    
        pip install -r requirements.txt

3. Set up your environment variables. You can create a `.env` file at the root of your project:
   SECRET_KEY=your_secret_key
   DEBUG=True
   DATABASE_URL=your_database_url
   APP_SECRET_KEY=your_app_secret_key


### Environment Variables

| Variable               | Description                             | Example                  |
|------------------------|-----------------------------------------|--------------------------|
| `GOOGLE_MAP_API_KEY`    | API key for Google Maps integration     | `your_google_map_key`     |
| `DJANGO_SECRET_KEY`     | Django secret key                       | `supersecretkey`          |
| `SQL_DATABASE`          | Database name                           | `platform_db`             |
| `SQL_HOST`              | Database host                           | `localhost`               |
| `SQL_USER`              | Database user                           | `db_user`                 |
| `SQL_PASSWORD`          | Database password                       | `password123`             |
| `SQL_PORT`              | Database port                           | `5432`                    |
| `DJANGO_SETTINGS_MODULE`| Django settings module                  | `platform_service.settings`|
| `MONGO_DB`              | MongoDB database name                   | `platform_mongo_db`       |
| `RABBITMQ_BROKER`       | RabbitMQ broker URL                     | `amqp://localhost`        |
| `RABBITMQ_API_URL`      | RabbitMQ management API URL             | `http://localhost:15672`  |
| `RABBITMQ_USERNAME`     | RabbitMQ username                       | `rabbitmq_user`           |
| `RABBITMQ_PASSWORD`     | RabbitMQ password                       | `rabbitmq_pass`           |
| `VHOST`                 | RabbitMQ virtual host                   | `/`                       |
| `ENV`                   | Environment (development/production)    | `development`             |


### Running the Project
1. Make migrations
        
        python manage.py makemigrations platforms


2. Apply migrations:

        python manage.py migrate

3. Run the development server:
        
        python manage.py runserver

> [!IMPORTANT]
> A supervisord.conf file has been created to run all process including celery

        supervisord ./supervisord.conf

### Running Celery
Make sure RabbitMQ or Redis is running, then run Celery:
        
        celery -A platform_service worker -l info

also:

        celery -A main worker --loglevel=info -Q platform_default,platform_priority,platform_signal


## API Documentation
### Request Headers

All requests must include the following headers:

| Header            | Required | Description                       |
|-------------------|----------|-----------------------------------|
| `App-Secret-Key`  | Yes      | Validates the incoming request    |
| `Content-Type`    | Yes      | Defines the content type (JSON)   |
| `Authorization`   | Optional | For endpoints that require auth   |

### Endpoints
#### CRUD Operations

#### CRUD Operations

Below are the available endpoints for the Platform Service. Each endpoint supports standard CRUD operations such as list, create, detail, update, and delete where applicable.


| Resource             | Method  | Endpoint                                          | Description                              |
|----------------------|---------|--------------------------------------------------|------------------------------------------|
| Locations            | GET     | `/locations/`                                    | List all locations                       |
|                      | POST    | `/locations/`                                    | Create a new location                    |
|                      | DELETE  | `/locations/`                                    | Bulk delete locations                    |
|                      | GET     | `/locations/<uuid:pk>/`                          | Retrieve location details                |
|                      | PATCH   | `/locations/<uuid:pk>/`                          | Partially update a location              |
|                      | PUT     | `/locations/<uuid:pk>/`                          | Fully update a location                  |
|                      | POST    | `/locations/bulk/`                               | Bulk create locations                    |
| Platforms            | GET     | `/platforms/`                                    | List all platforms                       |
|                      | POST    | `/platforms/`                                    | Create a new platform                    |
|                      | DELETE  | `/platforms/`                                    | Bulk delete platforms                    |
|                      | GET     | `/platforms/<uuid:pk>/`                          | Retrieve platform details                |
|                      | PATCH   | `/platforms/<uuid:pk>/`                          | Partially update a platform              |
|                      | PUT     | `/platforms/<uuid:pk>/`                          | Fully update a platform                  |
|                      | POST    | `/platforms/bulk/`                               | Bulk create platforms                    |
| Venues               | GET     | `/venues/`                                       | List all venues                          |
|                      | DELETE  | `/venues/`                                       | Bulk delete venues                       |
|                      | GET     | `/venues/<uuid:pk>/`                             | Retrieve venue details                   |
|                      | PATCH   | `/venues/<uuid:pk>/`                             | Partially update a venue                 |
|                      | PUT     | `/venues/<uuid:pk>/`                             | Fully update a venue                     |
| Venue Segments       | GET     | `/venue-segments/`                               | List all venue segments                  |
|                      | POST    | `/venue-segments/`                               | Create a new venue segment               |
|                      | DELETE  | `/venue-segments/`                               | Bulk delete venue segments               |
|                      | GET     | `/venue-segments/<str:pk>/`                      | Retrieve venue segment details           |
|                      | PATCH   | `/venue-segments/<str:pk>/`                      | Partially update a venue segment         |
|                      | PUT     | `/venue-segments/<str:pk>/`                      | Fully update a venue segment             |
| Events               | GET     | `/events/`                                       | List all events                          |
|                      | DELETE  | `/events/`                                       | Bulk delete events                       |
|                      | GET     | `/events/<uuid:pk>/`                             | Retrieve event details                   |
|                      | PATCH   | `/events/<uuid:pk>/`                             | Partially update an event                |
|                      | PUT     | `/events/<uuid:pk>/`                             | Fully update an event                    |
| Event Categories     | GET     | `/event-categories/`                             | List all event categories                |
|                      | POST    | `/event-categories/`                             | Create a new event category              |
|                      | DELETE  | `/event-categories/`                             | Bulk delete event categories             |
|                      | GET     | `/event-categories/<str:pk>/`                    | Retrieve event category details          |
|                      | PATCH   | `/event-categories/<str:pk>/`                    | Partially update an event category       |
|                      | PUT     | `/event-categories/<str:pk>/`                    | Fully update an event category           |
| Posts                | GET     | `/posts/`                                        | List all posts                           |
|                      | DELETE  | `/posts/`                                        | Bulk delete posts                        |
|                      | GET     | `/posts/<uuid:pk>/`                              | Retrieve post details                    |
|                      | PATCH   | `/posts/<uuid:pk>/`                              | Partially update a post                  |
|                      | PUT     | `/posts/<uuid:pk>/`                              | Fully update a post                      |
| Topics               | GET     | `/topics/`                                       | List all topics                          |
|                      | DELETE  | `/topics/`                                       | Bulk delete topics                       |
|                      | GET     | `/topics/<uuid:pk>/`                             | Retrieve topic details                   |
|                      | PATCH   | `/topics/<uuid:pk>/`                             | Partially update a topic                 |
|                      | PUT     | `/topics/<uuid:pk>/`                             | Fully update a topic                     |
| Topic Interactions   | GET     | `/topic-interactions/`                           | List all topic interactions              |
|                      | POST    | `/topic-interactions/`                           | Create a new topic interaction           |
|                      | DELETE  | `/topic-interactions/`                           | Bulk delete topic interactions           |
|                      | GET     | `/topic-interactions/<uuid:pk>/`                 | Retrieve topic interaction details       |
|                      | PATCH   | `/topic-interactions/<uuid:pk>/`                 | Partially update a topic interaction     |
|                      | PUT     | `/topic-interactions/<uuid:pk>/`                 | Fully update a topic interaction         |
| Tags                 | GET     | `/tags/`                                         | List all tags                            |
|                      | POST    | `/tags/`                                         | Create a new tag                         |
|                      | DELETE  | `/tags/`                                         | Bulk delete tags                         |
|                      | GET     | `/tags/<str:pk>/`                                | Retrieve tag details                     |
|                      | PATCH   | `/tags/<str:pk>/`                                | Partially update a tag                   |
|                      | PUT     | `/tags/<str:pk>/`                                | Fully update a tag                       |
| Activities           | GET     | `/activities/`                                   | List all activities                      |
|                      | POST    | `/activities/`                                   | Create a new activity                    |
|                      | DELETE  | `/activities/`                                   | Bulk delete activities                   |
|                      | GET     | `/activities/<int:pk>/`                          | Retrieve activity details                |
|                      | PATCH   | `/activities/<int:pk>/`                          | Partially update an activity             |
|                      | PUT     | `/activities/<int:pk>/`                          | Fully update an activity                 |
| Websites             | GET     | `/websites/`                                     | List all websites                        |
|                      | POST    | `/websites/`                                     | Create a new website                     |
|                      | DELETE  | `/websites/`                                     | Bulk delete websites                     |
|                      | GET     | `/websites/<int:pk>/`                            | Retrieve website details                 |
|                      | PATCH   | `/websites/<int:pk>/`                            | Partially update a website               |
|                      | PUT     | `/websites/<int:pk>/`                            | Fully update a website                   |
| Reservations         | GET     | `/reservations/`                                 | List all reservations                    |
|                      | POST    | `/reservations/`                                 | Create a new reservation                 |
|                      | DELETE  | `/reservations/`                                 | Bulk delete reservations                 |
|                      | GET     | `/reservations/<int:pk>/`                        | Retrieve reservation details             |
|                      | PATCH   | `/reservations/<int:pk>/`                        | Partially update a reservation           |
|                      | PUT     | `/reservations/<int:pk>/`                        | Fully update a reservation               |
| Comments             | GET     | `/comments/`                                     | List all comments                        |
|                      | POST    | `/comments/`                                     | Create a new comment                     |
|                      | DELETE  | `/comments/`                                     | Bulk delete comments                     |
|                      | GET     | `/comments/<int:pk>/`                            | Retrieve comment details                 |
|                      | PATCH   | `/comments/<int:pk>/`                            | Partially update a comment               |
|                      | PUT     | `/comments/<int:pk>/`                            | Fully update a comment                   |
| Files                | GET     | `/files/`                                        | List all files                           |
|                      | POST    | `/files/`                                        | Create a new file                        |
|                      | DELETE  | `/files/`                                        | Bulk delete files                        |
|                      | GET     | `/files/<int:pk>/`                               | Retrieve file details                    |
|                      | PATCH   | `/files/<int:pk>/`                               | Partially update a file                  |
|                      | PUT     | `/files/<int:pk>/`                               | Fully update a file                      |
| Memberships          | GET     | `/memberships/`                                  | List all memberships                     |
|                      | POST    | `/memberships/`                                  | Create a new membership                  |
|                      | DELETE  | `/memberships/`                                  | Bulk delete memberships                  |
|                      | GET     | `/memberships/<int:pk>/`                         | Retrieve membership details              |
|                      | PATCH   | `/memberships/<int:pk>/`                         | Partially update a membership            |
|                      | PUT     | `/memberships/<int:pk>/`                         | Fully update a membership                |


#### Pagination

All list endpoints are paginated. Use the following parameters to navigate the results:

| Parameter   | Type    | Description                 |
|-------------|---------|-----------------------------|
| `page`      | Integer | The page number to retrieve  |
| `page_size` | Integer | Number of items per page     |

#### Query Parameters

All list views support filtering, searching, and ordering.

| Parameter         | Type                   | Description                       |
|-------------------|------------------------|-----------------------------------|
| `search`          | String                 | Search term for full-text search  |
| `ordering`        | String                 | Field name to order results by    |
| `filterset_fields`| Filterable Model field | Field-specific filtering          |

## Models
### LocationModel

| Field            | Type        | Description                                |
|------------------|-------------|--------------------------------------------|
| `id`             | UUID        | Unique identifier for the location         |
| `longitude`      | FloatField   | Longitude coordinate                      |
| `latitude`       | FloatField   | Latitude coordinate                       |
| `city`           | CharField    | City name                                 |
| `state`          | CharField    | State name                                |
| `country`        | CharField    | Country name                              |
| `country_short`  | CharField    | Short code for the country                |
| `locality`       | CharField    | Locality name                             |
| `sublocality`    | CharField    | Sublocality name                          |
| `postal_code`    | CharField    | Postal code                               |
| `updated_at`     | DateTimeField| Last updated timestamp                    |

- **Filterable Fields**: `longitude`, `latitude`, `city`, `state`, `country`, `country_short`, `locality`, `sublocality`, `postal_code`, `updated_at`
- **Searchable Fields**: `city`, `country`
- **Orderable Fields**: `updated_at`, `country`, `state`, `city`, `locality`, `sublocality`

### PlatformModel

| Field           | Type         | Description                               |
|-----------------|--------------|-------------------------------------------|
| `id`            | UUID         | Unique identifier for the platform        |
| `name`          | CharField     | Name of the platform                      |
| `description`   | TextField     | Description of the platform               |
| `account_type`  | CharField     | Type of account (e.g., free, premium)     |
| `user`          | ForeignKey    | User associated with the platform         |
| `business`      | ForeignKey    | Business associated with the platform     |
| `type`          | CharField     | Platform type (e.g., social, e-commerce)  |
| `event`         | ForeignKey    | Event associated with the platform        |
| `venue`         | ForeignKey    | Venue associated with the platform        |
| `post`          | ForeignKey    | Post associated with the platform         |
| `topic`         | ForeignKey    | Topic associated with the platform        |
| `default_img`   | ImageField    | Default image for the platform            |

- **Filterable Fields**: `account_type`, `type`, `user`, `business`, `venue`, `event`, `post`, `topic`, `type`
- **Searchable Fields**: `name`, `description`
- **Orderable Fields**: `name`, `created_at`

### VenueModel

| Field           | Type         | Description                               |
|-----------------|--------------|-------------------------------------------|
| `id`            | UUID         | Unique identifier for the venue           |
| `location`      | ForeignKey    | Location associated with the venue        |
| `rating`        | FloatField    | Rating of the venue                       |
| `category`      | CharField     | Category of the venue                     |
| `open_time`     | TimeField     | Opening time of the venue                 |
| `close_time`    | TimeField     | Closing time of the venue                 |
| `open_day`      | CharField     | Day(s) the venue is open                  |
| `close_day`     | CharField     | Day(s) the venue is closed                |
| `capacity`      | IntegerField  | Seating capacity of the venue             |
| `city`          | CharField     | City where the venue is located           |

- **Filterable Fields**: `location__city`, `rating`, `category`, `open_time`, `close_time`, `open_day`, `close_day`, `capacity`
- **Searchable Fields**: `category`
- **Orderable Fields**: `rating`

### VenueSegmentModel

| Field           | Type         | Description                               |
|-----------------|--------------|-------------------------------------------|
| `id`            | UUID         | Unique identifier for the segment         |
| `venue`         | ForeignKey    | Venue associated with the segment         |
| `label`         | CharField     | Label for the segment                     |

- **Filterable Fields**: `venue`, `label`
- **Searchable Fields**: `label`
- **Orderable Fields**: `label`

### EventModel

| Field           | Type         | Description                               |
|-----------------|--------------|-------------------------------------------|
| `id`            | UUID         | Unique identifier for the event           |
| `venue`         | ForeignKey    | Venue associated with the event           |
| `is_virtual`    | BooleanField  | Whether the event is virtual              |
| `capacity`      | IntegerField  | Capacity of the event                     |
| `start_time`    | DateTimeField | Start time of the event                   |
| `end_time`      | DateTimeField | End time of the event                     |

- **Filterable Fields**: `venue`, `is_virtual`, `capacity`, `start_time`, `end_time`
- **Searchable Fields**: `capacity`
- **Orderable Fields**: `start_time`, `end_time`

### EventCategoryModel

| Field           | Type         | Description                               |
|-----------------|--------------|-------------------------------------------|
| `id`            | UUID         | Unique identifier for the event category  |
| `event`         | ForeignKey    | Event associated with the category        |
| `category`      | CharField     | Category name                             |

- **Filterable Fields**: `event`, `category`
- **Searchable Fields**: `category`
- **Orderable Fields**: `category`

### PostModel

| Field           | Type         | Description                               |
|-----------------|--------------|-------------------------------------------|
| `id`            | UUID         | Unique identifier for the post            |
| `category`      | ForeignKey    | Category associated with the post         |
| `venue`         | ForeignKey    | Venue associated with the post            |
| `expires_at`    | DateTimeField | Expiration date of the post               |

- **Filterable Fields**: `category`, `venue`, `expires_at`
- **Searchable Fields**: `category`
- **Orderable Fields**: `expires_at`

### TopicModel

| Field           | Type         | Description                               |
|-----------------|--------------|-------------------------------------------|
| `id`            | UUID         | Unique identifier for the topic           |
| `category`      | CharField     | Category associated with the topic        |
| `related_platform`| ForeignKey  | Platform associated with the topic        |

- **Filterable Fields**: `category`, `related_platform__id`
- **Searchable Fields**: `category`
- **Orderable Fields**: `category`

### TopicInteractionModel

- | Field          | Type         | Description                                  |
  |----------------|--------------|----------------------------------------------|
  | `id`           | BigAutoField  | Unique identifier for the topic interaction  |
  | `user`         | UUIDField     | User associated with the interaction         |
  | `content`      | TextField     | Content of the interaction                   |
  | `topic`        | ForeignKey    | Topic associated with the interaction        |

**Filterable Fields**: `user`, `topic`, `updated_at`  
**Searchable Fields**: `content`  
**Orderable Fields**: `updated_at`  

### TagModel

| Field           | Type         | Description                               |
|-----------------|--------------|-------------------------------------------|
| `id`            | UUID         | Unique identifier for the tag             |
| `platform`      | ForeignKey    | Platform associated with the tag          |
| `tag`           | CharField     | Tag name                                  |

- **Filterable Fields**: `platform`, `tag`
- **Searchable Fields**: `tag`, `platform__name`, `platform__description`
- **Orderable Fields**: `tag`

### ActivityModel

| Field           | Type         | Description                               |
|-----------------|--------------|-------------------------------------------|
| `id`            | UUID         | Unique identifier for the activity        |
| `platform`      | ForeignKey    | Platform associated with the activity     |
| `views`         | IntegerField  | Number of views                           |
| `likes`         | IntegerField  | Number of likes                           |
| `shares`        | IntegerField  | Number of shares                          |
| `flags`         | IntegerField  | Number of flags                           |
| `visibility`    | CharField     | Visibility status                         |
| `is_blocked`    | BooleanField  | Whether the activity is blocked           |
| `is_free`       | BooleanField  | Whether the activity is free              |
| `age_limit`     | IntegerField  | Age limit for the activity                |

- **Filterable Fields**: `platform`, `views`, `likes`, `shares`, `flags`, `visibility`, `is_blocked`, `is_free`, `age_limit`
- **Searchable Fields**: `platform__name`, `platform__description`
- **Orderable Fields**: `id`

### WebsiteModel

| Field           | Type         | Description                               |
|-----------------|--------------|-------------------------------------------|
| `id`            | UUID         | Unique identifier for the website         |
| `link_type`     | CharField     | Type of the link (e.g., official, social) |
| `platform`      | ForeignKey    | Platform associated with the website      |
| `link`          | URLField      | URL of the website                        |
| `created_at`    | DateTimeField | Creation timestamp                        |

- **Filterable Fields**: `link_type`, `platform`
- **Searchable Fields**: `link`, `platform__name`, `platform__description`
- **Orderable Fields**: `link`, `created_at`

### ReservationModel

| Field           | Type         | Description                               |
|-----------------|--------------|-------------------------------------------|
| `id`            | UUID         | Unique identifier for the reservation     |
| `type`          | CharField     | Type of reservation                       |
| `platform`      | ForeignKey    | Platform associated with the reservation  |
| `link`          | URLField      | URL related to the reservation            |
| `name`          | CharField     | Name of the reservation                   |

- **Filterable Fields**: `type`, `platform`, `link`, `name`
- **Searchable Fields**: `type`, `platform__name`, `platform__description`
- **Orderable Fields**: `id`

### CommentModel

| Field           | Type         | Description                               |
|-----------------|--------------|-------------------------------------------|
| `id`            | UUID         | Unique identifier for the comment         |
| `description`   | TextField     | Content of the comment                    |
| `platform`      | ForeignKey    | Platform associated with the comment      |
| `user`          | ForeignKey    | User who made the comment                 |
| `created_at`    | DateTimeField | Creation timestamp                        |

- **Filterable Fields**: `description`, `platform`, `user`
- **Searchable Fields**: `description`, `platform__name`, `platform__description`
- **Orderable Fields**: `created_at`

### FileModel

| Field           | Type         | Description                               |
|-----------------|--------------|-------------------------------------------|
| `id`            | UUID         | Unique identifier for the file            |
| `name`          | CharField     | Name of the file                          |
| `platform`      | ForeignKey    | Platform associated with the file         |
| `type`          | CharField     | Type of the file (e.g., image, document)  |
| `link`          | URLField      | URL or path to the file                   |
| `size`          | IntegerField  | Size of the file in bytes                 |

- **Filterable Fields**: `name`, `platform`, `type`, `link`
- **Searchable Fields**: `platform__name`, `platform__description`, `name`, `type`
- **Orderable Fields**: `size`, `type`

### MembershipModel

| Field           | Type         | Description                               |
|-----------------|--------------|-------------------------------------------|
| `id`            | UUID         | Unique identifier for the membership      |
| `user`          | ForeignKey    | User associated with the membership       |
| `platform`      | ForeignKey    | Platform associated with the membership   |
| `is_blocked`    | BooleanField  | Whether the membership is blocked         |
| `created_at`    | DateTimeField | Creation timestamp                        |

- **Filterable Fields**: `user`, `platform`, `is_blocked`, `created_at`
- **Searchable Fields**: `platform__name`, `platform__description`
- **Orderable Fields**: `created_at`


## Celery Tasks

All non-GET requests (POST, PUT, DELETE) use Celery for asynchronous task processing. Below are common tasks that are triggered by user operations:

| Task Name                         | Description                                                |
|-----------------------------------|------------------------------------------------------------|
| `api.tasks.create_location`        | Asynchronously creates or updates a location entry.       |
| `api.tasks.process_categories`     | Asynchronously processes and creates event categories.    |
| `api.tasks.process_venue_segment`  | Asynchronously processes and creates venue segments.      |
| `api.tasks.create_platform`        | Asynchronously creates a platform and associated types.   |
| `api.tasks.update_task`            | Asynchronously updates a model instance using a serializer.|
| `api.tasks.create_object_task`     | Asynchronously creates a model instance using a serializer.|
| `api.tasks.delete_object_task`     | Asynchronously deletes model instances by their IDs.      |
| `platforms.tasks.delete_expired_posts` | Deletes posts that have expired based on the current datetime. |
| `platforms.tasks.delete_platform` | Deletes platform instances based on the provided query. |
| `platforms.tasks.delete_post`    | Deletes a single post identified by post_id.      |
| `platforms.tasks.create_member`  | Creates a new membership instance for a user and platform. |
| `platforms.tasks.create_activity` | Creates a new activity instance for the given platform. |

### `api.tasks.create_location`

- **Description**: Creates or updates a location based on the provided data.
- **Arguments**:
  - `data` (dict): Location data.
- **Returns**: Created or updated location data or error message.

### `api.tasks.process_categories`

- **Description**: Creates event categories for an event model.
- **Arguments**:
  - `category` (str): The category to create.
  - `id` (str): The event ID.
- **Returns**: Created event category data or error message.

### `api.tasks.process_venue_segment`

- **Description**: Creates venue segments for a venue model.
- **Arguments**:
  - `segment` (str): The segment to create.
  - `id` (str): The venue ID.
- **Returns**: Created venue segment data or error message.

### `api.tasks.create_platform`

- **Description**: Creates a platform and associated type data.
- **Arguments**:
  - `platform` (dict): Platform data.
  - `platform_type` (dict): Platform type data.
  - `location` (dict, optional): Location data.
  - `venue_segments` (list, optional): List of venue segment names.
  - `event_categories` (list, optional): List of event category names.
- **Returns**: Created platform data or error message.

### `api.tasks.update_task`

- **Description**: Updates an existing model instance using a serializer.
- **Arguments**:
  - `object_id` (str): ID of the model instance.
  - `data` (dict): Data to update.
  - `serializer_path` (str): Path to the serializer class.
  - `partial` (bool, optional): Whether to perform a partial update.
- **Returns**: Updated model data or error message.

### `api.tasks.create_object_task`

- **Description**: Creates a new model instance using a serializer.
- **Arguments**:
  - `data` (dict): Data to create the model instance.
  - `serializer_path` (str): Path to the serializer class.
- **Returns**: Created model data or error message.

### `api.tasks.delete_object_task`

- **Description**: Deletes model instances by their IDs.
- **Arguments**:
  - `data` (list): List of IDs of the model instances to delete.
  - `serializer_path` (str): Path to the serializer class.
- **Returns**: Dictionary containing the delete count or error message.

### `platforms.tasks.delete_expired_posts`

- **Description**: Deletes posts that have expired based on the current datetime.
- **Arguments**: None
- **Returns**: Log message indicating the number of deleted posts or an error message.

### `platforms.tasks.delete_platform`

- **Description**: Deletes platform instances based on the provided query.
- **Arguments**:
  - `query` (dict): Query parameters to filter the platform instances.
- **Returns**: Log message indicating the number of deleted platform instances or an error message.

### `platforms.tasks.delete_post`

- **Description**: Deletes a single post identified by post_id.
- **Arguments**:
  - `post_id` (integer): ID of the post to delete.
- **Returns**: Log message indicating the result of the deletion or an error message.

### `platforms.tasks.create_member`

- **Description**: Creates a new membership instance for a user and platform.
- **Arguments**:
  - `user_id` (integer): ID of the user.
  - `platform_id` (integer): ID of the platform.
- **Returns**: Log message indicating the creation of the membership or an error message.

### `platforms.tasks.create_activity`

- **Description**: Creates a new activity instance for the given platform.
- **Arguments**:
  - `platform_id` (integer): ID of the platform.
- **Returns**: Log message indicating the creation of the activity or an error message.
## Testing

To run tests, use the following command:
        
        python manage.py test
    
        pytest

## License

This project is licensed under the MIT License.


