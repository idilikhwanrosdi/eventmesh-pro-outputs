# eventmesh-pro API Blueprint

## Authentication (/auth)

| Method | Endpoint | Description | Input | Output | Auth |
|---|---|---|---|---|---|
| POST | `/auth/login` | Login with email and password | `{ "email": "user@example.com", "password": "password123" }` | `{ "token": "jwt_token", "user": { "id": "user_id", "email": "user@example.com" } }` | None |
| POST | `/auth/register` | Register a new user | `{ "email": "user@example.com", "password": "password123" }` | `{ "user": { "id": "user_id", "email": "user@example.com" } }` | None |

## Projects (/projects)

| Method | Endpoint | Description | Input | Output | Auth |
|---|---|---|---|---|---|
| GET | `/projects` | Get all projects |  | `[{ "id": "project_id", "name": "Project Name", "description": "Project description" }]` | JWT |
| POST | `/projects` | Create a new project | `{ "name": "Project Name", "description": "Project description" }` | `{ "id": "project_id", "name": "Project Name", "description": "Project description" }` | JWT |
| GET | `/projects/{projectId}` | Get a specific project |  | `{ "id": "project_id", "name": "Project Name", "description": "Project description" }` | JWT |
| PUT | `/projects/{projectId}` | Update a project | `{ "name": "New Project Name", "description": "New project description" }` | `{ "id": "project_id", "name": "New Project Name", "description": "New project description" }` | JWT |
| DELETE | `/projects/{projectId}` | Delete a project |  | `{}` | JWT |


## Components (/components)

| Method | Endpoint | Description | Input | Output | Auth |
|---|---|---|---|---|---|
| GET | `/components` | Get all components |  | `[{ "id": "component_id", "name": "Component Name", "type": "Component Type" }]` | JWT |
| POST | `/components` | Create a new component | `{ "name": "Component Name", "type": "Component Type", "projectId": "project_id" }` | `{ "id": "component_id", "name": "Component Name", "type": "Component Type" }` | JWT |
| GET | `/components/{componentId}` | Get a specific component |  | `{ "id": "component_id", "name": "Component Name", "type": "Component Type" }` | JWT |
| PUT | `/components/{componentId}` | Update a component | `{ "name": "New Component Name", "type": "New Component Type" }` | `{ "id": "component_id", "name": "New Component Name", "type": "New Component Type" }` | JWT |
| DELETE | `/components/{componentId}` | Delete a component |  | `{}` | JWT |


## Templates (/templates)

| Method | Endpoint | Description | Input | Output | Auth |
|---|---|---|---|---|---|
| GET | `/templates` | Get all templates |  | `[{ "id": "template_id", "name": "Template Name", "content": "<html>..." }]` | JWT |
| POST | `/templates` | Create a new template | `{ "name": "Template Name", "content": "<html>..." }` | `{ "id": "template_id", "name": "Template Name", "content": "<html>..." }` | JWT |
| GET | `/templates/{templateId}` | Get a specific template |  | `{ "id": "template_id", "name": "Template Name", "content": "<html>..." }` | JWT |
| PUT | `/templates/{templateId}` | Update a template | `{ "name": "New Template Name", "content": "<html>..." }` | `{ "id": "template_id", "name": "New Template Name", "content": "<html>..." }` | JWT |
| DELETE | `/templates/{templateId}` | Delete a template |  | `{}` | JWT |


## Users (/users)

| Method | Endpoint | Description | Input | Output | Auth |
|---|---|---|---|---|---|
| GET | `/users/{userId}` | Get a specific user |  | `{ "id": "user_id", "email": "user@example.com" }` | JWT |
| PUT | `/users/{userId}` | Update a user | `{ "email": "new_user@example.com" }` | `{ "id": "user_id", "email": "new_user@example.com" }` | JWT |
| DELETE | `/users/{userId}` | Delete a user |  | `{}` | JWT |


All endpoints under `/projects`, `/components`, `/templates`, and `/users/{userId}` (except GET) require JWT for authentication.  `/auth` endpoints do not require JWT.