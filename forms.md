# Формы

## RegistrationSuperUserForm

```python
from django import forms
from .models import User


class RegistrationSuperUserForm(forms.ModelForm):
    password = forms.CharField(widget=forms.PasswordInput)
    password_confirmation = forms.CharField(widget=forms.PasswordInput)
    email = forms.EmailField(required=True)

    def __init__(self, *args, **kwargs):
        super(RegistrationSuperUserForm, self).__init__(*args, **kwargs)
        self.fields['username'].label = 'Login'
        self.fields['email'].label = 'Email'
        self.fields['password'].label = 'Password'
        self.fields['password_confirmation'].label = 'Password confirmation'

    def clean_email(self):
        email = self.cleaned_data['email']
        if User.objects.filter(email=email).exists():
            raise forms.ValidationError(f'Данный электронный адрес уже зарегистрирован')
        return email

    def clean_username(self):
        username = self.cleaned_data['username']
        if User.objects.filter(username=username).exists():
            raise forms.ValidationError(f'{username} занято')
        return username

    def clean(self):
        password = self.cleaned_data['password']
        password_confirmation = self.cleaned_data['password_confirmation']
        if password != password_confirmation:
            raise forms.ValidationError(f'Пароли не совпадают')
        return self.cleaned_data

    class Meta:
        model = User
        fields = [
            'id',
            'username',
            'email',
            'password',
            'password_confirmation',
            # 'last_login',
            'is_superuser',
            'is_staff',
            # 'is_active',
            # 'date_joined',
            'last_name',
            'first_name',
            'patronymic',
            'date_of_birth',
            'gender',
            'phone_number',
            # 'document_1',
            # 'document_2',
        ]
```

## RegistrationUserForm

```python
from django import forms
from .models import User


class RegistrationUserForm(forms.ModelForm):
    email = forms.EmailField(required=True)
    password = forms.CharField(widget=forms.PasswordInput)
    password_confirmation = forms.CharField(widget=forms.PasswordInput)

    def __init__(self, *args, **kwargs):
        super(RegistrationUserForm, self).__init__(*args, **kwargs)
        self.fields['username'].label = 'Login'
        self.fields['email'].label = 'Email'
        self.fields['password'].label = 'Password'
        self.fields['password_confirmation'].label = 'Password confirmation'

    def clean_email(self):
        email = self.cleaned_data['email']
        if User.objects.filter(email=email).exists():
            raise forms.ValidationError(f'Данный электронный адрес уже зарегистрирован')
        return email

    def clean_username(self):
        username = self.cleaned_data['username']
        if User.objects.filter(username=username).exists():
            raise forms.ValidationError(f'{username} занято')
        return username

    def clean(self):
        password = self.cleaned_data['password']
        password_confirmation = self.cleaned_data['password_confirmation']
        if password != password_confirmation:
            raise forms.ValidationError(f'Пароли не совпадают')
        return self.cleaned_data

    class Meta:
        model = User
        fields = [
            'id',
            'username',
            'email',
            'password',
            'password_confirmation',
            # 'last_login',
            # 'is_superuser',
            # 'is_staff',
            # 'is_active',
            # 'date_joined',
            'last_name',
            'first_name',
            'patronymic',
            'date_of_birth',
            'gender',
            'phone_number',
            # 'document_1',
            # 'document_2',
        ]
```

## LoginUserForm

```python
from django import forms
from .models import User
from django.contrib.auth.forms import AuthenticationForm


class LoginUserForm(AuthenticationForm):
    class Meta:
        model = User
        fields = [
            'username',
            'email',
            'password',
        ]
```
