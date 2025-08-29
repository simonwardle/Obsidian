
# Here is a road-map of the steps taken to build the new re-homing page:

   1. Create the URL: We'll define a new URL path (e.g., /rehome/) that will be the address for our new page.
   2. Create a Simple View: We'll create a basic "view" function that sends a simple "Hello, Rehoming Page!" message to the browser. This will  confirm the URL and view are connected correctly.
   3. Create the Template: We'll create the actual HTML file for the page and make the view render it. This will give us a visual page to look at.
   4. Add Data to the View: We'll modify the view to fetch a specific kitten's data from the database and pass it to the template.
   5. Display Data in the Template: We'll update the HTML template to display the kitten's information.
   6. Add a Form: We'll create and display a form for entering the rehoming details.
   7. Process the Form: We'll add the logic to the view to handle the form submission and update the kitten's status in the database.

### Step 1: Create the URL

  First, we need to tell Django that a new page exists at a specific URL. We do this in the urls.py file. For now, we'll point it to a view function that doesn't exist yet. That's okay! We'll create it in the next step.

  1.  path('rehome/<int:kitten_id>/', kitten_views.rehome_kitten, name='rehome_kitten'),

  Explanation:

   * `path(...)`: This function defines a new URL pattern.
   * `'rehome/<int:kitten_id>/'`: This is the URL structure.
	   * It will match URLs like /rehome/1/, /rehome/23/, etc.
       * The <int:kitten_id> part is important. It tells Django to expect an integer in that part of the URL and to capture its value into variable named kitten_id.
   * `kitten_views.rehome_kitten`: This is the view function that will handle requests to this URL. We haven't created it yet, but we're telling Django that it will be located in the kitten_views.py file.
   * `name='rehome_kitten'`: This gives the URL a unique name. Naming URLs is a best practice in Django. It allows you to refer to this URL easily in other parts of the code (like in templates) without having to hardcode the path.

  Now, if you were to run the development server and try to go to /rehome/1/, Django would give you an error because we haven't created the  rehome_kitten function yet.

### Step 2: Create a Simple View

  Now we'll create the rehome_kitten view function in main/kitten_views.py. For this step, we'll make it do the absolute minimum: return a  simple HTTP response. This is just to confirm that our URL from Step 1 is correctly wired up to this function.

  Here's the code I added:

   ```
   from django.http import HttpResponse
    
   @login_required
   def rehome_kitten(request, kitten_id):
       return HttpResponse(f"<h1>Rehoming page for kitten {kitten_id}</h1>")
```

  Explanation:

   * `from django.http import HttpResponse`: We import the HttpResponse class, which is the simplest way to send a response back to the user's
     browser.
   * `@login_required`: Just like our other views, we want to make sure only logged-in users can access this page.
   * `def rehome_kitten(request, kitten_id):`: This defines our view function.
       * It takes the standard request object.
       * Crucially, it also takes kitten_id as an argument. This is the value that Django captured from the URL (e.g., the 1 from /rehome/1/).
   * `return HttpResponse(...)`: This creates a response object containing the simple HTML string we provided. The f before the string makes it an "f-string," which allows us to easily embed the value of the kitten_id variable directly into the text.

  #### How to Test This Step

   1. Make sure your Django development server is running (python manage.py runserver).
   2. Open your web browser and go to the URL http://127.0.0.1:8000/main/rehome/1/.
   3. You should see a page with the text: "Rehoming page for kitten 1".
   4. Try changing the number in the URL (e.g., to /rehome/42/) and you should see the number on the page update.

  This confirms that our URL is correctly linked to our view function and that the kitten_id is being passed correctly.

### Step 3: Create a Simple HTML Template

Okay, the base.html template has a {% block content %} section. This is where we'll inject the specific content for our rehoming page.

  Now, I will create a new file called rehome_kitten.html in the main/templates/main/ directory.

  Here is the content for the new file:

