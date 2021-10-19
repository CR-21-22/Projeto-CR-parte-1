# Laboratório: primeira web app django ⛅

**OBJECTIVO**: 
* Neste laboratório criará uma primeira aplicação django simples, para se familiarizar com os conceitos de urls, views, templates e sua linguagem. 
* A ideia é trabalhar a estrutura e dinamicidade. O conteúdo é secundário.
* Exercitará a edição dos módulos urls.py, views.py e a criação de templates HTML com linguagem template.

**RECOMENDAÇÕES**: 
* Leia uma vez o enunciado. É extenso, mas detalha todos os passos e fornece o código necessário, sendo rápida a sua realização.
* Instale e use o Pycharm (preferencialmente a versão profissional, usando as credenciais da universidade) para editar o código de forma fácil. O Pycharm sinaliza os erros. Veja com atenção eventuais mensagens. 
* quando necessário, guie-se pelo projeto que está no repo.
* se tiver dúvidas, consulte os slides e a documentação do [djangoproject](https://www.djangoproject.com/)

## 1. Primeiros passos 👶
Vamos nesta secção criar um projeto e aplicação django.

### 1.1. Crie um projeto e app django
1. Abra a linha de comandos (PowerShell ou cmd) e execute os comandos em baixo a cinzento. 
1. Crie e entre na pasta projeto-django `mkdir projeto-django; cd projeto-django`
1. Instale o pipenv executando `pip install pipenv`
1. Crie um ambiente virtual com django `pipenv install django`
1. Active o ambiente virtual `pipenv shell`
1. crie um projeto django `django-admin startproject config .`
1. Migre as base de dados `python manage.py migrate`
1. Lance o projeto para ver se está tudo ok, com o comando `python manage.py runserver` 
1. Pare o servidor com Ctrl + C
1. Crie a aplicação website, com a instrução `python manage.py startapp website`

### 1.2. Configure a aplicação
1. abra a pasta `projeto-django` com o Pycharm 
    * botão direito sobre a pasta, e escolha lançar com Pycharm
    * abra o Pycharm e abra a pasta `projeto-django` 
3. em config/settings.py registe a aplicação na lista INSTALLED_APPS, colocando no fim `'website'`
4. em config/urls.py registe a rota para a nova aplicação website, inserindo na lista `urlpatterns` o caminho `path('', include('website.urls))` para a sua aplicação, ficando:

```python
# config/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('website.urls')),
]
```

**Nota**: o segundo path especifica `''`, querendo dizer que por defeito será encaminhado para a aplicação diretamente. 

## 3. Templates 🖺
Designa-se de template um ficheiro HTML retornado  ao browser por uma função view específica da `views.py`, eventualmente renderizado com conteúdos. Começamos assim por construir os conteúdos que teremos para retornar a um cliente. Vamos criar um template base\pai que terá o layout, os restantes consistindo em templates "filhos" que herdam e estendem o layout base, inserindo conteúdos neste.

### 3.1 Layout base
1. na pasta `website` crie a pasta `templates`, e dentro dessa a pasta `/website`, ficando com o caminho `projeto-django/website/templates/website`
1. Crie, na pasta `website/templates/website`, o ficheiro `layout.html`, usando o snippet HTML5 sugerido pelo Pycharm. 
1. USaremos estilos definidos pelo [Bootstrap](https://getbootstrap.com/), que contêm especificação de estilos pa vários tipos de elementos. Integre no elemento `<head>` um link para o bootstrap, `<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">`. 
2. O layout será inspirado no Bootstrap [jumbotron](https://www.w3schools.com/bootstrap4/bootstrap_jumbotron.asp). Como no head temos especificado um link para a stylesheet do Bootstrap, iremos utilizar várias das suas classes que permitem formatar elementos.
 
O template layout.html que construiremos a seguir terá a seguinte estrutura:
```html
<!-- layout.html -->
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
    		<title>Website</title>
		<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
	</head>
	<body>
	    <header>...</header>
	    <article>
		<main>...</main>
		<aside>...</aside>
	    </article>
	    <footer>...</footer>
	</body>
</html>
```
**Nota**: `<!-- base.html -->` em HTML é um comentário

As etiquetas `header, article, main, aside, footer` são [etiquetas semânticas](https://www.w3schools.com/html/html5_semantic_elements.asp) do HTML5, que servem de identificadores de blocos que podem ser estilizados com CSS. 

#### header
1. No elemento `<body>` crie aninhado o elemento `<header class="jumbotron text-center">` com as classes Bootstrap jumbotron que evidenciará o cabeçalho do website, e text-center que centrará o texto. Dentro do elemento header deverá ter aninhado três elementos:
    1. um elemento `<h1>`com o título do website
    1. um elemento `<p>` com uma frase curta da mesma largura do título
    2. um elemento `<nav>` três hiperlinks `<a>` para três páginas que o seu site irá ter, cada com a classe `class="btn btn-info"` que transforma o hiperlink num botão (ficando por exemplo `<a href="" class="btn btn-info">Home</a>)`. A forma de incluir o link em `href` será especificada na secção 7.

#### main
1. Por baixo do `<header>`, crie uma secção `<article class="container">`, com a classe Bootstrap. O article irá ter dentro dois elementos, o `<main>` e o `<aside>` (veja a estrutura acima).
1. O elemento `main` tem um classe bootstrap que ocupará 6 colunas de largura ([responsive grid](https://www.w3schools.com/css/css_rwd_grid.asp)). Contém uma etiqueta template `{% block main %}` (em vez de usarmos a palavra `content`, podemos usar outras) que especifica que este template será estendido com conteúdos templates filhos. 
```html
<!-- base.html -->
...
<main class="col-sm-6"> 
	{% block main %}
	{% endblock main %}
</main>
```

1. O elemento `<aside>` terá a mesma classe `<aside class="col-sm-6">`. Dentro deste elemento deverá inserir um elemento `<img>` com uma imagem à sua escolha. A forma como o fazer será explicado mais em baixo.

#### footer
1. A seguir ao `<header>`crie um elemento `<footer></footer>`, com um texto simples de rodapé. 

### 3.2 Templates Filhos
1. Crie três ficheiros HTML que estendam o layout base.html segundo a seguinte sintaxe:

```html
<!-- home.html -->

{% extends 'website/base.html' %}

{% block main %}
    <h3>Titulo</h3>
    <p>texto texto texto texto texto texto texto </p>
{% endblock %}
```
1. Estes terão os conteúdos que irão aparecer no elemento main. 
2. A única coisa que mudará entre os três elementos será o conteúdo do block main.
3. Especifica para cada um deles um título texto, duas ou tres frases basta.

## 4. Static 🖼️
A pasta static contém ficheiros "estáticos", i.e., imagens, ficheiros CSS e scripts JavaScript. Estes organizam-se em pastas especificas. Usaremos a seguinte estrutura para guardar uma imagem e um ficheiro css:
```dos
projeto-django
└───website
    └───static
        └───website
            ├───css
            │       base.css
            │
            └───images
                    imagem.png	    
```
É extensa, mas previne problemas de ambiguidade.

1. crie a estrutura acima. na pasta `website` crie a pasta `static`, e dentro dessa a pasta `website`. Esta pasta deverá conter uma pasta `css` e outra `ìmages`. 

### 4.1 CSS
1. Crie dentro da pasta `css` o ficheiro `base.css` com algumas configurações.
1. configure neste a estilização do elemento footer, por forma a que fique em baixo. Poderá configurar desta forma:  

```css
/* base.css */

footer {
   position: fixed;
   bottom: 30px;
   width: 100%;
   text-align: center;
}
```
1. configure o elemento article de forma a ficar centrado e com largura máxima de 800px
```css
/* base.css */

body > article {
    max-width: 800px;
    margin: auto;
}
```

### 4.2 images
1. Insira em `website/static/website/images` uma imagem a seu gosto, com uma largura máxima de 200px, que irá ficar no elemento `aside` acima definido. A sua referência no `src`é especificada na secção 7.

## 5. Views ⚙️
As views são funções responsáveis por responder ao pedido (request) de um recurso (URL), retornando o recurso pedido, um template HTML eventualmente renderizado com dados e customizado. Fazem assim a interligação entre os dados e os templates, respondendo aos pedidos encaminhados via urls.

1. no ficheiro `views.py` crie uma função view que renderize cada uma das páginas. Por exemplo, para renderizar a página home.html teremos a função `home_page_view`:

```python
#  hello/views.py

from django.shortcuts import render

def home_page_view(request):
	return render(request, 'website/home.html')
```
1. defina outras funções para as restantes páginas do seu webstie

## 6. URLS ✉️
Existem dois ficheiros ficheiros urls. O `urls.py` da pasta `config` (`config\urls.py`), responsável por encaminhar um pedido de um recurso à respetiva aplicação (no nosso caso apenas temos uma aplicação, website). E também deverá existir um módulo `urls.py` na pasta `website` (`website\urls.py`). Este irá mapear, para um determinado pedido (*request*) de recurso, uma função do ficheiro views.py que tratará desse pedido, preparando e devolvendo o recurso pedido num template HMTL.

1. o config/urls.py já está configurado. Contém rotas para as aplicações existentes dentro do projeto, que configurou em 1.2. 

3. Na pasta website crie o ficheiro `urls.py`. Este deverá especificar as rotas existentes para os vários  Exemplifica-se em baixo uma rota na lista urlpatterns, devendo incluir uma rota para cada uma das três views anteriormente criadas. 

```python
#  hello/urls.py

from django.urls import path
from . import views

app_name = "website"

urlpatterns = [
    path('home', views.home_page_view, name='home')
]
```
* Este módulo importa a função `path()`, responsavel por mapear a rota (`home`) na função (`views.home_page_view`).
* Este módulo importa o módulo `views` que se encontra na mesma pasta (e por isso é importado como `from . import views`), por forma a poder falar das funções view. 


## 7. Hiperlinks 🔗
1. falta especificar o conteúdo dos hiperlinks do menu. Insira `href="{% url 'website:home' %}"`, onde `website` é o nome dado à app (em `app_name`), e `home` é o nome do path especificado em website/urls.py. 
2. Para a imagem `<img>` no ficheiro `base.html`, inclua antes desta a etiqueta template `{% load static %}`, para construir o URL para o path relativo. Na especificação da `src`, use a etiqueta template `{% static 'website/images/image.png' %}`, ficando da seguinte forma:

```html
<!-- layout.html -->
...
{% load static %}
<img src="{% static 'website/images/image.png' %}">
```
3. para o ficheiro base.css, devemos também incluir no ficheiro `layout.html` um link, usando o path relativo para a pasta static:
```html
<!-- layout.html -->
...
{% load static %}
<link rel="stylesheet" href="{% static 'website/css/base.css' %}">
```

## 8. Ready... GO! 🏁
1. Lance a aplicação com o comando `python manage.py runserver` e verifique que consegue visualizar corretamente a aplicação que fez. 

# 9. Repositório GitHub e Video ⛅
* Crie um repositório no github com o seu projeto
* Grave um vídeo de 30 segundos onde navega no seu website, mostrando as várias páginas. Pode fazê-lo usando a aplicação OBS. Carregue-o para o Youtube e disponibilize-o como público.

# 11. Submissão
Submeta hiperlink para o seu repositório github e para o Youtube


*Espero que tenha gostado de conhecer um pouco do funcionamento do django e de ter feito uma web app simples*
