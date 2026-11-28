NoteCraft – MERN Stack Backend

A full-stack note-taking and productivity application built using the MERN stack (MongoDB, Express.js, React.js and Node.js).

The NoteCraft backend provides RESTful APIs for user authentication, notebook management, notes, tags and task management. It uses MongoDB for persistent data storage and JWT for user authentication.

1. Project Overview

NoteCraft is a productivity application that allows users to create, organize and manage their personal notes and daily tasks.

The backend is developed using Node.js, Express.js and MongoDB with Mongoose.

Main Features
User registration and login.
JWT-based authentication.
User profile management.
Notebook creation and management.
Note creation, editing and deletion.
Tag creation and categorization.
Task management and tracking.
Task priorities and deadlines.
Task completion and reopening.
Note pinning and favorites.
Note archiving and trash management.
Search and filtering.
Pagination and sorting.
User-specific data access.
Centralized error handling.
Request validation.

Note: Some note and authentication features are planned API functionality and should only be considered complete after their controllers and routes are implemented and tested.

2. Technologies Used
Backend
Node.js – JavaScript runtime.
Express.js – Backend web framework.
MongoDB – NoSQL database.
Mongoose – MongoDB object modeling.
JWT – Authentication.
bcrypt – Password hashing.
dotenv – Environment variable management.
cors – Cross-origin resource sharing.
Development Tools
Visual Studio Code.
Postman.
MongoDB Compass.
Git.
GitHub.
npm.
3. Backend Project Structure
text
Server/
│
├── controllers/
│   ├── auth.controller.js
│   ├── notebook.controller.js
│   ├── tag.controller.js
│   ├── task.controller.js
│   └── note.controller.js
│
├── middleware/
│   ├── auth.middleware.js
│   ├── error.middleware.js
│   └── validate.middleware.js
│
├── models/
│   ├── User.js
│   ├── Notebook.js
│   ├── Tag.js
│   ├── Task.js
│   ├── Note.js
│   └── OtpChallenge.js
│
├── routes/
│   ├── auth.routes.js
│   ├── notebook.routes.js
│   ├── tag.routes.js
│   ├── task.routes.js
│   └── note.routes.js
│
├── services/
│   └── email.service.js
│
├── src/
│   └── config/
│       └── db.js
│
├── utils/
│   └── generateToken.js
│
├── .env
├── .env.example
├── .gitignore
├── package.json
├── package-lock.json
├── README.md
└── server.js

Folder Responsibilities

controllers/

Contains business logic for authentication, notes, notebooks, tags and tasks.

middleware/

Contains authentication, validation and centralized error-handling middleware.

models/

Contains Mongoose schemas defining the structure of MongoDB collections.

routes/

Contains Express API endpoints and connects routes to controllers.

services/

Contains reusable external service functionality, such as sending emails.

src/config/

Contains database and application configuration.

utils/

Contains reusable helper functions such as JWT generation.

server.js

The main application entry point. Initializes Express, connects MongoDB, registers middleware and mounts API routes.

4. Installation and Setup
Step 1: Clone the repository
bash
git clone YOUR_GITHUB_REPOSITORY_URL

Step 2: Navigate to the Server directory
bash
cd Server

Step 3: Install dependencies
bash
npm install

Step 4: Configure environment variables

Create a .env file in the Server directory.

dotenv
PORT=5000
NODE_ENV=development

MONGO_URI=mongodb://127.0.0.1:27017/notecraft

JWT_SECRET=your_long_random_secret
JWT_EXPIRES_IN=7d

CLIENT_URL=http://localhost:5173

EMAIL_HOST=
EMAIL_PORT=
EMAIL_USER=
EMAIL_PASSWORD=
EMAIL_FROM=


Replace the example values with your actual configuration.

Never upload .env to GitHub.

Step 5: Start MongoDB

Make sure MongoDB is running locally or configure a MongoDB Atlas connection string.

Step 6: Start the backend server

Development:

bash
npm run dev


Production:

bash
npm start


Default backend URL:

text
http://localhost:5000

5. Package Scripts

Add these scripts to your package.json:

json
{
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js"
  }
}


Install development dependencies if required:

bash
npm install --save-dev nodemon

6. Database Configuration

NoteCraft uses MongoDB as its primary database.

The application connects to MongoDB through Mongoose.

Database name:

text
notecraft

Main Collections
Collection	Purpose
users	Stores user accounts
notebooks	Stores user notebooks
notes	Stores notes
tags	Stores note categories
tasks	Stores user tasks
otpchallenges	Stores OTP verification records
Model Relationships
One user can create multiple notebooks.
One user can create multiple notes.
One user can create multiple tags.
One user can create multiple tasks.
One notebook can contain multiple notes.
One note can have multiple tags.
One task can have multiple tags.
One task can optionally belong to a notebook.

User ownership is maintained through MongoDB ObjectId references.

7. Authentication and Security

NoteCraft uses JWT authentication.

Authentication Process
User registers with their details.
Password is hashed before storage.
User logs in with email and password.
Backend verifies credentials.
Backend generates a JWT.
Frontend stores the authentication token securely.
Protected API routes verify the token.
The authenticated user's ID is used to access personal resources.
Authentication Middleware

File:

text
middleware/auth.middleware.js


Purpose:

Verify JWT tokens.
Reject unauthorized requests.
Identify authenticated users.
Attach user information to the request object.

Protected requests use:

http
Authorization: Bearer YOUR_JWT_TOKEN

8. Error Handling Middleware

File:

text
middleware/error.middleware.js


Responsibilities:

