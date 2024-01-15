# Instalação do djangorestframework 

para usarmos o DRF em seu projeto precisamos realizar alguns passos:

1. Crie e ative um ambiente virtual python:

```bash
$ python3 -m venv venv
$ source ./venv/bin/activate
```

2. Instale as bibliotecas e frameworks nescessários para o desenvolvimento através do gerenciador de pacotes do python `pip`:

```bash
$ pip install django
$ pip install djangorestframework
```

3. Agora com o django instalado precisamos iniciar um novo projeto:

```bash
$ django-admin startproject <nome do projeto>
```

4. Precisamos entrar no arquivo `settings.py` no diretorio principal do seu projeto e adicionar o `rest_framework` no **INSTALLED_APPS**:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework',
]
```

com isso você instalou o DRF em seu projeto e ele está pronto para ser usado.

