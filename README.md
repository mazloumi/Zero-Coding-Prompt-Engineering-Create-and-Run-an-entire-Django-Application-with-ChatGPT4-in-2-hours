
# Zero-Coding-Prompt-Engineering-Create-and-Run-an-entire-Django-Application-with-ChatGPT4-in-2-hours
Below are all the prompts used to create the web application as described in the Medium article: https://medium.com/@nima.mazloumi/zero-coding-through-prompt-engineering-create-and-run-an-entire-django-application-with-chatgpt4-5f70d0dc0f0f

## DALL-E Prompt
Draw a conveyor belt that comes from the top left and pours words and phrases into a golden hourglass with glowing blue liquid. On the bottom of the hourglass another conveyor belt moves to the right bottom of the screen carrying code, files, scripts, SQL statements, etc.. The illustration is warm, radiant, magical.

## Initial Setup
### Creating the Django project

I want to create a Django web application. Provide the command that creates the Django project called  conference_app and the command to create the "conference" application within the project.

### Django Folder Structure

Show me for the folder structure that Django would create.

### Creating the Application Container with Docker

Create a Dockerfile for a Python project that creates a working directory /app,  copies the requirements.txt inside the /app/ folder,  upgrades pip installs all required packages, and then copies all the local files to the working directory.  

No need to expose any ports or define environment variables.  
Just add the command tail -f /dev/null to keep the container alive after startup.

### Defining The Application Stack With Docker Compose

Now provide for a docker-compose.yml file that creates a PostgreSQL database container and a web container that uses the above Dockerfile to deploy the project in Docker.  For the PostgreSQL parameters use the following:   
  
- DB: conf_db   
- User: conf_user   
- Password: Qwer!@34   
  
Bind the database port externally to port 6432.  Name the database container db_cont.  For the web application don't provide a command.  Instead, we will use the Dockerfile to build the web container.  Also, don't provide any environment variables.  Bind the web container from 8080 to the external port 9090.  Do not provide a database URL for the web container.  We will do that later. Name the web container web_cont.  Make sure all containers are built for the linux/amd64 platform.

### Defining Environments

Provide a  .env file that defines all the below key environment variables  for the Django project:  
  
- DEBUG  
- ALLOWED_HOSTS  
- CSRF_TRUSTED_ORIGINS  
- DB_NAME, DB_USER, DB_PASSWORD, DB_HOST, DB_PORT  
- ADMIN_USER=admin  
- ADMIN_EMAIL=admin@django.com  
- ADMIN_PASSWORD=Qwer!@34  
- CORS_ALLOW_CREDENTIALS  
- CORS_ORIGIN_ALLOW_ALL   
- CORS_ALLOW_CREDENTIALS   
- CSRF_COOKIE_DOMAIN  
- CORS_ORIGIN_WHITELIST  
- SECRET_KEY - generate a secret for me  
  
Only generate the .env file. We get to the settings.py later.

### Specifying the Application Settings

Create the settings.py file. Use the dotenv package to load the .env file from the project folder.  Make sure you set the databases section.  Make sure all default Django apps as well as the above application,  drf_yasg, and rest_framework packages are installed. Make sure the cors settings are loaded into the settings.py. Provide the default middleware. Use the root url config that you defined in the Dockerfile above. Use /static/ as the static URL and provide a static root that   
uses the static URL as the path.  

Ignore for now pip install. We will take care of that later.

## Creating the Web Application
### Defining the Data Model

I have the following data model:  
  
- Person - A person has the required fields First Name, Last Name,   and optionally the fields Photo as image upload, Pronouns, Role, Organization, City, Country, Ask Me About, Passion,  Good At, Bio, Email, Phone, LinkedIn handle, X/Twitter handle,  Instagram handle, a URL.  When a Person is associated with a Conference the person becomes an Attendee to the Conference. One Person can attend zero or more Conferences.  
- Conference - A Conference has the required fields Name, Start Date, and End Date, and optionally a Description, a Map field as Image upload, Sessions (0 or many), Venues (0 or many), and Attendees (0 or many, from Person)  
- Venue - A Venue has the required Name, and optionally a Description, Conferences (0 or many, from Conference), Address, and Locations (0 or many, from Location)  
- Location - A Location has a Name, Description and Venue  
- Session - A Session belongs to a Conference, is associated with a Location, and has the required fields Name, Start Date Time, and End Date Time, and optionally a Description, Facilitators (0 or many, from Attendees),  Panelists (0 or many, from Attendees), and Talks (0 or many from Talk)  
- Talk - A Talk has the required field Name, and the optional fields Description, Speakers (0 or many, from Attendee), Participants (0 or many, from Attendee), and belongs to a Session  
- If a Person has a User in Django, then the User Id field of Person needs to be associable with the Auth User model of Django.  
  
