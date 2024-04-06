 Код описывает процесс добавления поля для хранения фотографии продукта в модель Django и 
 
 создания формы для загрузки фотографии.

Изменение модели продукта:

В файле models.py  Django приложения добавляем поле для хранения фотографии продукта с помощью 

ImageField. В следующем примере добавляем поле photo:

from django.db import models

class Product(models.Model):
    name = models.CharField(max_length=100)
    description = models.TextField()
    price = models.DecimalField(max_digits=10, decimal_places=2)
    photo = models.ImageField(upload_to='product_photos/')

Создание формы Django:

В файле forms.py приложения Django определяем форму для загрузки фотографии продукта. Создаем 

класс формы ProductForm, который наследуется от forms.ModelForm и указываем модель Product и

 поля, которые должны отображаться в форме, включая новое поле photo.

from django import forms
from .models import Product

class ProductForm(forms.ModelForm):
    class Meta:
        model = Product
        fields = ['name', 'description', 'price', 'photo']

Обновление представления для отображения формы:

В представлении, которое обрабатывает создание или обновление продукта, используем новую форму

 для создания или обновления продукта. В следующем примере функция create_product обрабатывает
 
  POST-запросы для создания продукта, используя форму ProductForm, а также передает файлы через 
  
  request.FILES.

  from django.shortcuts import render, redirect
from .forms import ProductForm

def create_product(request):
    if request.method == 'POST':
        form = ProductForm(request.POST, request.FILES)
        if form.is_valid():
            form.save()
            return redirect('product_list')
    else:
        form = ProductForm()
    return render(request, 'create_product.html', {'form': form})

Обновление шаблона для отображения формы:

В шаблоне (create_product.html) добавим HTML-код для отображения формы. Обратим внимание 

на атрибут enctype="multipart/form-data", который необходим для корректной передачи файлов через

 форму.

 <form method="post" enctype="multipart/form-data">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Сохранить</button>
</form>

Теперь есть форма Django для загрузки фотографии продукта и настроены модель и представление для

 ее использования.


