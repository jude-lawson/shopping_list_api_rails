# Shopping List API

## Summary

An api built for a shopping list interface. It currently supports:

- Creating a user
- Creating a new shopping list associated to a user
- Editing that shopping list by:
  - Adding an item
  - Removing an item
  - Editing an item
- View an item
- A user can be edited
- A user can be deleted
- Users can be added to a list (it's only owner will be its creator)

## Endpoints

### Users
#### Create a User
`POST /api/v1/users`

Creates a new user with the following attributes:
- First Name
- Last Name
- Email
- Username (can be null)

```javascript
{
  "first_name": "David",
  "last_name": "Davidson",
  "email": "ddavidson@notanemail.na.moc",
  "username": "ddavidson"
}
```

#### Edit a User
`PATCH /api/v1/users/:id`

Edits a particular field(s) for a user:

```javascript
{
  "first_name": "Jack"
}
```

#### Get a User
`GET /api/v1/users`

Returns the following JSON shape:

```javascript
{
  "id": "123456",
  "first_name": "David",
  "last_name": "Davidson",
  "email": "ddavidson@notanemail.moc",
  "username": "ddavidson"
}
```

#### Delete a User and all associated records
`DELETE /api/v1/users/:id`

Deletes a user's record and all associated records (lists and items):

Returns a 201 is successful with the message:

```javascript
{
  "message": "User 123456 successfully deleted"
}
```

### Lists
#### Create a List
`POST /api/v1/lists`

Creates a list record. Accepts the following attribtutes:

```javascript
{
  "title": "Thanksgiving Day Shopping List"
}
```

Returns a 200 if successful and the following JSON shape:

```javascript
{
  "id": "123",
  "title": "Thanksgiving Day Shopping List",
  "created_at": datetime in ISO8601,
  "updated_at": datetime in ISO8601,
  "owner_id": "123456" (a user id)
}
```

#### Edit a List
`PATCH /api/v1/lists/:id`

Edits the list specified by the id. Accepts the following JSON parameter:

```javascript
{
  "title": "Christmas Day Shopping List"
}
```

#### Get a List
`GET /api/v1/lists/:id`

Get the list specified by id:

```javascript
{
  "id": "123",
  "title": "Christmas Day Shopping List",
  "created_at": datetime in ISO8601,
  "updated_at": datetime in ISO8601,
  "owner_id": "123456",
  "associated_users": [Array of users including owner]
}
```

#### Get all lists owned by a user
`GET /api/v1/users/:id/owned-lists`

Returns a JSON array of all of the shopping lists owned by a user:

```javascript
{
  "lists": [
    {
      "id" "123",
      "title": "Christmas Day Shopping List",
      "created_at": datetime in ISO8601,
      "updated_at": datetime in ISO8601
    },
    {
      "id": "456",
      "title": "Thanksgiving Day",
      "created_at": datetime in ISO8601,
      "updated_at": datetime in ISO8601,
    }
  ]
}
```

#### Get all lists associated with a user
`GET /api/v1/users/:id/all-lists`

Returns a JSON array of all lists associated with a user whether it is owner by them or not:

```javascript
{
  "lists": [
    {
      "id" "123",
      "title": "Christmas Day Shopping List",
      "created_at": datetime in ISO8601,
      "updated_at": datetime in ISO8601
    },
    {
      "id": "456",
      "title": "Thanksgiving Day",
      "created_at": datetime in ISO8601,
      "updated_at": datetime in ISO8601,
    }
  ]
}
```

#### Delete a list
`DELETE /api/v1/list/:id`

Destroys the list record and user associations, but leaves items in database. Returns a 201 if successful with the following message:

```javascript
{
  "message": "List 'Thanksgiving Day' successfully deleted"
}
```

### Items
#### Create an item
`POST /api/v1/users/:id/items`

Creates an item record. Accepts the following JSON payload:

```javascript
{
  "name": "butter",
  "quantity": "12",
  "unit_price": "300",
  "list_id": "123",
  "owner_id": "123456"
}
```

#### Edit an item
`PATCH /api/v1/users/:id/items/:item_id`

Edits the item specified by `item_id`. Accepts an supported parameter given:

```javascript
{
  "name": "maragarine"
}
```

#### Delete an item
`DELETE /api/v1/users/:id/item/item_id`

Removes the item with the specified id. Returns a 201 with the following message if successful:

```javascript
{
  "message": "Item 'Margarine' successfully deleted"
}
```