Handle MongoDB duplicate key errors.
Handle Mongoose validation errors.
Handle invalid ObjectIds.
Handle JWT errors.
Handle expired tokens.
Handle undefined API routes.
Return standardized JSON error responses.
Example Error Response
json
{
  "success": false,
  "status": "fail",
  "message": "Resource not found"
}

9. Validation Middleware

File:

text
middleware/validate.middleware.js


Responsibilities:

Validate request body.
Validate route parameters.
Validate query parameters.
Validate required strings.
Validate email addresses.
Validate MongoDB ObjectIds.
Validate enums.
Validate dates and arrays.

Validation middleware prevents invalid requests from reaching the controller.

10. API Documentation

Base URL:

text
http://localhost:5000

A. Authentication APIs

Base route:

text
/api/auth

Method	Endpoint	Description
POST	/register	Register user
POST	/login	Login user
GET	/me	Get current user
POST	/logout	Logout user
POST	/forgot-password	Request password reset
POST	/reset-password	Reset password
POST	/verify-otp	Verify OTP
POST	/resend-otp	Resend OTP
PUT	/change-password	Change password
PUT	/update-profile	Update profile

Implement and test only the endpoints registered in auth.routes.js.

B. Notebook APIs

Base route:

text
/api/notebooks

Method	Endpoint	Description
POST	/	Create notebook
GET	/	Get all notebooks
GET	/:id	Get notebook
PUT	/:id	Update notebook
DELETE	/:id	Delete notebook
GET	/:id/notes	Get notebook notes
C. Tag APIs

Base route:

text
/api/tags

Method	Endpoint	Description
POST	/	Create tag
GET	/	Get all tags
GET	/:id	Get tag
PUT	/:id	Update tag
DELETE	/:id	Delete tag
GET	/:id/notes	Get notes by tag
GET	/:id/stats	Get tag statistics
D. Task APIs

Base route:

text
/api/tasks

Method	Endpoint	Description
POST	/	Create task
GET	/	Get all tasks
GET	/stats	Get task statistics
GET	/:id	Get task
PUT	/:id	Update task
DELETE	/:id	Delete task
PATCH	/:id/complete	Complete task
PATCH	/:id/reopen	Reopen task
E. Note APIs

Base route:

text
/api/notes

Method	Endpoint	Description
POST	/	Create note
GET	/	Get all notes
GET	/:id	Get note
PUT	/:id	Update note
DELETE	/:id	Move note to trash
PATCH	/:id/pin	Pin/unpin note
PATCH	/:id/favorite	Favorite/unfavorite note
PATCH	/:id/archive	Archive/unarchive note
PATCH	/:id/restore	Restore note
DELETE	/:id/permanent	Permanently delete note
GET	/search	Search notes
GET	/stats	Note statistics
PATCH	/:id/lock	Lock/unlock note

These note endpoints describe the intended API design. Register them when their controller functions are ready.

11. API Testing with Postman

Use Postman to test all backend endpoints.

Testing Order
Test server health.
Register a user.
Login and obtain JWT.
Test protected authentication endpoints.
Create a notebook.
Create tags.
Create tasks.
Update and complete tasks.
Create notes.
Assign notes to notebooks and tags.
Test search and filters.
Test archive and trash functionality.
Test invalid requests.
Test authorization and user ownership.
Authentication Header

For protected endpoints:

http
Authorization: Bearer YOUR_JWT_TOKEN

Common HTTP Status Codes
Status	Meaning
200	Successful request
201	Resource created
400	Bad request
401	Unauthorized
403	Forbidden
404	Resource not found
409	Duplicate resource
500	Internal server error
12. Git and GitHub Configuration

The project uses Git for version control.

Important Git Ignore Rules

The following files and directories should not be committed:

text
.env
.env.*
node_modules/
logs/
*.log
coverage/
dist/
build/


The .env.example file should be committed so other developers know which environment variables are required.

Git Commands

Initialize Git:

bash
git init


Check status:

bash
git status


Stage files:

bash
git add .


Commit changes:

bash
git commit -m "Initial NoteCraft backend setup"


Connect GitHub repository:

bash
git remote add origin YOUR_GITHUB_REPOSITORY_URL


Push code:

bash
git branch -M main
git push -u origin main

13. Future Improvements

Planned improvements for NoteCraft:

Complete note controller and APIs.
Rich text editor integration.
Advanced note search.
Task reminders and notifications.
Email verification.
Password reset using OTP.
Note locking and privacy.
Dashboard analytics.
File and image attachments.
Soft deletion and restore.
API rate limiting.
Automated backend testing.
API documentation using Swagger.
Deployment using a cloud hosting service.
14. Development Status
Module	Status
Express server setup	In progress
MongoDB connection	In progress
User model	Planned/verify implementation
Authentication controller	In progress
Authentication middleware	In progress
Notebook model and controller	In progress
Tag model and controller	Implemented draft
Task model and controller	Implemented draft
Error middleware	Implemented draft
Validation middleware	Implemented draft
Note controller	Pending
Postman testing	Pending
Frontend integration	Pending

Update these statuses as you complete and test each module.

15. Project Author

Project: NoteCraft

Technology: MERN Stack

Backend: Node.js, Express.js, MongoDB, Mongoose

API Style: RESTful APIs

Authentication: JWT

Database: MongoDB

16. Developer Notes

This backend follows a modular architecture that separates:

Models – Database structure.
Controllers – Business logic.
Routes – API endpoints.
Middleware – Authentication, validation and errors.
Services – External integrations.
Utilities – Reusable helper functions.

The objective is to maintain clean, reusable, secure and scalable backend code.

NoteCraft – Organize Your Thoughts, Manage Your Tasks, Boost Your Productivity.