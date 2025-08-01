# Setting Up a New Django Project

This guide outlines the basic steps to set up a new Django project from scratch.

## 1. Create a Python Virtual Environment

A virtual environment is a self-contained directory that holds a specific Python interpreter and its own set of installed packages. This is a best practice to keep project dependencies isolated.

### For macOS and Linux:

Open your terminal and run the following commands:

```bash
# Navigate to your project folder
mkdir my_new_django_project
cd my_new_django_project

# Create a virtual environment named 'venv'
python3 -m venv venv

# Activate the virtual environment
source venv/bin/activate
```

You'll know the environment is active because your shell prompt will be prefixed with `(venv)`.

### For Windows:

Open Command Prompt or PowerShell and run:

```bash
# Navigate to your project folder
mkdir my_new_django_project
cd my_new_django_project

# Create a virtual environment named 'venv'
python -m venv venv

# Activate the virtual environment
.\venv\Scripts\activate
```

## 2. Install Django

With your virtual environment active, you can now install Django using `pip`, the Python package installer.

```bash
# Make sure pip is up-to-date
pip install --upgrade pip

# Install Django
pip install django
```

You can verify the installation by running:
```bash
django-admin --version
```

## 3. Start a New Django Project

Now that Django is installed, you can create the basic project structure.

```bash
# Create a new project named 'myproject' in the current directory
django-admin startproject myproject .
```

The `.` at the end is important - it prevents Django from creating an extra subdirectory.

After running this, your directory will look like this:
```
.
├── manage.py
├── myproject/
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── venv/
    └── ...
```

## 4. Run Initial Setup

You can now test your new Django project and set up the initial database.

```bash
# Apply the initial database migrations
python manage.py migrate

# Start the development server
python manage.py runserver
```

Open your web browser and navigate to `http://127.0.0.1:8000/`. You should see the default Django welcome page.

## 5. Create a Superuser

A superuser account has all permissions and can access the Django admin site.

```bash
# Create a superuser account
python manage.py createsuperuser
```

You will be prompted to enter a username, email address, and password. Once you have created the superuser, you can start the development server again (`python manage.py runserver`) and log in to the admin site at `http://127.0.0.1:8000/admin`.

Congratulations! You have successfully set up a new Django project.

