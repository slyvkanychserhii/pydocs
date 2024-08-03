
https://docs.djangoproject.com/en/5.0/ref/models/options/

* **abstract** - Если эта модель будет абстрактным базовым классом abstract = True
* **db_table** - Имя таблицы базы данных, используемой для модели
* **ordering** - Порядок объекта по умолчанию, используемый при получении списков объектов
```python
ordering = ["pub_date"]
ordering = ["-pub_date"]
ordering = ["-pub_date", "author"]
```
* **verbose_name** - Удобочитаемое название объекта, единственное число
```python
verbose_name = "pizza"
```
* **verbose_name_plural** - Множественное имя объекта
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
