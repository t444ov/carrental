# База данных Прокат автомобилей

В базе данных реализованы модели: _**пользователи**_(**User**), _**автомобили**_(**Car**), _**прокат автомобилей**_(**RentCar**), _**статьи**_(**Article**), _**о нас**_(**AboutUs**).

## Модель User

```python

from django.db import models
from django.contrib.auth.models import AbstractUser
from django.core.validators import MinLengthValidator, RegexValidator
from django.utils import timezone


class User(AbstractUser):
    id = models.BigAutoField(
        auto_created=True,
        primary_key=True,
        serialize=False,
        verbose_name='ID',
        db_column='id'
    )
    email = models.EmailField(
        unique=True,
        verbose_name='Email',
        db_column='email'
    )
    password = models.CharField(
        max_length=128,
        validators=[MinLengthValidator(4)],
        verbose_name='Password',
        db_column='password'
    )
    last_login = models.DateTimeField(
        blank=True,
        null=True,
        verbose_name='last login'
    )
    is_superuser = models.BooleanField(
        default=False,
        help_text='Designates that this user has all permissions without explicitly assigning them.',
        verbose_name='superuser status'
    )
    is_staff = models.BooleanField(
        default=False,
        help_text='Designates whether the user can log into this admin site.',
        verbose_name='staff status'
    )
    is_active = models.BooleanField(
        default=True,
        help_text='Designates whether this user should be treated as active. Unselect this instead of deleting accounts.',
        verbose_name='active'
    )
    date_joined = models.DateTimeField(
        default=timezone.now,
        verbose_name='date joined',
        db_column='date_joined'
    )
    ALPHA = RegexValidator(r'^[a-zA-Zа-яА-Я]*$', 'Only alpha characters are allowed.')
    last_name = models.CharField(
        max_length=60,
        validators=[ALPHA],
        verbose_name='Last name',
        db_column='last_name'
    )
    first_name = models.CharField(
        max_length=60,
        validators=[ALPHA],
        verbose_name='First name',
        db_column='first_name'
    )
    patronymic = models.CharField(
        max_length=60,
        validators=[ALPHA],
        null=True,
        blank=True,
        verbose_name='Patronymic',
        db_column='patronymic'
    )
    date_of_birth = models.DateField(
        verbose_name='Date of birth',
        db_column='date_of_birth'
    )
    GENDER = [
        ('Male', 'Male'),
        ('Female', 'Female'),
        ('Other', 'Other'),
    ]
    gender = models.CharField(
        max_length=7,
        choices=GENDER,
        verbose_name='Gender',
        db_column='gender'
    )
    NUMERIC = RegexValidator(r'^[0-9+]*$', 'Only numeric characters are allowed.')
    phone_number = models.CharField(
        max_length=12,
        validators=[MinLengthValidator(12), NUMERIC],
        unique=True,
        verbose_name='Phone number',
        db_column='phone_number'
    )
    # document_1 = models.ImageField(
    #     upload_to='img/user_document/',
    #     null=True,
    #     blank=True,
    #     verbose_name="Front side of the document (Driver's license or Pass)",
    #     db_column='document_1'
    # )
    # document_2 = models.ImageField(
    #     upload_to='img/user_document/',
    #     null=True,
    #     blank=True,
    #     verbose_name="Back side of the document (Driver's license or Pass)",
    #     db_column='document_2'
    # )

    class Meta:
        verbose_name = 'user'
        verbose_name_plural = 'Users'
        db_table = 'users'

    def __str__(self):
        return f'{self.first_name} {self.last_name} : {self.email}, {self.phone_number}'

```

## Модель Car