```
 {% extends "main/base.html" %}
     
     {% block title %}
         Rehome Kitten - {{ kitten.name|default:'Kitten' }}
     {% endblock %}
     
     {% block content %}
     <div class="container mt-4">
         <div class="card">
            <div class="card-header">
                <h2 class="mb-0">Rehome Kitten</h2>
            </div>
            <div class="card-body">
                <p>This is the rehoming page for kitten ID: <strong>{{ kitten_id }}</strong></p>
                <!-- More content will go here in the next steps -->
            </div>
        </div>
    </div>
    {% endblock %}

```


   Explanation:

   * `{% extends "main/base.html" %}`: This is the most important line. It tells Django to use base.html as the foundation and just replace the  blocks we define. This ensures our new page has the same navigation bar, footer, and general styling as the rest of the site.
   * `{% block title %}`: We're overriding the default title to be more specific for this page.
   * `{% block content %}`: All the HTML inside this block will be inserted into the content block of the base.html file.
   * `{{ kitten_id }}`: This is a Django template variable. In the next step, we will modify our view to pass the kitten_id to this template so  it can be displayed.
     
     The final part of this step is to modify our rehome_kitten view to actually use this new template instead of returning that simple  HttpResponse.

Here is the new code:

```
@login_required
def rehome_kitten(request, kitten_id):
     context = {
     'kitten_id': kitten_id,
     }
     return render(request, 'main/rehome_kitten.html', context)
```

  Explanation:

   * `render(...)`: This is a standard Django shortcut function. It takes the request object, the path to the template file, and a context dictionary.
   * `context`: This is a dictionary where the keys are the names of variables you want to use in your template, and the values are the data for those variables.
   * `'kitten_id': kitten_id`: We're creating a context dictionary with one item. The key, 'kitten_id', is the name we used in our template ({{ kitten_id }}). The value is the kitten_id that was passed into our view function from the URL.

  How to Test This Step

   1. Make sure your Django development server is running.
   2. Open your browser and go to http://127.0.0.1:8000/main/rehome/1/.

  Now, instead of the simple text from Step 2, you should see your full site layout (navigation bar, etc.), and the main content area should  show the "Rehome Kitten" card with the text "This is the rehoming page for kitten ID: 1".

  This confirms that your view is correctly rendering your new template and passing data to it.
  
   Now for the exciting part: let's make the page dynamic by pulling real data from your database.

### Step 4: Add Data to the View

  In this step, we'll modify the rehome_kitten view. Instead of just passing the kitten_id number to the template, we will use that ID to find
  the actual Kitten object in the database and pass the whole object.

  I'll also add the breeder profile and logo information, which is needed by your base.html template to render the navigation bar correctly.

 Here is the new code:

```
from django.shortcuts import render, redirect, get_object_or_404
# ... other imports
 
 @login_required
 def rehome_kitten(request, kitten_id):
     breeder_profile, logo_url, logo_exists = get_breeder_profile_and_logo()
     kitten = get_object_or_404(Kitten, pk=kitten_id)
     context = {
         'breeder_profile': breeder_profile,
         'logo_url': logo_url,
         'logo_exists': logo_exists,
         'kitten': kitten,
     }
     return render(request, 'main/rehome_kitten.html', context)
```


  Explanation of Changes:

   * `from django.shortcuts import ..., get_object_or_404`: I've added get_object_or_404 to the imports. This is a very common and useful Django
     shortcut.
   * `breeder_profile, logo_url, logo_exists = get_breeder_profile_and_logo()`: I added this line to fetch the necessary branding information
     that your base.html template requires for the navigation bar.
   * `kitten = get_object_or_404(Kitten, pk=kitten_id)`: This is the key line.
       * It attempts to get a Kitten object from the database.
       * pk=kitten_id tells it to find the kitten whose "primary key" (which is its unique ID) matches the kitten_id from the URL.
       * If it finds the kitten, it returns the Kitten object, which we store in the kitten variable.
       * If it cannot find a kitten with that ID, it will automatically stop and show a "404 Not Found" page, which is exactly what we want to happen if someone types in a URL for a kitten that doesn't exist.
   * `'kitten': kitten`: In the context dictionary, we are now passing the entire Kitten object to the template under the name kitten. We no longer need to pass the kitten_id separately, as it's a property of the kitten object.
     
