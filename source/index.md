---
title: C0#00ff00 API Docs

language_tabs:
  - API

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>
includes:
  - errors

search: true
---

# Introduction

Welcome to the SeeNoGreen API! 

See No Green is a Fishrod reusable technology for green screen experiences. The API allows the associated apps and frontend websites to post data about green screen projects, scenes, customer data, etc. 

To make the documentation language agnostic, we specify all request examples in a standardized way:

<aside class="notice">
'METHOD', 'URL', params(), headers()
</aside>

# Registration

To register a user account, make a request of the type:

### HTTP Request

`POST http://seenogreen.co.uk/security/users.json`

### HTTP Parameters

Parameter | Required | Description
--------- | ------- | -----------
username | true | Your username.
email | true | Your e-mail.
plainPassword | true | Your plaintext password.

> To register, use this request:

```API
'POST', '/security/users.json', params('email' => 'bogdan@fishrod.co.uk', 'username' => 'bogdan', 'plainPassword' => 'trickypassword'))
```

> The sever responds with json in the format:

```json

{
  "id": 3,
  "username_canonical": "bogdan",
  "email": "bogdan@fishrod.co.uk",
  "email_canonical": "bogdan@fishrod.co.uk",
  "enabled": true,
  "locked": false,
  "expired": false,
  "roles": [],
  "credentials_expired": false,
  "uuid": "852dd18b-066e-43ff-b446-9eb684bd08ab",
  "_links": {
    "self": {
      "href": "http://localhost:8090/api/users/852dd18b-066e-43ff-b446-9eb684bd08ab.json"
    }
  }
}
```

# Authentication

> To log-in and get a WSSS token, use this request:

```API
'POST', '/security/tokens/creates.json', params('_username' => 'TestUser1', '_password' => 'myPassword'))
```

See No Green uses WSSE authentication. You must obtain a WSSE token and attach that to every request you make to the API.

If you have registered as a user, you can login by making a post request to the token creation endpoint with your username and password as request parameters. If your authentication is successful, you will receive the WSSE token as described on the right.

> The sever responds with json in the format:

```json
{
  "WSSE": "UsernameToken Username=\"TestUser1\", PasswordDigest=\"MqNBPaF2PO8xU2ypW4IOLcwZveg=\", Nonce=\"OWU2NzRhMjQwZmQ3ZDVkYg==\", Created=\"2015-01-29T10:34:25+00:00\""
}
```

> The WSSE field is your WSSE TOKEN.

# Authorization

To access API endpoints with your token, you must use the following headers:

        'CONTENT_TYPE' => 'application/x-www-form-urlencoded',

        'HTTP_AUTHORIZATION' => 'WSSE profile="UsernameToken"',

        'ACCEPT' => 'application/json',

        'HTTP_X-wsse' => [YOUR TOKEN]


> Example API access.

```API
'GET', '/api/users.json', params(), headers('CONTENT_TYPE' => 'application/x-www-form-urlencoded',
        'HTTP_AUTHORIZATION' => 'WSSE profile="UsernameToken"',
        'ACCEPT' => 'application/json',
        'HTTP_X-wsse' => 'UsernameToken Username=\"TestUser1\", PasswordDigest=\"MqNBPaF2PO8xU2ypW4IOLcwZveg=\", Nonce=\"OWU2NzRhMjQwZmQ3ZDVkYg==\", Created=\"2015-01-29T10:34:25+00:00\"')
```

Because the WSSE headers are quite long, we will remove them from example responses and replace them with [WSSE HEADERS] where they are required.

Tokens expire after a while.

<aside class="notice">
You will receive a 403 forbidden response if your token is invalid, expired or you don't have access to a resource due to API permissions.

If you think your request seems to have the right headers and you expect a successful response, it probably means there's a formatting problem - you missed a comma, quote, or your string encoding is wrong.

</aside>

# HATEOAS

All API endpoints in SeeNoGreen are HATEOAS-enabled. This means that JSON responses contain navigable links to child or parent resources, as well as the resource itself. Collections support pagination via URL query parameters - requesting a collection with no parameters will return the first page with link to the first, next and last page with preformatted query parameters. This means that all responses are effectively self-documenting, and a HATEOAS-aware client should be able to navigate the API without direct knowledge of other endpoints after entry through one endpoint. 

