## Requirements

- Functional

  - Convert url to a shorter unified version (ThisIsALongUrlPleaseShortenMe.com => domain.com/smallId)
  - Redirect user to the short url version

- ## Non Functional

  - Minimize latency for redirection
  - 10K RPS
  - 5B Urls lifetime
  - 100M DAU

- ## API & DB

  - DB

    - No relations needed, we can use NoSQL: partition-friendly key lookups (Exact use case we have)

  - Shorten url

    - Endpoint: /api/v1/short
    - Method: POST
    - Body: {url}

  - Redirect user
    - Endpoint: /api/v1/redirect/:id
    - Method: GET
    - Query param: {id}

- ## High level design

<PICTURE>

- ## Deep dive

  - How will the unique url ids be created?

    - Base62 encoding

  - Redirect 301 or 302?

    - Since we will go with analytics we can use 302, in case we aren't we can use 301 since it will be cached in the user browser and the service will be meaningless

  - Machine ID (Prefix) as Shard Key, use Sharding to scale for higher loads
