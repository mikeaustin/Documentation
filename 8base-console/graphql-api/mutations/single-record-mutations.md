*For the sake of the following examples, let's consider a scenario where a table called `Posts` exists, having expected fields and relations like `title`, `body`, `author`, etc.*

### Mutating individual records
You can create, update, and delete individual table records using 8base's auto-generated GraphQL mutation operations.

***Note**: Some examples will use [aliases](../README.md) to show side by side the use of `data.id` versus `filter`. All examples work without aliases.*

##### Creating a single record
Create a new record using the data argument that defines the records data. 

{% code-tabs %}
{% code-tabs-item title="Query" %}
```javascript
mutation {
  authorCreate(data: {
    name: "Wyatt"
  }) {
    id
    name
  }
}
```
{% endcode-tabs-item %}

{% code-tabs-item title="Result" %}
```json
{
  "data": {
    "authorCreate": {
      "id": "ck0d12w8e01c001l1dtxz5b7f",
      "name": "Wyatt"
    }
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

##### Updating a single record
Update a record using the data argument while including the record's `id` OR using a `filter` that includes a unqiue field.

{% code-tabs %}
{% code-tabs-item title="Query" %}
```javascript
mutation {
  /* Updates record name with find by unqiue name) */
  quade: authorUpdate(filter: {
    name: "Quade"
  },
  data: {
    name: "PenPossum"
  }) {
    id
    name
  }
  
  /* Updates record with (find by id) */
  wyatt: authorUpdate(data: {
    id: "ck0d12w8e01c001l1dtxz5b7f",
    name: "Hyatt"
  }) {
    id
    name
  }
}
```
{% endcode-tabs-item %}

{% code-tabs-item title="Result" %}
```json
{
  "data": {
    "quade": {
      "id": "ck0d12nf001bu01l15skw13pg",
      "name": "PenPossum"
    },
    "wyatt": {
      "id": "ck0d12w8e01c001l1dtxz5b7f",
      "name": "Hyatt"
    }
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

##### Delete a single record
Delete a record using the data argument while including the record's `id` OR using a `filter` that includes a unqiue field.

{% code-tabs %}
{% code-tabs-item title="Query" %}
```javascript
mutation {
  /* Deletes record by unqiue field. */
  quade: authorDelete(filter: {
    name: "PenPossum"
  }) {
    success
  }
  
  /* Deletes record by id. */
  wyatt: authorDelete(data: {
    id: "ck0d12w8e01c001l1dtxz5b7f",
  }) {
    success
  }
}
```
{% endcode-tabs-item %}

{% code-tabs-item title="Result" %}
```json
{
  "data": {
    "quade": {
      "success": true
    },
    "wyatt": {
      "success": true
    }
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

When handling delete mutations, an additional force parameter can be specified in the data object that accepts a Boolean value—the default value is `false`. When set to `true`, it will force a cascading delete on the record. This means that if the record being deleted is a parent in a mandatory relationship with child records, all child records will be deleted as well.