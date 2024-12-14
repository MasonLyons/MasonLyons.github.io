---
layout: post
title: How to add a custom language to Django
description: Learn how to seamlessly add custom languages to your Django project.
permalink: /how-to-add-a-new-language-to-django
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
It's key that the `django.conf.locale.LANG_INFO` key matches up with the `code` value, in this case `mt`.
The other keys are for the following:
 - bidi: languages that are essentially written and read from right-to-left.
 - code: how the url will appear in the url. For example `https://example.come/mt/blog`
 - name: The name of the language in english
 - name_locale: The name of the language as written in the language

This can then be referenced in your `settings.py` as you'd normally do for a language
``` python
LANGUAGES = [
    ...
    ("mt", "Maltese"),
    ...
]
```
