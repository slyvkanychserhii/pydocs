
https://docs.djangoproject.com/en/5.0/ref/models/fields/#module-django.db.models.fields.related
* **ForeignKey** - Отношение «многие к одному». Требует двух позиционных аргументов: класс, к которому относится модель, и опция on_delete.
    * **on_delete** - Django будет эмулировать поведение ограничения SQL, указанного аргументом on_delete
        * **CASCADE** - Каскадное удаление.
        * **PROTECT** - Предотвратите удаление указанного объекта.
        * **RESTRICT** - 
        * **SET_NULL** - Установите ForeignKey значение null; это возможно только в том случае, если null равно True.
        * **SET()** - Установите ForeignKey значение, переданное в SET().
        * **DO_NOTHING** - Не предпринимайте никаких действий.
```python
from django.db import models

class Car(models.Model):
    manufacturer = models.ForeignKey(
        "Manufacturer",
        on_delete=models.CASCADE,
    )

class Manufacturer(models.Model):
    pass
```
*
    * **related_name & related_query_name** - Имя, используемое для отношения от связанного объекта обратно к этому.
```python
class Tag(models.Model):
    name = models.CharField(max_length=255)
    is_deleted = models.BooleanField(default=False)
    article = models.ForeignKey(
        Article,
        models.CASCADE,
        related_name="tags",
        related_query_name="tag",
        limit_choices_to={"is_deleted": False},
    )
    parent_tag = models.ForeignKey(
        'self', 
        on_delete=models.SET_NULL, 
        null=True,
    )

# related_name - имя обратной связи
# получить все связанные со статьей теги
article = Article.objects.get(id=1)
tags = article.tags.all()

# related_query_name - получить все статьи, 
# связанные с определенным тегом
articles = Article.objects.filter(tag__name="example")
```
* **OneToOneField** - Отношение один к одному. Концептуально это похоже на отношение ForeignKey с unique=True, но «обратная» сторона отношения будет напрямую возвращать один объект.
* **ManyToManyField** - Отношение «многие ко многим».
```python
class Publication(models.Model):
    title = models.CharField(max_length=30)

class Article(models.Model):
    headline = models.CharField(max_length=100)
    publications = models.ManyToManyField(
        Publication,
        db_table="publications_article",
    )
```
или
```python
class Person(models.Model):
    friends = models.ManyToManyField("self")
```
или через 3-ю модель
```python
class Person(models.Model):
    name = models.CharField(max_length=50)

class Group(models.Model):
    name = models.CharField(max_length=128)
    members = models.ManyToManyField(
        Person,
        through="Membership",
        through_fields=("group", "person"),
    )

class Membership(models.Model):
    group = models.ForeignKey(Group, on_delete=models.CASCADE)
    person = models.ForeignKey(Person, on_delete=models.CASCADE)
    inviter = models.ForeignKey(
        Person,
        on_delete=models.CASCADE,
        related_name="membership_invites",
    )
    invite_reason = models.CharField(max_length=64)
```