```python

from django.db import models
from django.utils import timezone


import datetime
YEAR_CHOICES = []
for r in range(1900, (datetime.datetime.now().year+1)):
    YEAR_CHOICES.append((r, r))
YEAR_CHOICES = list(reversed(YEAR_CHOICES))


class Car(models.Model):
    id = models.BigAutoField(
        auto_created=True,
        primary_key=True,
        serialize=False,
        verbose_name='ID',
        db_column='id'
    )
    brand = models.CharField(
        max_length=255,
        verbose_name='Brand',
        db_column='brand'
    )
    model = models.CharField(
        max_length=255,
        verbose_name='Model',
        db_column='model'
    )
    BODY_TYPE_CHOICES = [
        ('Convertible', 'Convertible'),
        ('Coupe', 'Coupe'),
        ('SUV', 'SUV'),
        ('Sedan', 'Sedan'),
        ('Truck', 'Truck'),
        ('Van', 'Van'),
        ('Wagon', 'Wagon'),
        ('Hatchback', 'Hatchback'),
        ('Hybrid', 'Hybrid'),
        ('Electric', 'Electric'),
    ]
    body_type = models.CharField(
        max_length=20,
        choices=BODY_TYPE_CHOICES,
        verbose_name='Body type',
        db_column='body_type'
    )
    year_of_issue = models.IntegerField(
        choices=YEAR_CHOICES,
        default=datetime.datetime.now().year,
        verbose_name='Year of issue',
        db_column='year_of_issue'
    )
    color = models.CharField(
        max_length=255,
        verbose_name='Color',
        db_column='color'
    )
    engine_volume = models.FloatField()
    power = models.IntegerField()
    FUEL_TYPE_CHOICES = [
        ('Gasoline', 'Gasoline'),
        ('Diesel', 'Diesel'),
        ('Electricity', 'Electricity'),
    ]
    fuel_type = models.CharField(
        max_length=12,
        choices=FUEL_TYPE_CHOICES,
        verbose_name='Fuel type',
        db_column='fuel_type'
    )
    TRANSMISSION_TYPE_CHOICES = [
        ('Automatic', 'Automatic'),
        ('Manual', 'Manual'),
        ('Automated Manual', 'Automated Manual'),
        ('Continuously Variable', 'Continuously Variable'),
    ]
    transmission_type = models.CharField(
        max_length=30,
        choices=TRANSMISSION_TYPE_CHOICES,
        verbose_name='Transmission type',
        db_column='transmission_type'
    )
    CAR_DRIVE_TYPE_CHOICES = [
        ('FWD', 'FWD'),
        ('RWD', 'RWD'),
        ('AWD', 'AWD'),
        ('4WD', '4WD'),
    ]
    car_drive_type = models.CharField(
        max_length=35,
        choices=CAR_DRIVE_TYPE_CHOICES,
        verbose_name='Car drive type',
        db_column='car_drive_type'
    )
    license_plate = models.CharField(
        max_length=10,
        unique=True,
        verbose_name='License plate',
        db_column='license_plate'
    )
    number_of_seats = models.IntegerField()
    description = models.CharField(
        max_length=255,
        verbose_name='Description',
        db_column='description'
    )
    price = models.FloatField()
    # image = models.ImageField(
    #     upload_to='img/car_image/',
    #     null=True,
    #     blank=True,
    #     verbose_name='Image',
    #     db_column='image'
    # )
    CAR_STATUS_CHOICES = [
        ('Available', 'Available'),
        ('Reserved', 'Reserved'),
        ('Rented', 'Rented'),
        ('Maintenance', 'Maintenance'),
    ]
    car_status = models.CharField(
        max_length=12,
        choices=CAR_STATUS_CHOICES,
        default='Available',
        verbose_name='Car status',
        db_column='car_status'
    )
    publishing_date = models.DateTimeField(
        default=timezone.now,
        verbose_name='Publishing date',
        db_column='publishing_date'
    )
    published_by = models.ForeignKey(
        settings.AUTH_USER_MODEL,
        on_delete=models.CASCADE,
        verbose_name='Published by',
        db_column='published_by'
    )

    class Meta:
        verbose_name = 'car'
        verbose_name_plural = 'Cars'
        db_table = 'cars'

    def __str__(self):
        return f'{self.brand} {self.model} : {self.license_plate}'


```

## Модель RentCar

```python

from django.db import models
from django.utils import timezone
from django.conf import settings


class RentCar(models.Model):
    id = models.BigAutoField(
        auto_created=True,
        primary_key=True,
        serialize=False,
        verbose_name='ID',
        db_column='id'
    )
    client = models.ForeignKey(
        settings.AUTH_USER_MODEL,
        on_delete=models.CASCADE,
        verbose_name='Client',
        db_column='client'
    )
    car = models.ForeignKey(
        Car,
        on_delete=models.CASCADE,
        verbose_name='Car',
        db_column='car'
    )
    booking_date = models.DateTimeField(
        default=timezone.now,
        verbose_name='Booking date',
        db_column='booking_date'
    )
    date_of_issue = models.DateTimeField(
        verbose_name='Date of issue',
        db_column='date_of_issue'
    )
    planned_return_date = models.DateTimeField(
        verbose_name='Planned return date',
        db_column='planned_return_date'
    )
    total_days = models.IntegerField(
        null=True,
        blank=True,
        verbose_name='Total days',
        db_column='total_days'
    )
    is_bill_paid = models.BooleanField(
        default=False,
        verbose_name='is bill paid',
        db_column='is_bill_paid'
    )
    return_date = models.DateTimeField(
        null=True,
        blank=True,
        verbose_name='Return date',
        db_column='return_date'
    )
    total_payment_amount = models.FloatField(
        null=True,
        blank=True,
        verbose_name='Total payment amount',
        db_column='total_payment_amount'
    )
    request_responded_by = models.ForeignKey(
        settings.AUTH_USER_MODEL,
        on_delete=models.CASCADE,
        blank=True,
        null=True,
        related_name='workers',
        verbose_name='Request responded by',
        db_column='request_responded_by'
    )
    REQUEST_STATUS_CHOICES = [
        ('Pending', 'Pending'),
        ('Accepted', 'Accepted'),
        ('Declined', 'Declined'),
    ]
    request_status = models.CharField(
        max_length=30,
        choices=REQUEST_STATUS_CHOICES,
        default="Pending",
        verbose_name='Request status',
        db_column='request_status'
    )

    class Meta:
        verbose_name = 'rent car'
        verbose_name_plural = 'Rent cars'
        db_table = 'rent_cars'

    def __str__(self):
        return f'{self.client} : {self.car}'

```

## Модель Article

```python

from django.db import models
from django.utils import timezone


class Article(models.Model):
    title = models.CharField(max_length=255)
    date_created = models.DateField(default=timezone.now)
    # image = models.ImageField(upload_to='img/article_image/', null=True, blank=True)
    description = models.TextField()

    def __str__(self):
        return f'{self.title}, {self.date_created}, {self.description}'

```

## Модель AboutUs

```python

from django.db import models
from django.utils import timezone


class AboutUs(models.Model):
    title = models.CharField(max_length=255)
    date_created = models.DateField(default=timezone.now)
    # image = models.ImageField(upload_to='img/about_us_image/', null=True, blank=True)
    description = models.TextField()

    def __str__(self):
        return f'{self.title}, {self.date_created}, {self.description}'

```
