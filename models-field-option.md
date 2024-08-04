
https://docs.djangoproject.com/en/5.0/ref/models/fields/#field-options

# Field options

наиболее часто используемых общих аргументы:

- **null** - Если True, Django сохранит пустые значения, как NULL в базе данных. По умолчанию False. - проверка базы данных
- **blank** - Если True, поле может быть пустым. По умолчанию False. - проверка формы
- **choices** - 
```python
class Person(models.Model):
    SHIRT_SIZES = {
        "S": "Small",
        "M": "Medium",
        "L": "Large",
    }
    shirt_size = models.CharField(max_length=1, choices=SHIRT_SIZES)
```
или
```python
class Runner(models.Model):
    MedalType = models.TextChoices("MedalType", "GOLD SILVER BRONZE")
    medal = models.CharField(blank=True, choices=MedalType, max_length=10)
```
или
```python
class Person(models.Model):
    class ShirtSize(models.TextChoices):
        S_SMALL = "S", _("Small")
        S_MEDIUM = "M", _("Medium")
        S_LARGE = "L", _("Large")
        
    shirt_size = models.CharField(max_length=1, choices=ShirtSize, default=ShirtSize.S_MEDIUM)
    
    def __str__(self):
        ss = self.shirt_size
        return f'{ss.label} -> "S Medium"; {ss.value} -> "M"; {ss.name} -> "Medium"'
```
или
```python
MedalType = models.TextChoices("MedalType", "GOLD SILVER BRONZE")
MedalType.choices
# [('GOLD', 'Gold'), ('SILVER', 'Silver'), ('BRONZE', 'Bronze')]
Place = models.IntegerChoices("Place", "FIRST SECOND THIRD")
Place.choices
#[(1, 'First'), (2, 'Second'), (3, 'Third')]
```
- **default** - Значение по умолчанию для поля. Это может быть значение или вызываемый объект. Если вызываемый, он будет вызываться каждый раз при создании нового объекта.
- **help_text** - Дополнительный текст «помощи», отображаемый с помощью виджета формы. Он полезен для документации, даже если ваше поле не используется в форме.
- **primary_key** - Если True, это поле является первичным ключом для модели.
- **unique** - Если True, это поле должно быть уникальным во всей таблице.
- **unique_for_date** - Задайте здесь имя DateField или DateTimeField чтобы потребовать, чтобы это поле было уникальным для значения поля даты. Например, если у вас есть поле title в котором есть unique_for_date="pub_date", то Django не допустит ввода двух записей с одинаковыми значениями title и pub_date.
- **editable** - Если False, поле не будет отображаться в админке или любой другой ModelForm. Они также пропускаются при проверке модели . По умолчанию True.
- **db_column** - Имя столбца базы данных, используемого для этого поля. Если это не указано, Django будет использовать имя поля.