Create the entire models.py file for this data model.   
  
Add ManyToManyFields or ForeignKeys for Attendees, Facilitators, Panelists, Speakers, and Participants where appropriate.  
  
Make sure that all models have a default function that returns a human-readable name using the Name field. For Person use the first name and last name as the name.   
  
Also, make sure that fields that require a file upload use a subfolder under the STATIC_ROOT folder.

### Generate Diagram

For the models Conference, Venue, Location, Session, Talk, and Person draw a diagram using Graphviz. No need to add the fields.  Provide a link to download the diagram as an image.

### Defining An Administrative User Interface

Based on the model create the entire admin.py file for the admin UI  and register the models. Ignore the urls.py. We take care of that later.

### Defining the Application Programming Interfaces (APIs)
#### Serializers

Based on the model create the entire serializers.py file. We will use that later for the rest framework.

#### ViewSets

Based on the data model create the entire views.py file that uses the models and serializers.   
Ignore for now the wiring with the urls.py.  We take care of that later.

### Defining URL Routes

Create the full urls.py file, Make sure it registers all the above view sets, and set the paths for the admin, API, swagger,   
and redoc. Also, add the static_url to the path based on the static_root we defined above.

### Package Dependencies

Create the full requirements.txt file for the above application. Make sure it includes the packages needed to load the .env file and support for upload of images in Django, cors headers, PostgreSQL, and Gunicorn.

## Sample Data
### Creating Test Data using SQL
#### DDL
Based on the model create a DDL called data.sql and create sample records. Use some creative fake labels for the various records you create.  
  
- Create 5 person records and for 2 of the people create a Django user. Make sure when creating the user to provide values for review fields such as first_name, last_name, is_superuser, is_staff, is_active, and date_joined.   
- Create 1 venue with 5 locations.  
- Create 1 conference and associate it with the venue.  
- Create 5 sessions, each in a different location.  
- Associate the above people with the conference, sessions, and talks.  

Make sure when you create the script you follow the naming convention of  Django using application name conference in the table names.

#### Management Command

Create a management command called run_script that I can use to load the above file into the database.

#### Cleanup DDL

Provide another SQL script cleanup.sql to drop all application tables from the PostgreSQL database. Check, whether the auth_user table exists, and if so truncate it.

### Creating Test Data using Django ORM
#### Create Command
Create a management command that uses the Django ORM to create sample data.  Use some creative fake labels for the various records you create.  
  
- Create 5 person records and for 2 of the people create a Django user. Make sure when creating the user to provide values for review fields such as first_name, last_name,  is_superuser, is_staff, is_active, and date_joined.   
- Create 1 venue with 5 locations.  
- Create 1 conference and associate it with the venue.  
- Create 5 sessions, each in a different location.  
- Associate the above people with the conference, sessions, and talks.

#### Delete Command

Also, create a management command that deletes all the records from the database.

### Creating a Django Admin User

Create a management command for the conference app called create_superuser that based on the environment file we created above creates a super user for the Django application.

## Build and Run the Application

### Running The Application

Create a shell script called run.sh for macOS that does the following:  
- Runs cleanup.sql using the run_script command  
- Runs Django manage makemigrations  
- Runs Django manage to migrate  
- Populate the database using the populate_sample_data command  
- Runs the Django management command we created called create_superuser to create the admin user  
- Run collectstatic  
- Runs Gunicorn for the WSGI application we defined above  
  
No need to set any environment variables or use the psql command.  The above is sufficient.

### Connecting to the Web Container
Provide a shell script called connect.sh that includes the docker command to connect to the shell of the docker container web_cont instance.

### Building the Application Stack

Create a shell script called build.sh that uses linux/amd64 as the platform for docker, brings down any running containers, prunes all the orphan systems, prunes all orphan volumes, runs the docker-compose, and then shows the logs following them.
