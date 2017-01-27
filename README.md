# django-autocertbot
How to use certbot on django-heroku

## django
settings.py
```
# SSL LETS ENCRYPTED
ACME_CHALLENGE = env('ACME_CHALLENGE',
                     default=None)
```

app/views.py
```
def acme(request, token):
    return HttpResponse(settings.ACME_CHALLENGE)

```

urls.py
```
from app import views

urlpatterns = [
    ...
    # Let's encrypt cert
    url(r'^.well-known/acme-challenge/(?P<token>.+)/$', views.acme),
    ]

```


## Create cert using certbot
```
sudo certbot certonly --manual -d www.domain.com

Right before acme challenge, copy the acme challenge code and use it on heroku config

```


## heroku config && cert upload
``` # config acme challenge
    heroku config:set ACME_CHALLENGE=<acme-challenge-code> --remote=<env>
    # switch back to certbot page, hit enter, new cert is updated 
    sudo bash # as root
    cd /etc/letsencrypt/live/<env>/
    heroku certs:update fullchain.pem privkey.pem --remote <env>
```
