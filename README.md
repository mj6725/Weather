# in the terminal

# Create a virtual environment and install Django
python3 -m venv venv

#if you are using mac use next command as :
source venv/bin/activate

# REMEMBER if you are using windows
venv\Scripts\activate

pip install django

# Create a Django project
django-admin startproject weather

# cd into the weather directory
cd weather_app

# Create the weather app
python manage.py startapp weather

# Add the weather app to the installed apps in settings.py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'weather',
    'weather_app',
]

# Add the OpenWeatherMap API key to settings.py
OPENWEATHERMAP_API_KEY = 'YOUR_API_KEY_HERE'
Create the necessary views in the weather/views.py file:
from django.shortcuts import render
from django.http import request,HttpResponse
import requests

# Create your views here.
def home(request):
    if request.method == 'POST':
        city = request.POST.get('city')
        url = f'http://api.openweathermap.org/data/2.5/weather?q={city}&amp;appid=YOUR_API_KEY'
        response = requests.get(url)
        data = dict(response.json())
        return render(request,'weather.html',{
            'city':data['name'],
            'main':data['weather'][0]['main'],
            'temp': data['main']['temp'],
            'max':data['main']['temp_max'],
            'min':data['main']['temp_min'],
            'feels':data['main']['feels_like'],
            })
    return render(request,'index.html',{})
    <!--Create the HTML template in the
weather/templates/weather/index.html
file:-->





    <title>Weather App</title>



        {% csrf_token %}
        <label for="city">Your City Name</label>





<!--Create the HTML template in the
weather/templates/weather/weather.html
file:-->





    <title>Weather at finger tips</title>


    <h1>{{city}}</h1>
    <h3>Today we have some {{main}} in weahter</h3>
    <h3>Temprature: {{temp}} <br>Feels like: {{feels}}</h3>
    #Update the urls.py file in the project's root directory:
from django.urls import path
from app import views

urlpatterns = [
    path('',views.home),
]

# Make migrations for the weather app
python manage.py makemigrations weather

# Migrate the changes to the database
python manage.py migrate
# Now in the terminal again

# Run the server
python manage.py runserver

# Open the app in a web browser
http://localhost:8000
