# Add a custom Edit page for Contacts

This guide walks you through adding a custom edit page for Contacts that matches the styling and patterns used by your other edit pages (Cats, Kittens, Litters). It also includes tests and quick manual checks after each step.

Project context and conventions used below are based on your repository:

- Contact model: main/models.py (class Contact)
- Contact form: main/forms.py (ContactForm)
- Contacts listing: main/contact_views.py (manage_contacts) + main/templates/main/manage_contacts.html
- Existing edit patterns: edit_cat, edit_kitten, edit_litter (views, URLs, templates)
- App is mounted under /main/ (see breeder_project/urls.py)

Outcome: The edit icon on the Contacts page will navigate to a new custom page at /main/contacts//edit/ instead of Django Admin.

## Overview of changes

New files to create:

- main/templates/main/edit_contact.html

Existing files to edit:

- main/urls.py (add route)
- main/contact_views.py (add view)
- main/templates/main/manage_contacts.html (update edit icon link)
- main/tests.py (add tests)

Optional (not required):

- main/forms.py (add ContactEditForm if you prefer symmetry with other EditForm classes)

## Step 1: Add the URL route

Edit: main/urls.py

Add the route next to your other contacts routes:

```python
from django.urls import path
from . import contact_views

urlpatterns = [
    # ... existing routes ...
    path('contacts/', contact_views.manage_contacts, name='manage_contacts'),
    path('contacts/add/', contact_views.add_contact, name='add_contact'),
    # New edit route for contacts
    path('contacts/<int:contact_id>/edit/', contact_views.edit_contact, name='edit_contact'),
]
```

Quick test (manual):