How to Test This Step

  This step doesn't have a visual change on its own, but it's a crucial update to our view's logic.

   1. Refresh the page at http://127.0.0.1:8000/main/rehome/1/. It should still load correctly.
   2. Try a URL for a kitten that you know doesn't exist, like http://127.0.0.1:8000/main/rehome/9999/. You should now see a "Not Found" (404)  error page, which proves our get_object_or_404 is working correctly.

### Step 5: Display Data in the Template

  We will now edit the main/templates/main/rehome_kitten.html file to show details about the specific kitten we're rehoming. We can access the kitten's attributes (like name, sex, date_of_birth, etc.) directly in the template.

Here's a summary of the changes:

```
<div class="card-header">
  <h2 class="mb-0">Rehome Kitten: {{ kitten.name }}</h2>
</div>
<div class="card-body">
   <div class="row">
     <div class="col-md-6">
       <h5>Kitten Details</h5>
         <ul class="list-group">
           <li class="list-group-item"><strong>Name:</strong> {{ kitten.name }}             </li>
           <li class="list-group-item"><strong>Sex:</strong>{{kitten.get_sex_display}}</li>
           <li class="list-group-item"><strong>Date of Birth:</strong> {{ kitten.litter.date_of_birth }}</li>
           <li class="list-group-item"><strong>Status:</strong> 
           <span class="badge bg-primary">{{ kitten.status }}</span>
           </li>
         </ul>
     </div>
     <div class="col-md-6">
         <!-- The form for rehoming will go here in the next step -->
     </div>
  </div>
</div>
```
    
Explanation:

   * `{{ kitten.name }}`: Since our view passes the entire kitten object, we can now access its properties using dot notation. This will display the kitten's name.
   * `{{ kitten.get_sex_display }}`: For fields that have "choices" defined in the model (like sex), Django automatically provides a get_FIELD_display() method. Instead of showing the raw value from the database (e.g., "M" or "F"), this will show the more human-readable label (e.g., "Male" or "Female").
   * `{{ kitten.litter.date_of_birth }}`: Because the Kitten model has a relationship with the Litter model, we can "follow" that relationship in the template. This line gets the kitten's litter and then gets the date_of_birth from that litter.
   * `{{ kitten.status }}`: This displays the current status of the kitten.
   * Layout: I've used Bootstrap's grid system (row, col-md-6) to create a two-column layout. The kitten's details are on the left, and we've reserved the right-hand column for the rehoming form we'll add in the next step.

How to Test This Step

   1. Go to http://127.0.0.1:8000/main/rehome/1/ in your browser (or use the ID of any other existing kitten).
   2. You should now see the kitten's details (Name, Sex, Date of Birth, and Status) displayed in a structured list on the left side of the card.

### Step 6: Add a Form

  This is a three-part step:
   1. First, we need to define the fields of our form in forms.py.
   2. Then, we'll update the view in kitten_views.py to create an instance of the form and pass it to the template.
   3. Finally, we'll render the form in the rehome_kitten.html template.
#### Part 6.1: Define the Form

  I'll start by creating a new form class called RehomeKittenForm in the main/forms.py file. This form will have fields for the new owner and the sale price.
   I've added the RehomeKittenForm to main/forms.py.

  Here's the code I added:

