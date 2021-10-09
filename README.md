# bug-opentelemetry-django-asgi

Presented with this error message when attempting to run Python 3.8.11 // Django 3.2.8 // Channels 3.0.4 // OpenCensus middleware.

> # 500 Internal Server Error
> 
> Exception inside application.
> 
> _Daphne_

<details><summary>Screenshot</summary>
  <p>
    ![image](https://user-images.githubusercontent.com/23239955/136654348-619d3aee-94bb-4a8d-bce9-fb0c3db1c11c.png)
  </p>
</details>

## With the Django channels app enabled
```
python manage.py runserver --settings testproject.with_channels

HTTP GET / 200 [0.01, 127.0.0.1:36604]
```
## With the OpenCensus middleware
```
python manage.py runserver --settings testproject.with_opencensus

[09/Oct/2021 07:41:18] "GET / HTTP/1.1" 200 40
```
## With both enabled
```
python manage.py runserver --settings testproject.with_both

Internal Server Error: /
Traceback (most recent call last):
  File "/home/vscode/.local/lib/python3.8/site-packages/django/core/handlers/exception.py", line 47, in inner
    response = get_response(request)
  File "/home/vscode/.local/lib/python3.8/site-packages/django/utils/deprecation.py", line 119, in __call__
    response = self.process_response(request, response)
  File "/home/vscode/.local/lib/python3.8/site-packages/django/middleware/clickjacking.py", line 26, in process_response
    if response.get('X-Frame-Options') is not None:
AttributeError: 'coroutine' object has no attribute 'get'
Exception inside application: object HttpResponse can't be used in 'await' expression
Traceback (most recent call last):
  File "/home/vscode/.local/lib/python3.8/site-packages/channels/staticfiles.py", line 44, in __call__
    return await self.application(scope, receive, send)
  File "/home/vscode/.local/lib/python3.8/site-packages/channels/routing.py", line 71, in __call__
    return await application(scope, receive, send)
  File "/home/vscode/.local/lib/python3.8/site-packages/django/core/handlers/asgi.py", line 161, in __call__
    response = await self.get_response_async(request)
  File "/home/vscode/.local/lib/python3.8/site-packages/django/core/handlers/base.py", line 150, in get_response_async
    response = await self._middleware_chain(request)
TypeError: object HttpResponse can't be used in 'await' expression
HTTP GET / 500 [0.68, 127.0.0.1:36658]
```