- Start the server: python manage.py runserver
- Visit a placeholder URL, e.g., [http://localhost:8000/main/contacts/1/edit/](http://localhost:8000/main/contacts/1/edit/) (will 404 until the view/template exist). This confirms routing is wired.

## Step 2: Implement the view

Edit: main/contact_views.py

- Import get_object_or_404 (to be consistent with other edit views).
- Implement edit_contact similar to edit_cat/edit_kitten patterns.

```python
from django.shortcuts import render, redirect, get_object_or_404
from django.contrib.auth.decorators import login_required
from django.contrib import messages
from .models import Contact
from .forms import ContactForm  # You can reuse ContactForm for editing
from .utils import get_breeder_profile_and_logo

@login_required
def edit_contact(request, contact_id):
    breeder_profile, logo_url, logo_exists = get_breeder_profile_and_logo()
    contact = get_object_or_404(Contact, pk=contact_id)

    if request.method == 'POST':
        form = ContactForm(request.POST, instance=contact)
        if form.is_valid():
            form.save()
            messages.success(request, f"Contact '{contact.name}' updated successfully!")
            return redirect('manage_contacts')
    else:
        form = ContactForm(instance=contact)

    context = {
        'breeder_profile': breeder_profile,
        'logo_url': logo_url,
        'logo_exists': logo_exists,
        'contact': contact,
        'form': form,
    }
    return render(request, 'main/edit_contact.html', context)
```

Notes:

- Reusing ContactForm is fine; your other edit pages use distinct EditForm classes, but the fields and widgets for Contact are identical, so this keeps it simple.

Quick test (manual):

- If the template (next step) doesn’t exist yet, the view will raise a template error when you hit the route. That’s expected for now.

## Step 3: Create the template (match existing styling)

Create: main/templates/main/edit_contact.html

Base this on your existing edit_cat.html / edit_kitten.html structure. Example:

```html
{% extends "main/base.html" %}
{% block title %}
    Edit Contact - {{ contact.name|default:'Contact' }}
{% endblock %}

{% block content %}
<div class="container-fluid">
    <div class="row mb-4">
        <div class="col-12">
            <h1 class="mb-4">Edit Contact - {{ contact.name|default:'Contact' }}</h1>
        </div>
    </div>
</div>

<form method="post">
    {% csrf_token %}

    <div class="card mb-0">
        <div class="card-header d-flex justify-content-between align-items-center">
            <h5 class="mb-0"><i class="fas fa-pen-to-square me-2"></i>Details</h5>
        </div>
        <div class="card-body" style="background-color: #e0dede">
            <div class="row g-3">
                <div class="col-md-6">
                    <label for="id_name" class="form-label">Name</label>
                    {{ form.name }}
                </div>
                <div class="col-md-6">
                    <label for="id_email" class="form-label">Email</label>
                    {{ form.email }}
                </div>
                <div class="col-md-6">
                    <label for="id_phone" class="form-label">Phone</label>
                    {{ form.phone }}
                </div>
                <div class="col-12">
                    <label for="id_address" class="form-label">Address</label>
                    {{ form.address }}
                </div>
                <div class="col-12">
                    <label for="id_notes" class="form-label">Notes</label>
                    {{ form.notes }}
                </div>
                <div class="col-12">
                    <button type="submit" class="btn btn-primary"><i class="fas fa-floppy-disk me-1"></i> Save Changes</button>
                    <a href="{% url 'manage_contacts' %}" class="btn btn-secondary">Cancel</a>
                </div>
            </div>
        </div>
    </div>
</form>

{% endblock %}
```

Quick test (manual):

- Visit [http://localhost:8000/main/contacts/](http://localhost:8000/main/contacts/)/edit/ while logged in.
- You should see the edit form with the same styling as other edit pages. Submitting valid data should redirect back to the contacts listing with a success message.

## Step 4: Point the edit icon to the new page

Edit: main/templates/main/manage_contacts.html

Replace the current Admin edit link:

```django
<a href="{% url 'admin:main_contact_change' contact.id %}" class="btn btn-sm btn-primary"><i class="fas fa-edit"></i></a>
```

With your new route:

```django
<a href="{% url 'edit_contact' contact.id %}" class="btn btn-sm btn-primary" title="Edit Contact"><i class="fas fa-edit"></i></a>
```

Quick test (manual):

- Visit [http://localhost:8000/main/contacts/](http://localhost:8000/main/contacts/)
- Click the edit icon; it should take you to [http://localhost:8000/main/contacts/](http://localhost:8000/main/contacts/)/edit/

## Step 5: Add tests

Edit: main/tests.py

Add tests alongside your existing AdminViewsTests and litter edit tests.

```python
from django.test import TestCase, Client
from django.urls import reverse
from django.utils import timezone
from datetime import date, timedelta
from main.models import Contact

class ContactEditTests(TestCase):
    def setUp(self):
        from django.contrib.auth.models import User
        self.client = Client()
        self.user = User.objects.create_user(username='testuser', password='testpassword')
        self.client.login(username='testuser', password='testpassword')

        # Ensure base context can render (BreederProfile is used in header)
        from main.models import BreederProfile
        BreederProfile.objects.create(cattery_name="Test Cattery", address="123 Test St")

        self.contact = Contact.objects.create(
            name='Original Name',
            email='orig@example.com',
            phone='0123456789',
            address='1 Test St',
            notes='Original notes',
        )

    def test_edit_contact_get(self):
        url = reverse('edit_contact', args=[self.contact.id])
        resp = self.client.get(url)
        self.assertEqual(resp.status_code, 200)
        self.assertContains(resp, 'Edit Contact')
        self.assertContains(resp, self.contact.name)

    def test_edit_contact_post_updates_fields(self):
        url = reverse('edit_contact', args=[self.contact.id])
        resp = self.client.post(url, {
            'name': 'Updated Name',
            'email': 'updated@example.com',
            'phone': '0987654321',
            'address': '2 New St',
            'notes': 'Updated notes',
        })
        self.assertEqual(resp.status_code, 302)
        self.contact.refresh_from_db()
        self.assertEqual(self.contact.name, 'Updated Name')
        self.assertEqual(self.contact.email, 'updated@example.com')
        self.assertEqual(self.contact.phone, '0987654321')
        self.assertEqual(self.contact.address, '2 New St')
        self.assertEqual(self.contact.notes, 'Updated notes')

    def test_edit_contact_requires_login(self):
        self.client.logout()
        url = reverse('edit_contact', args=[self.contact.id])
        resp = self.client.get(url)
        self.assertEqual(resp.status_code, 302)
        self.assertIn('/accounts/login', resp['Location'])

class ManageContactsLinkTests(TestCase):
    def setUp(self):
        from django.contrib.auth.models import User
        self.client = Client()
        self.user = User.objects.create_user(username='testuser', password='testpassword')
        self.client.login(username='testuser', password='testpassword')
        from main.models import BreederProfile
        BreederProfile.objects.create(cattery_name="Test Cattery", address="123 Test St")
        self.contact = Contact.objects.create(name='Link Test')

    def test_manage_contacts_uses_custom_edit_link(self):
        list_url = reverse('manage_contacts')
        resp = self.client.get(list_url)
        self.assertEqual(resp.status_code, 200)
        # Ensure the edit link targets the new route, not admin
        edit_url = reverse('edit_contact', args=[self.contact.id])
        self.assertContains(resp, edit_url)
        self.assertNotContains(resp, 'admin:main_contact_change')
```

Run tests:

- python manage.py test

## Step 6: Optional tidy-up

- Forms symmetry: If you prefer mirroring your other edit pages, you can introduce a ContactEditForm in main/forms.py duplicating the Meta from ContactForm and switch the view to use ContactEditForm.
- Delete flow: If you also want a custom delete experience (instead of Admin delete), add a delete view, route, and a small confirm template. Mirror your litter delete pattern if you introduce one for contacts.

## Troubleshooting

- 404 on /main/contacts//edit/: Ensure the new URL is in main/urls.py and you’re logged in.
- Template not found: Confirm the file path is main/templates/main/edit_contact.html and template name matches in the view.
- CSRF error posting the form: Ensure {% csrf_token %} is present inside the form.
- Missing Breeder Profile error: Some pages rely on BreederProfile for header/logo context; create one in Admin or via a fixture for tests.

## Quick checklist

- [ ]  Route added: main/urls.py
- [ ]  View added: edit_contact in main/contact_views.py
- [ ]  Template created: main/templates/main/edit_contact.html
- [ ]  Edit icon updated: main/templates/main/manage_contacts.html
- [ ]  Tests added: main/tests.py
- [ ]  Tests passing: python manage.py test

You now have a custom Contacts edit page consistent with the rest of your site’s UI and patterns.

## Running the tests

From the project root (where manage.py lives):
- Run the entire test suite

```bash
python manage.py test
```

- Run only tests in the main app

```bash
python manage.py test main
```

- Run only the new Contact edit tests
  
```bash
python manage.py test main.tests.ContactEditTests
python manage.py test main.tests.ManageContactsLinkTests
```

- Run a single test method

```bash
python manage.py test main.tests.ContactEditTests.test_edit_contact_post_updates_fields
```

- Useful flags
- More verbose output

```bash
python manage.py test -v 2
```

- Keep the test database between runs (speeds up repeated runs)

```bash
python manage.py test --keepdb
```

- Optional: test coverage (if coverage is installed)

```bash
coverage run manage.py test
coverage report -m
```