```
class RehomeKittenForm(forms.ModelForm):
   class Meta:
      model = Rehoming
         fields = ['owner', 'price', 'deposit_amount', 'deposit_taken_date',                        'collection_datetime', 'pedigree_status']
         widgets = {
           'owner': forms.Select(attrs={'class': 'form-select'}),
           'price': forms.NumberInput(attrs={'class': 'form-control'}),                     'deposit_amount': forms.NumberInput(attrs={'class': 'form-control'}),
           'deposit_taken_date': forms.DateInput(attrs={'class': 'form-control               flatpickr-date', 'type': 'date'}),
           'collection_datetime': forms.DateTimeInput(attrs={'class': 'form-                 control flatpickr-datetime', 'type': 'datetime-local'}),
           'pedigree_status': forms.Select(attrs={'class': 'form-select'}),
         }
 
     def __init__(self, *args, **kwargs):
         super().__init__(*args, **kwargs)
         self.fields['owner'].queryset = Contact.objects.all()
         self.fields['owner'].empty_label = "Select a Contact"
```

  Explanation:

   * `class RehomeKittenForm(forms.ModelForm):`: We're creating a ModelForm, which is a special type of form that automatically builds itself from a Django model.
   * `class Meta:`: This inner class tells the ModelForm which model to use and which fields to include.
   * `model = Rehoming`: We're basing this form on our Rehoming model.
   * `fields = [...]`: We specify the list of fields from the Rehoming model that we want to include in our form.
   * `widgets = {...}`: This dictionary allows us to customize the HTML input element for each field. I've added the form-control and form-select classes to make the form elements look good with Bootstrap. I've also added the flatpickr-date class to the date fields to enable the date picker.
   * `__init__` method: This method is called when the form is created. We're using it to customize the owner field.
       * self.fields['owner'].queryset = Contact.objects.all(): This populates the "owner" dropdown with a list of all the contacts from your database.
       * self.fields['owner'].empty_label = "Select a Contact": This sets the default, non-selectable option in the dropdown.

  Now that the form is defined, let's move on to the next part of this step.

#### Part 6.2: Update the View
I've updated the rehome_kitten view in main/kitten_views.py.

  Here's the new code:

```
from .forms import KittenForm, RehomeKittenForm
# ...
@login_required
    def rehome_kitten(request, kitten_id):
        # ... (code to get kitten and profile)
        form = RehomeKittenForm()
        context = {
            # ... (other context variables)
             'form': form,
        }
        return render(request, 'main/rehome_kitten.html', context)
```
    

  Explanation of Changes:

   * `from .forms import KittenForm, RehomeKittenForm`: I imported our new RehomeKittenForm.
   * `form = RehomeKittenForm()`: I created an instance of the form.
   * `'form': form`: I added the form instance to the context dictionary so we can access it in the template.

  Now for the final part of this step.

  Part 6.3: Render the Form in the Template
I've updated the rehome_kitten.html template.

  Here's the code I added:

   1 <div class="col-md-6">
   2     <h5>Rehoming Details</h5>
   3     <form method="post">
   4         {% csrf_token %}
   5         {{ form.as_p }}
   6         <button type="submit" class="btn btn-primary">Rehome Kitten</button>
   7     </form>
   8 </div>

  Explanation:

   * `<form method="post">`: This starts the form. method="post" is important because it means the form data will be sent in the body of the
     request, which is the standard and secure way to submit data that will modify the server.
   * `{% csrf_token %}`: This is a crucial Django security feature. It protects against Cross-Site Request Forgery attacks. You must include
     this tag in any POST form.
   * `{{ form.as_p }}`: This is a magical Django template instruction. It takes the form object we passed from the view and renders each of its
     fields (the owner dropdown, the price input, etc.) wrapped in a paragraph (<p>) tag. It's a quick way to get the form displayed without
     having to manually write the HTML for each field.
   * `<button type="submit" ...>`: This is the button the user will click to submit the form.

  How to Test This Step

   1. Go to http://127.0.0.1:8000/main/rehome/1/ in your browser.
   2. You should now see the "Rehoming Details" form on the right-hand side of the page. It should have a dropdown for the owner, input boxes for
      the price and deposit, date pickers, and a "Rehome Kitten" button.

  At this point, the form is displayed, but it doesn't do anything yet. If you fill it out and click the button, the page will just reload.

