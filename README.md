# Django-DRF-REACT/Image-upload-ToBackend

Integrating Django Rest Framework (DRF) with React for image upload involves setting up the backend API with DRF to handle file uploads and creating a React frontend to interact with it. Below are step-by-step instructions along with sample code snippets.

## Backend (Django Rest Framework):
### 1. Create Django Project and App:

```bash
django-admin startproject myproject
cd myproject
python manage.py startapp myapp
```
### 2. Install Required Packages:
```bash
pip install djangorestframework
```
### 3. Configure settings.py:
Add 'rest_framework' and your app to INSTALLED_APPS.
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'api',
    'rest_framework',
]
```
### 4. Create Model for Image:
In models.py of your app:
```python
from django.db import models

# Create your models here.
class Image(models.Model):
    image = models.ImageField(upload_to='images/')
```
### 5. Create Serializer:
In serializers.py of your app:
```python
from rest_framework import serializers
from .models import Image

class ImageSerializer(serializers.ModelSerializer):
    class Meta:
        model = Image
        fields = '__all__'
```
### 6. Create Views:
In views.py of your app:
```python
from django.shortcuts import render

# Create your views here.
from rest_framework import viewsets
from .models import Image
from .serializers import ImageSerializer

class ImageViewSet(viewsets.ModelViewSet):
    queryset = Image.objects.all()
    serializer_class = ImageSerializer
```

### 7. Configure URLs:
In urls.py of your app:
```python
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from .views import ImageViewSet

router = DefaultRouter()
router.register(r'images', ImageViewSet)

urlpatterns = [
    path('', include(router.urls)),
]
```

### 8. Configure urls.py in the project folder:
```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/', include('api.urls')),
]
```

### 9. Run Migrations:
```bash
python manage.py makemigrations
python manage.py migrate
```

## Frontend (React):
### 1. Create React App:
```bash
npx create-react-app myreactapp
cd myreactapp
```

### 2. Install Axios (for API requests):
```bash
npm install axios
```

### 3. Create a Component for Image Upload:
In src/components/ImageUploadForm.js:
```React
import React, { useState } from 'react';
import axios from 'axios';

const ImageUpload = () => {
    const [selectedFile, setSelectedFile] = useState(null);

    const handleFileChange = (event) => {
        setSelectedFile(event.target.files[0]);
    };

    const handleUpload = () => {
        const formData = new FormData();
        formData.append('file', selectedFile);

        axios.post('http://localhost:8000/api/images/', formData)
            .then(response => {
                console.log('Image uploaded successfully:', response.data);
            })
            .catch(error => {
                console.error('Error uploading image:', error);
            });
    };

    return (
        <div>
            <input type="file" onChange={handleFileChange} />
            <button onClick={handleUpload}>Upload Image</button>
        </div>
    );
};

export default ImageUpload;
```
### 4. Use the Component in src/App.js:
```React
import React from 'react';
import ImageUpload from './components/ImageUpload';

function App() {
    return (
        <div>
            <h1>Image Upload App</h1>
            <ImageUpload />
        </div>
    );
}

export default App;
```
### 5. Run the React App:
```bash
npm start
```

* Now, you should have a basic setup where you can upload an image from your React frontend to your Django backend using Django Rest Framework. Adjust the code and configurations as needed for your specific requirements.

## For more details:
1. [Djnago](https://docs.djangoproject.com/en/4.2/)
2. [Django-rest-framework](https://www.django-rest-framework.org/)
3. [React](https://react.dev/learn)