Example JSON responses are shown on the right (throughout the documentation).

> Resource reponses look like this:

```json
{
  "id": 1,
  "scenedata": "some scene data",
  "uuid": "8065cf21-9bbf-4826-a76e-fdb79fac47d3",
  "_links": {
    "self": {
      "href": "http:\/\/localhost\/api\/scenes\/8065cf21-9bbf-4826-a76e-fdb79fac47d3.json"
    },
    "project": {
      "href": "http:\/\/localhost\/api\/projects\/a6f54731-a041-447f-a353-9c958aa9f9d3.json"
    }
  }
}
```

> Some resources have child resources:

```json
{
  "id": 1,
  "uuid": "f3afba72-abf0-4758-9e51-10d44d0ac30f",
  "name": "TestProject1",
  "_links": {
    "self": {
      "href": "http:\/\/localhost\/api\/projects\/f3afba72-abf0-4758-9e51-10d44d0ac30f.json"
    },
    "user": {
      "href": "http:\/\/localhost\/api\/users\/ec887123-8813-4b87-b34e-f2021670da48.json"
    },
    "scenes": {
      "href": "http:\/\/localhost\/api\/projects\/f3afba72-abf0-4758-9e51-10d44d0ac30f\/scenes.json"
    }
  }
}
```

> Collections looks like this:

```json
{
  "page": 1,
  "limit": 20,
  "pages": 1,
  "total": 1,
  "_links": {
    "self": {
      "href": "\/api\/users\/379fb09b-c2c3-4724-9849-362868e8cae5\/projects.json?page=1&limit=20"
    },
    "first": {
      "href": "\/api\/users\/379fb09b-c2c3-4724-9849-362868e8cae5\/projects.json?page=1&limit=20"
    },
    "last": {
      "href": "\/api\/users\/379fb09b-c2c3-4724-9849-362868e8cae5\/projects.json?page=1&limit=20"
    }
  },
  "_embedded": {
    "projects": [
      {
        "id": 1,
        "uuid": "dcda2726-bf2b-43a2-b38d-b8ab151700dc",
        "name": "TestProject1",
        "_links": {
          "self": {
            "href": "http:\/\/localhost\/api\/projects\/dcda2726-bf2b-43a2-b38d-b8ab151700dc.json"
          }
        }
      }
    ]
  }
}
```


# Whoami and auths

You can retrieve your user details like this, in order to store your user UUID to access user subresources:

> Get your user details with a request of the form.

```API
'GET', '/api/whoami.json', params(), headers([WSSE HEADERS])
```

> The sever responds with json in the format:

```json
{
  "id": 1,
  "username_canonical": "testuser1",
  "email": "TestUser1@9gag.com",
  "email_canonical": "testuser1@9gag.com",
  "enabled": false,
  "groups": [
    
  ],
  "locked": false,
  "expired": false,
  "roles": [
    "ROLE_ACCOUNTUSER"
  ],
  "credentials_expired": false,
  "uuid": "a0b95e7f-2535-45be-810c-396f0f39d890",
  "_links": {
    "self": {
      "href": "http:\/\/localhost\/api\/users\/a0b95e7f-2535-45be-810c-396f0f39d890.json"
    }
  }
}
```

> Check if your token is valid by making a request of the form:

```API
'GET', '/api/auths.json', params(), headers([WSSE HEADERS])
```

> This returns no useful content, but a 200 code means your token is valid, while a 403 means it's not. This is essentially just a dummy API request, you can check if your token is still valid by making any request to a protected endpoint.


### HTTP Request

`GET http://seenogreen.co.uk/api/whoami.json`

You can check if you have a valid token (i.e. to see if your token has expired or is somehow invalidated) with the following request:

`POST http://seenogreen.co.uk/api/auths.json`

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Isis",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/3"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Isis",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">If you're not using an administrator API key, note that some kittens will return 403 Forbidden if they are hidden for admins only.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the cat to retrieve