Step 7: Process the Form

  This is where we add the logic to the rehome_kitten view to handle the data when the user clicks the "Rehome Kitten" button.

  Here's what we'll do:
   1. Check if the request is a POST request (meaning the form was submitted).
   2. If it is, we'll validate the submitted data.
   3. If the data is valid, we'll create a new Rehoming record, link it to the kitten, update the kitten's status, show a success message, and
      redirect the user.
   4. If the data is invalid, we'll show the form again with error messages.

I've updated the rehome_kitten view with the final logic.

  Here is the new code:

    1 @login_required
    2 def rehome_kitten(request, kitten_id):
    3     breeder_profile, logo_url, logo_exists = get_breeder_profile_and_logo()
    4     kitten = get_object_or_404(Kitten, pk=kitten_id)
    5 
    6     if request.method == 'POST':
    7         form = RehomeKittenForm(request.POST)
    8         if form.is_valid():
    9             rehoming = form.save(commit=False)
   10             rehoming.kitten = kitten
   11             rehoming.save()
   12 
   13             # Update kitten status
   14             sold_status = KittenStatus.objects.get(name='Sold')
   15             kitten.status = sold_status
   16             kitten.save()
   17 
   18             messages.success(request, f"{kitten.name} has been successfully rehomed to {rehoming.owner.name}.")
   19             return redirect('manage_kittens')
   20     else:
   21         form = RehomeKittenForm()
   22 
   23     context = {
   24         'breeder_profile': breeder_profile,
   25         'logo_url': logo_url,
   26         'logo_exists': logo_exists,
   27         'kitten': kitten,
   28         'form': form,
   29     }
   30     return render(request, 'main/rehome_kitten.html', context)
   
   Explanation:

   * `if request.method == 'POST':`: This block of code only runs when the form is submitted. If the user is just visiting the page (a GET
     request), this block is skipped, and a new, empty form is created at the end.
   * `form = RehomeKittenForm(request.POST)`: We create a form instance and populate it with the data the user submitted (request.POST).
   * `if form.is_valid():`: Django checks the submitted data against the rules of the form (e.g., are the numbers actual numbers? Is a required
     field filled out?).
   * `rehoming = form.save(commit=False)`: This is a clever ModelForm trick. form.save() would normally save the Rehoming object directly to the
     database. However, we need to set the kitten for this rehoming record first. commit=False creates the Rehoming object in memory without
     saving it to the database yet.
   * `rehoming.kitten = kitten`: We manually set the kitten field on our new rehoming object to be the kitten we loaded from the URL.
   * `rehoming.save()`: Now that the kitten is set, we save the complete Rehoming object to the database.
   * `sold_status = KittenStatus.objects.get(name='Sold')`: We fetch the KittenStatus object for "Sold" from the database.
   * `kitten.status = sold_status`: We update the status of our kitten to this new status.
   * `kitten.save()`: We save the changes to the kitten.
   * `messages.success(...)`: We create a success message that will be displayed on the next page.
   * `return redirect('manage_kittens')`: We redirect the user back to the main kitten management page, where they will see the success message
     and the updated status of the kitten.

  How to Test This Final Step

   1. Go to the rehoming page for a kitten that is not yet sold (e.g., http://127.0.0.1:8000/main/rehome/1/).
   2. Fill out the "Rehoming Details" form. Select a contact from the dropdown, enter a price, and choose the dates.
   3. Click the "Rehome Kitten" button.
   4. You should be redirected to the "Manage Kittens" page.
   5. You should see a green success message at the top of the page confirming the rehoming.
   6. Find the kitten in the list. Its status should now be "Sold".

  Congratulations! You have successfully built a new, fully functional page in Django from scratch, following a structured, step-by-step
  process.


