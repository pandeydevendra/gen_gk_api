# Gen GK API — Endpoint Reference

Base URL (local dev): `http://127.0.0.1:8000`
Base URL (Railway deployment): replace with your deployed URL, e.g. `https://your-app.up.railway.app`

Run locally with:

```bash
uvicorn main:app --reload
```

---

## `GET /`

Health-check / welcome message.

**curl**

```bash
curl -X GET http://127.0.0.1:8000/
```

**Response**

```json
{
  "message": "Hello Railway!"
}
```

---

## `GET /about`

Basic metadata about the app.

**curl**

```bash
curl -X GET http://127.0.0.1:8000/about
```

**Response**

```json
{
  "app": "FastAPI Demo",
  "status": "Running"
}
```

---

## `GET /capital/{country}`

Returns the capital city for a given country. The `country` path parameter is
case-insensitive; multi-word country names should be URL-encoded (spaces as
`%20` or `+`, or simply wrap the URL in quotes as shown below).

**curl — success case**

```bash
curl -X GET http://127.0.0.1:8000/capital/india
```

**Response**

```json
{
  "country": "india",
  "capital": "New Delhi"
}
```

**curl — multi-word country name**

```bash
curl -X GET "http://127.0.0.1:8000/capital/united%20kingdom"
```

**Response**

```json
{
  "country": "united kingdom",
  "capital": "London"
}
```

**curl — case-insensitive lookup**

```bash
curl -X GET http://127.0.0.1:8000/capital/JAPAN
```

**Response**

```json
{
  "country": "JAPAN",
  "capital": "Tokyo"
}
```

**curl — unknown country (404)**

```bash
curl -i -X GET http://127.0.0.1:8000/capital/atlantis
```

**Response**

```
HTTP/1.1 404 Not Found

{
  "detail": "Capital for 'atlantis' not found"
}
```

---

## Notes

- All endpoints are `GET` only; no request body or auth is required.
- The `capitals` dict in [main.py](main.py) covers ~190 UN member states (and a
  few commonly referenced territories/observers such as Vatican City,
  Palestine, Taiwan, Kosovo). Keys are lowercase; lookups lowercase the input
  before matching, so casing in the URL doesn't matter.
- For countries with multi-word names, either URL-encode spaces (`%20`) or
  wrap the full URL in quotes in your shell so it isn't split into multiple
  arguments.
