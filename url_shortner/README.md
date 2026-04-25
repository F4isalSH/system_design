# URL Shortener

## Requirements

### Functional
- Convert a URL to a shorter unified version (`ThisIsALongUrlPleaseShortenMe.com` → `domain.com/smallId`)
- Redirect user to the original URL from the short version

### Non-Functional
- Minimize latency for redirection
- 10K RPS
- 5B URLs lifetime
- 100M DAU

## API & Database

### Database
- No relations needed — use NoSQL: partition-friendly key lookups (exact use case we have)

### Endpoints

**Shorten URL**
- Endpoint: `/api/v1/short`
- Method: `POST`
- Body: `{ url }`

**Redirect User**
- Endpoint: `/api/v1/redirect/:id`
- Method: `GET`
- Query param: `{ id }`

## High Level Design

<img width="2442" height="722" alt="image" src="https://github.com/user-attachments/assets/8a8d4643-40da-426a-8f98-1665a1c38e02" />

<img width="2302" height="1054" alt="image" src="https://github.com/user-attachments/assets/4a05bbe6-e66e-46b5-8269-b37a85264218" />

<img width="2062" height="1190" alt="image" src="https://github.com/user-attachments/assets/3f0ae58f-e1bc-4190-bd2b-e2c48730877e" />

## Deep Dive

**How will the unique URL IDs be created?**
- Base62 encoding

**Redirect 301 or 302?**
- Since we will go with analytics, we can use 302. If we aren't, we can use 301 since it will be cached in the user's browser (and the service would otherwise be meaningless).

**Scaling**
- Use Machine ID (prefix) as the shard key, and apply sharding to scale for higher loads.
