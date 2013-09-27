# Django 
## mérite bien un
# Oscar

#### DjangoCong 2013
<small>September, 28th 2013 - UTMB, Belfort, France</small>

<small>[Julien Maupetit](http://julien.maupetit.me) / [@julienmaupetit](http://twitter.com/julienmaupetit)</small>

---

<img src="images/oscar.gif" alt="hollywood oscar" width="20%" class="no-border" />
#### Oscar statuette

--

<img src="images/oscar-wilde.jpg" alt="Oscar Wilde">
#### Oscar Wilde

--

<img src="images/oscar-peterson.jpg" alt="Oscar Peterson" />
##### Oscar Peterson

--

## Django Oscar

#### Framework e-commerce pour Django

<img src="images/django-oscar-logo.png" alt="django oscar logo" class="no-border" />

[https://github.com/tangentlabs/django-oscar](https://github.com/tangentlabs/django-oscar)

---

# Pourquoi 
### utiliser 
## django-oscar ?

--

# “Domain Driven”

---

## [Batzeko.com](http://batzeko.com)

<a href="http://batzeko.com" title="More about batzeko.com" target="_blank">
    <img src="images/batzeko_logo_origami.png" alt="Batzeko logo" class="no-border" />    
</a>

#### Site multi-boutiques de vente en ligne.

<a href=""></a>

--

## Templates

    # settings.py

    TEMPLATE_LOADERS = (
        'django.template.loaders.filesystem.Loader',
        'django.template.loaders.app_directories.Loader',
    )

    TEMPLATE_DIRS = (
        location('templates'),
    )

    from oscar import OSCAR_MAIN_TEMPLATE_DIR
    TEMPLATE_DIRS = TEMPLATE_DIRS + (OSCAR_MAIN_TEMPLATE_DIR,)

--

## Templates: override

    # base.html

    {% extends 'oscar/base.html' %}

    {% block styles %}
        <link rel="stylesheet" type="text/css" href="{% static "oscar/css/styles.css" %}" />
        <link rel="stylesheet" type="text/css" href="{% static "oscar/css/responsive.css" %}" />
        <link rel="stylesheet" type="text/css" href="{% static "batzeko/css/batzeko.css" %}" />
    {% endblock styles %}

--

## Templates: extend

    # base.html

    {% extends 'oscar/base.html' %}

    {% block extrascripts %}
        {{ block.super }}
        ...
    {% endblock extrascripts %}

--

## Oscar apps ...

    # settings.py

    from oscar import get_core_app

    INSTALLED_APPS = [
        'django.contrib.auth',
        ...
    ] + get_core_apps(
        overrides=(
            'apps.catalogue',
            'apps.checkout',
            'apps.promotions',
        )
    )

### ... are dynamically loaded

    from django.db.loading import get_model

    Category = get_model('catalogue', 'Category')

--

#### App label must be the same as parent app

    apps
    ├── __init__.py
    ├── catalogue
    │   ├── __init__.py
    │   ├── app.py
    │   ├── models.py
    │   ├── views.py
    ├── checkout
    │   ├── __init__.py
    │   ├── app.py
    │   ├── models.py
    │   ├── views.py
    └── promotions
        ├── __init__.py
        ├── app.py
        ├── models.py
        └── views.py

--

## `models.py`

    from django.db import models
    from django.utils.translation import ugettext_lazy as _

    from oscar.apps.catalogue.abstract_models import AbstractCategory


    class Category(AbstractCategory):
        """
        Link categories to stores
        """
        store = models.ForeignKey('stores.Store', related_name='categories',
                                  null=True, blank=True)


    from oscar.apps.catalogue.models import *

--

## `views.py`

    from oscar.apps.checkout import views


    class PaymentDetailsView(views.PaymentDetailsView):

        def handle_payment(self, order_number, total_incl_tax, **kwargs):
            # Get PayPal transaction
            try:
                adaptive_transaction = AdaptiveTransaction.objects.get(
                    pay_key=self.pay_key)
            except:
                raise exceptions.UnableToTakePayment(
                    _('PayPal adaptive transaction not found!'))
            #...

---

## Merci !

<img src="images/ComSource_Super.jpg" alt="Super ComSource" />