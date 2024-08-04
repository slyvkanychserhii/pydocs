
https://docs.djangoproject.com/en/5.0/ref/models/options/

# Model Meta options

- **abstract** - Если True - модель будет [абстрактным](https://docs.djangoproject.com/en/5.0/topics/db/models/#abstract-base-classes) базовым классом abstract = True
- **db_table** - Имя таблицы базы данных, используемой для модели
- **ordering** - Порядок объекта по умолчанию, используемый при получении списков объектов
```python
ordering = ["pub_date"]
ordering = ["-pub_date"]
ordering = ["-pub_date", "author"]
```
```python
from django.db import models

class CommonInfo(models.Model):
    name = models.CharField(max_length=100)
    class Meta:
        abstract = True
        ordering = ["name"]

class Student(CommonInfo):
    home_group = models.CharField(max_length=5)
    class Meta(CommonInfo.Meta):
        db_table = "student_info"
```
- **proxy** Если True - модель, которая является подклассом другой модели ([прокси-модель](https://docs.djangoproject.com/en/5.0/topics/db/models/#proxy-models))
```python
from django.db import models

class Person(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)

class MyPerson(Person):
    class Meta:
        proxy = True

    def do_something(self):
        pass

class OrderedPerson(Person):
    class Meta:
        ordering = ["last_name"]
        proxy = True
```
- **verbose_name** - Удобочитаемое название объекта, единственное число
```python
verbose_name = "pizza"
```
- **verbose_name_plural** - Множественное имя объекта
```python
verbose_name_plural = "stories"
```
еще
```python
class Ox(models.Model):
    horn_length = models.IntegerField()

    class Meta:
        ordering = ["horn_length"]
        verbose_name_plural = "oxen"
```
