# Documents

## About documents

In the Onfido API, documents belong to a single applicant, so they must be uploaded after [an applicant](http://cw.sparshik.com:8006/#applicants) has been created.<br><br>
Some reports require identity documents in order to be processed successfully. For example, [Document reports]().
<aside class="notice">
Depending on the type of the document, we may require both sides of the image to be uploaded. See the [full list of documents we support](). 
</aside>
## Document object

The example given is for a driving licence document.

**Attribute** | **Description**
------------- | ---------------
id            |    **string** <br> The unique identifier of the document
created_at            |    **datetime** <br> The date and time at which the document was uploaded
href           |    **string** <br> The URI of this resource
download_href	         |    **string** <br> The URI that can be used to download the document
file_name         |    **string** <br> The name of the uploaded file
file_type           |    **string** <br> The file type of the uploaded file
file_size            |    **integer** <br> The size of the file in bytes
type            |    **string** <br> The type of document. The possible values are detailed [in the “Document types” section.]()
side            |    **string** <br> The side of the document, if applicable. The possible values are *front* and *back*
issuing_country            |    **string** <br> The issuing country of the document, in 3-letter ISO code, specified when uploading it.
applicant_id           |    **string** <br> The id of the applicant to whom the document belongs.

## Document types
### IDENTITY DOCUMENTS
The following is a non-exhaustive list of document types as they’re described in the Onfido API:

       **Type**          | 
-------------------------| 
`national_identity_card` |
`driving_licence`        |
`passport`               |
`voter_id`               |
`work_permit`            |

For some document types, both sides are required for processing.
<br>
<aside class="notice">
For some document types, both sides are required for processing. You can review a [full list of documents we support.]()
</aside>

If you’re unsure of the type of document you want to verify, you can submit documents with type `unknown.` In this case, we will attempt to classify and recognize the document type when processing a Document report.

## Upload document
<span class="get">GET</span><span class="url">https://api.onfido.com/v3/documents/</span>  
<aside class="notice">
Using this endpoint in a live context will cause you to send personal data to Onfido. Always make sure you inform your users about this and obtain any necessary permissions. For more information on how Onfido uses personal data, view our [Privacy Policy.]()
</aside>

Uploads a [document]() as part of a multipart request. Returns a [document object.](http://cw.sparshik.com:8006/#document-object)  
<br><br>
We provide a [sample document]() to test this endpoint.
<br><br>
Valid file formats for documents are `jpg`, `png` and `pdf`.<br> The file size must be between 32KB and 10MB.<br>

### REQUEST BODY PARAMETERS
|                   |               |
|-------------------|---------------|
| applicant_id      | **required** <br> The ID of the applicant who owns the document
| file              | **required** <br> The file to be uploaded
| type              |  **required**<br> The [type of document.](http://cw.sparshik.com:8006/#document-types) For example, `passport`
| side              |  **optional**<br> Either the `front` or `back` of the document
| issuing_country   |  **optional**<br> The issuing country of the document in 3-letter ISO code

## Retrieve document
<span class="get">GET</span><span class="url">https://api.onfido.com/v3/documents/{document_id}</span>
<br><br>
Retrieves a single document. Returns the corresponding [document object.](http://cw.sparshik.com:8006/#document-object)
## List documents
<span class="get">GET</span><span class="url">https://api.onfido.com/v3/documents?applicant_id={applicant_id}</span>
<br>
Lists all documents belonging to an applicant.<br><br>
Returns data in the form:<br> `{"documents": [<LIST_OF_DOCUMENT_OBJECTS>]}.`<br><br>

<p style="font-size:14px">QUERY STRING PARAMETERS</p>

`applicant_id` (required): the ID of the applicant ID whose documents you want to list.

## Download document
<span class="get">GET</span><span class="url">https://api.onfido.com/v3/documents?applicant_id={applicant_id}</span>
<br>Downloads specific documents belong to an applicant.<br>

If successful, the response will be the binary data representing the image.

