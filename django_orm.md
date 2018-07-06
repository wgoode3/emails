# Simplified Django ORM

```python
# inside of models.py

from django.db import models

class Widget(models.Model):
    name = models.CharField(max_length=255)
    number = models.IntegerField()
    ordered_on = models.DateTimeField()
```

If we jump into the django shell we can test out the Widget class.

```shell
python manage.py shell
```

```python
from apps.example_app.models import Widget
widget = Widget.objects # we can hide objects from students here and talk about what it does in a later (optional) section

w1 = Widget(name="Tony", number=1, ordered_on="2018-07-07")

# we can check what is in this w1 variable with

w1.__dict__
"""
which outputs:

{'_state': <django.db.models.base.ModelState at 0x7fc32029f1d0>,
  'id': None,
  'name': 'Tony',
  'number': 3,
  'ordered_on': '2018-07-07'}
"""

w1.save()

# Alternatively we could also create a Widget like this

w2 = Widget(None, 'Tim', 4, "2018-07-07")
w2.save()

# we need to provide the value None for the id if we don't use keyword value pairs

# we can see what is in the Widget table with 

widget.all()

"""
which outputs:

<QuerySet [<Widget: Widget object (1)>, <Widget: Widget object (2)>]>
"""

```

## Adding simple validations

For a simple validation we can overwrite the default save method, and then do whatever checks we want to on the object. If we determine the object should be saved we can use ```super``` to run the original save method.

```python
# inside of models.py

from django.db import models

class Widget(models.Model):
    name = models.CharField(max_length=255)
    number = models.IntegerField()
    ordered_on = models.DateTimeField()
    
    def save(self, *args, **kwargs):
        if self.name == "Paul":
            return False
        else:
            super(Widget,self).save(*args, **kwargs)
            return True
```

```python
# back in the python shell

w3 = Widget(name="Paul", number=6, ordered_on="2018-07-07")

# when we go to save, it will run the save method with our new check

w3.save()
# This returns False to let us know it wasn't saved... we could replace this with a dictionary of errors if we like

# if we change the name we will be allowed to save the Widget
w3.name = "Anne"

w3.save()
# This returns True, we could just as easily return self.id if we would prefer to have that returned
```

