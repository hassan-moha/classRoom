# 📌 Task Management API Documentation

## 🏠 Base URL

```
https://classroom-ms9i.onrender.com
```
*(Replace with your deployed URL, e.g., `https://your-app-name.onrender.com/api`)*

---

## 🔐 Authentication

All endpoints (except registration and login) require a **JWT token**.

- **Header:**  
  `Authorization: Bearer <your_jwt_token>`

- Obtain a token by registering or logging in (see Auth endpoints).

---

## 🧑‍💻 Auth Endpoints

### Register

- **POST** `/auth/register`
- **Body:**
  ```json
  {
    "name": "John Doe",
    "email": "john@example.com",
    "password": "yourpassword"
  }
  ```
- **Response:**
  ```json
  {
    "success": true,
    "data": {
      "id": "user_id",
      "name": "John Doe",
      "email": "john@example.com"
    },
    "token": "<jwt_token>"
  }
  ```

### Login

- **POST** `/auth/login`
- **Body:**
  ```json
  {
    "email": "john@example.com",
    "password": "yourpassword"
  }
  ```
- **Response:**
  ```json
  {
    "success": true,
    "data": {
      "id": "user_id",
      "name": "John Doe",
      "email": "john@example.com"
    },
    "token": "<jwt_token>"
  }
  ```

### Get Current User

- **GET** `/auth/me`
- **Headers:**  
  `Authorization: Bearer <jwt_token>`
- **Response:**
  ```json
  {
    "success": true,
    "data": {
      "id": "user_id",
      "name": "John Doe",
      "email": "john@example.com"
    }
  }
  ```

---

## 📂 Task Endpoints

All require authentication.

### Get All Tasks

- **GET** `/tasks`
- **Response:**
  ```json
  {
    "success": true,
    "count": 2,
    "data": [
      {
        "id": "task_id",
        "title": "Build API",
        "description": "Implement endpoints",
        "status": "pending",
        "priority": "high",
        "dueDate": "2025-09-14T00:00:00.000Z",
        "assignedTo": "user_id",
        "subtasks": [ ... ]
      }
    ]
  }
  ```

### Get Task by ID

- **GET** `/tasks/{id}`
- **Response:**
  ```json
  {
    "success": true,
    "data": { ...task object... }
  }
  ```
- **404 Response:**
  ```json
  {
    "success": false,
    "error": "Task not found"
  }
  ```

### Create Task

- **POST** `/tasks`
- **Body:**
  ```json
  {
    "title": "New Task",
    "description": "This is a new task",
    "status": "pending",
    "priority": "medium",
    "dueDate": "2025-09-20T00:00:00.000Z",
    "assignedTo": "user_id",
    "subtasks": [
      {
        "title": "Subtask 1",
        "description": "Details"
      }
    ]
  }
  ```
- **201 Response:**
  ```json
  {
    "success": true,
    "data": { ...created task object... }
  }
  ```

### Update Task

- **PUT** `/tasks/{id}`
- **Body:** (any updatable fields)
  ```json
  {
    "title": "Updated Task",
    "status": "completed"
  }
  ```
- **Response:**
  ```json
  {
    "success": true,
    "data": { ...updated task object... }
  }
  ```

### Delete Task

- **DELETE** `/tasks/{id}`
- **Response:**
  ```json
  {
    "success": true,
    "data": { ...deleted task object... }
  }
  ```

---

## 📝 Subtask Endpoints

### Get All Subtasks for a Task

- **GET** `/tasks/{taskId}/subtasks`
- **Response:**
  ```json
  {
    "success": true,
    "count": 2,
    "data": [
      {
        "id": "subtask_id",
        "title": "Write controllers",
        "completed": false
      }
    ]
  }
  ```

### Get Subtask by ID

- **GET** `/subtasks/{id}`
- **Response:**
  ```json
  {
    "success": true,
    "data": { ...subtask object... }
  }
  ```

### Create Subtask

- **POST** `/tasks/{taskId}/subtasks`
- **Body:**
  ```json
  {
    "title": "New Subtask",
    "description": "Details"
  }
  ```
- **201 Response:**
  ```json
  {
    "success": true,
    "data": { ...created subtask object... }
  }
  ```

### Update Subtask

- **PUT** `/subtasks/{id}`
- **Body:**
  ```json
  {
    "title": "Updated Subtask",
    "completed": true
  }
  ```
- **Response:**
  ```json
  {
    "success": true,
    "data": { ...updated subtask object... }
  }
  ```

### Delete Subtask

- **DELETE** `/subtasks/{id}`
- **Response:**
  ```json
  {
    "success": true,
    "data": { ...deleted subtask object... }
  }
  ```

---

## ⚠️ Error Handling

| Status | Meaning                        | Example Response                                 |
|--------|--------------------------------|--------------------------------------------------|
| 400    | Bad Request                    | `{ "success": false, "error": "Invalid input" }` |
| 401    | Unauthorized (missing/invalid) | `{ "success": false, "message": "Access token required" }` |
| 404    | Not Found                      | `{ "success": false, "error": "Task not found" }`|
| 500    | Internal Server Error          | `{ "success": false, "message": "Something went wrong!" }`|

---

## 🛡️ Security Notes

- Always keep your JWT token secret.
- Passwords are hashed using bcrypt.
- All sensitive endpoints require authentication.

---

## 🗂️ Data Models (Prisma)

**User:**  
- id, email, password, name, createdAt, updatedAt

**Task:**  
- id, title, description, status (`pending`, `in_progress`, `completed`, `cancelled`), priority (`low`, `medium`, `high`, `urgent`), dueDate, assignedTo, userId, subtasks

**Subtask:**  
- id, title, description, completed, taskId

---

## 📝 Example Usage

**cURL Example:**
```bash
curl -H "Authorization: Bearer <jwt_token>" http://localhost:3000/api/tasks
```

---

For more details, see the code and comments in the repository.

---
