---
layout: post
title: How to add a custom language to Django
description: Learn how to seamlessly add custom languages to your Django project.
tags: Python Django Translations
---


# How to add a custom language to Django

When working on my employer project [(Corsair Explorer)](https://www.corsair.com/uk/en/explorer/) we had the need to add a new language not currently supported by Django.
We're not providing full translation this language but we do need to provide it as an option for some content.

---
My typical layout for django projects is to have a main `core` app that houses 99% of the logic and code.
Although breaking up logic and models into apps is _suggested_ it's very, very rare i reuse an app in a different project so this gives the easiest development experience for me.
The below is part of the `appy.py` file inside the core app in the project.
``` python
# core/apps.py
from django.apps import AppConfig
from django.conf import settings
import django.conf.locale


class CoreConfig(AppConfig):
    default_auto_field = "django.db.models.BigAutoField"
    name = "core"

    def ready(self):
        django.conf.locale.LANG_INFO["mt"] = {
            "bidi": False,
            "code": "mt",
            "name": "Maltese",
            "name_local": "Malti",
        }
```
This can then be referenced in your `settings.py` as you'd normally do for a language

``` python
LANGUAGES = [
    ...
    ("mt", "Maltese"),
    ...
]
```