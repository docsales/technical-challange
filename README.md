# DocSales tech test

## The challange

**Hello future Docster!**

Your mission now is to build an API with Ruby on Rails that generates PDFs from an HTML fragment
template with some placeholders to be substituted by data received when creating the PDF.

The API shall have two endpoints:

- GET /api/v1/documents/list
- POST/PUT /api/v1/documents/create

The first one (documents/list) will show all the previously created documents, their information and
metadata, as follows:
```javascript
// Example response of the GET /api/v1/documents/list request
[
  {
    "uuid": "2b2ab03a-8b81-47c1-9af3-3b3c8d695f71",
    "pdf_url": "presigned_url or local file path",
    "description": "Example description 1",
    "document_data": { // arbitrary data coming from the user
      "customer_name": "Tomás",
      "contract_value": "R$ 1.990,90",
      // ...
    },
    "created_at": "2012-04-23T18:25:43.511Z"
  },
    {
    "uuid": "d3b75481-3f8e-4a23-9c2e-738abf8e998b",
    "pdf_url": "presigned_url or local file path",
    "description": "Example description 2",
    "document_data": { // arbitrary data coming from the user
      "customer_name": "Haroldo",
      "contract_value": "R$ 10.990,90",
      // ...
    },
    "created_at": "2012-04-23T18:25:43.511Z"
  },
  // ...
]
```

The other endpoint should allow the creation of PDFs from HTML fragments with placeholders to substitute
the data from the `document_data` entry above. The request and the response should be as demonstrated
below:

```javascript
// Example request to the POST/PUT /api/v1/documents/create endpoint
{
  "description": "Example description 2",
  "document_data": { // arbitrary data coming from the user
    "customer_name": "Haroldo",
    "contract_value": "R$ 10.990,90",
    // ...
  },
  "template": "HTML Fragment template goes here, for brevity it is shown on template.html file"
}
```

```javascript
// Example response of the POST/PUT /api/v1/documents/create endpoint
{
  "uuid": "10a32fae-1c61-4b2f-b5c7-4de80f4d6f1d",
  "pdf_url": "presigned_url or local file path",
  "description": "Example description 2",
  "document_data": { // arbitrary data coming from the user
    "customer_name": "Haroldo",
    "contract_value": "R$ 10.990,90"
  },
  "created_at": "2012-04-23T18:25:43.511Z"
}
```

### Document creation

When the API is called to create a document with the above JSON, the following steps must be followed:

1. Substitute all placeholders in the HTML fragment
  - The placeholder is composed of a string inside two curly brackets `{{}}`. The string should correspond
  to a key in the `document_data` entry given on the request body. After that this placeholder should be
  substituted by the corresponding value from the key with same name as in the example below:

  ```html
  <p>CONTRATANTE: {{customer_name}} </p>
  ```

  Should become

  ```html
  <p>CONTRATANTE: Haroldo </p>
  ```

2. Generate the PDF
  - With the final HTML, the document should be converted to PDF using any tool available for this
  purpose. After that it should be persisted on cloud storage providers on production (e.g Amazon, Google Cloud Services, Azure), along with the other data used to create the document, as
  demonstrated above.
