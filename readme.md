# Contact List Application

## Overview

This project is a contact list application built with [Django](https://www.djangoproject.com/) as the backend and [Ionic Vue](https://ionicframework.com/docs/vue/overview) for the frontend. The app provides a simple interface to manage contacts with CRUD operations: create, read, update, and delete.

## Features

- **CRUD Operations**: Add, edit, and delete contacts.
- **User Management**: Maintain a list of user contacts with their details.
- **Responsive Design**: Built with Ionic for a mobile-first approach.
- **API Integration**: Django handles the backend API requests.

## Technologies Used

- **Frontend**: 
  - [Ionic Vue](https://ionicframework.com/docs/vue/overview)
  - [Vue.js](https://vuejs.org/)
- **Backend**: 
  - [Django](https://www.djangoproject.com/)
- **Development Tools**:
  - [Visual Studio Code](https://code.visualstudio.com/)

## Setup and Installation

### Backend Setup (Django)

1. **Clone the Repository**:

2. **Navigate to the Backend Directory**:
   - `cd django_backend`

3. **Create a Virtual Environment**:
   - `python -m venv venv`

4. **Activate the Virtual Environment**:
   - On Windows: `venv\Scripts\activate`
   - On macOS/Linux: `source venv/bin/activate`

5. **Install Dependencies**:
   - `pip install -r requirements.txt`

6. **Apply Migrations**:
   - `python manage.py migrate`

7. **Run the Development Server**:
   - `python manage.py runserver`

### Frontend Setup (Ionic Vue)

1. **Navigate to the myApp Directory**:
   - `cd myApp`

2. **Install Dependencies**:
   - `npm install`

3. **Update Environment Configuration**:
   - Edit `src/environments/environment_dev.js` to include your Django API URL:
     ```javascript
     export const environment = {
       apiUrl: 'http://localhost:8000/api/'
     };
     ```

4. **Run the Ionic Development Server**:
   - `npm start`

5. **Build APK** (for Android):
   - `ionic cap build android`

## Folder Structure

- `django_backend/`: Contains the Django backend code.
  - `contactlist/`: Django project directory.
  - `users/`: Django app for user management.
  - `manage.py`: Django management script.

- `myapp/`: Contains the Ionic Vue frontend code.
  - `src/`: Source directory for Vue components.
  - `public/`: Public assets and index file.
  - `package.json`: NPM configuration file.

## API Endpoints

- **GET /api/users/**: Retrieve all users.
- **POST /api/users/**: Add a new user.
- **PUT /api/users/{id}/**: Update a user.
- **DELETE /api/users/{id}/**: Delete a user.

## Contributing

Feel free to contribute to this project by submitting pull requests or issues. Please ensure that your contributions adhere to the project's coding standards and include appropriate tests.

