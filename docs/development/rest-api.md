# REST API
- Routes should never contain verbs or action words (unless there is an entity named that way).  
- APIs should always be versioned.
- Publish API docs, like Open API and fully document parameters and operations.
- Use POST for read requests if there is sensitive data.  `GET` requests are cachable by proxies and other internest appliances.  If the URI contains sensitive data, if may exist in logs or cache several places, HTTP spec allows for using POST, where the request is contained in the body, for this purpose.

## Bad Examples
```
GET: /products/GetById/{id}
GET: /products/search
POST: /products/Update
```

## Good Examples
```
GET: /v1/products/{id} (read)
GET: /v1/products?name=searchTerm (nonsenstive search)
PUT: /v1/products/{id} (update)
POST: /v1/products/list (sensitive search)
POST: /v1/products (create)
```

- Use informative response codes

### Common Success Codes

- 200 - processed successful and contains response body.
- 201 - entity was created.
- 202 - request accepted/validated and queued for processing.
- 204 - processed successfully but no response body

#### Scenarios
- `PUT: /v1/products/13` -> `204`  You replaced the entity on the server with entity in the request body.  No need to return the entity, the client already has the latest copy.
- `POST: /v1/products` -> `201` You created an entity in response to the request.  Return the created entity and a `location` header where the client and send a `GET` to read it.
- `POST: /v1/products` -> `202` Request was validated and accepted but is queued to be processed by a background service.  It is also common to return a status check URI when returning a 202.