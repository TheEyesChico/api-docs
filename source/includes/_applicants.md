# Applicants

## About applicants

An applicant represents an individual who will be the subject of [a check](). An applicant must exist before a check can be initiated.

## Applicant object 

> An example applicant object

```http
HTTP/1.1 201 Created
Content-Type: application/json
```
> <h3 style="color:white;text-align:center">RESPONSE</h3>

```json
{
  "id": "<APPLICANT_ID>",
  "created_at": "2020-10-09T17:52:42Z",
  "first_name": "Jane",
  "last_name": "Doe",
  "email": "JaneDoe@xyz.com",
  "dob": "1990-01-01",
  "delete_at": null,
  "href": "/applicants/<APPLICANT_ID>",
  "id_numbers": [],
  "address": {
    "flat_number": null,
    "building_number": null,
    "street": "Second Street",
    "town": "London",
    "postcode": "S2 2DF",
    "country": "GBR",
  }
}
```

**Attribute** | **Description**
------------- | ---------------
id            |    **string** <br> The unique identifier for the applicant
created_at    |    **datetime** <br> The date and time when this applicant was created
delete_at    |    **datetime** <br> The date and time when this applicant is scheduled to be deleted, or `null` if the applicant is not scheduled to be deleted
href    |    **string** <br> The URI of this resource
first_name    |    **string** <br> The applicant’s first name
last_name   |    **string** <br> The applicant’s surname
email   |    **string** <br> The applicant’s email address
dob   |    **date** <br> The applicant’s date of birth in yyyy-mm-dd format
id_numbers   |    **array of id number objects** <br> A collection of identification numbers belonging to this applicant
address   |    **[address](#address-object) object** <br> The address of the applicant


### ID NUMBER

**Attribute** | **Description**
------------- | ---------------
type          |    **string** <br> Type of ID number. Valid values are `ssn`, `social_insurance` (e.g. UK National Insurance), `tax_id`, `identity_card` and `driving_licence`
value         |    **string** <br> Value of ID number <br> Note that `ssn` supports both the full SSN or the last 4 digits. If providing the full SSN the value has to be supplied with dashes in the following format: xxx-xx-xxxx
state_code    |    **string** <br> Two letter code of issuing state (state-issued driving licences only) 

<aside class="notice">
The API uses the spelling “driving_licence”
</aside>

## Address object
**Attribute** | **Description**
------------- | ---------------
flat_number            |    **string** <br> The flat number
building_number            |    **string** <br> The building number
building_name           |    **string** <br> The building name
street            |    **string** <br> The street of the applicant’s address. There is a 32-character limit on this field for UK addresses
sub_street            |    **string** <br> The sub-street
town            |    **string** <br> The town
state            |    **string** <br> The address state. US states must use the USPS abbreviation (see also [ISO 3166-2:US](https://www.iso.org/obp/ui/#iso:code:3166:US)), for example `AK`, `CA`, or `TX`.
postcode            |    **string** <br> The postcode or ZIP of the applicant’s address. For UK postcodes, specify the value in the following format: `SW4` `6EH`
country           |    **string** <br> The 3 character ISO country code of this address. For example, `GBR` is the country code for the United Kingdom
line1            |    **string** <br> Line 1 of the address

The applicant address object is nested inside the address object. <br><br>
`postcode` and `country` are required fields if an address is provided for an applicant. For US addresses, `state` is also a required field.<br><br>
Most addresses will contain other information such as `flat_number`. Please make sure they are supplied as separate fields, and do not try and fit them all into the `street` field. Doing so is likely to affect check performance. Alternatively, you can provide addresses in the form `line1`, `line2` and `line3`.<br>

## Forbidden characters

For addresses the following characters are forbidden:

`!$%^*=<>`

For names the following characters are forbidden:

`^!#$%*=<>;{}"`

## Create applicant

<span class="post">POST</span><span class="url">curl https://api.xyz.com/applicants</span>

> Create an applicant

```http
POST /v3/applicants/ HTTP/1.1
Host: api.xyz.com
Authorization: Token token=<YOUR_API_TOKEN>
Content-Type: application/json
```

```shell
$ curl https://api.xyz.com/applicants
  -H 'Authorization: Token token=<YOUR_API_TOKEN>' 
  -H 'Content-Type: application/json' 
  -d '{
  "first_name": "Jane",
  "last_name": "Doe",
  "dob": "1990-01-31",
  "address": {
    "building_number": "100",
    "town": "London",
    "country": "GBR"
  }
}'
```

```python
applicant_address = user.Address(
    building_number=100,
    town='London',
    country='GBR'
)

applicant_details = user.Applicant(
    first_name='Jane',
    last_name='Doe',
    dob="1990-01-31",
    address=applicant_address
)

api_instance.create_applicant(applicant_details)
```

```php
$applicantDetails = new xyz\Model\Applicant();

$applicantDetails->setFirstName('Jane');
$applicantDetails->setLastName('Doe');
$applicantDetails->setDob('1990-01-31');

$address = new \xyz\Model\Address();
$address->setBuilding_number('100');
$address->setTown('London');
$address->setCountry('GBR');

$applicantDetails->setAddress($address);

$xyz->createApplicant($applicantDetails);
```

```java
import com.website.xyz.models.Address;
import com.website.xyz.models.Applicant;

Address.Request addressRequest = Address.request()
        .building_number("100")
        .town("London")
        .country("GBR");

Applicant.Request applicantRequest = Applicant.request()
        .firstName("Jane")
        .lastName("Doe")
        .dob(LocalDate.parse("1990-01-31"))
        .address(addressRequest);

Applicant applicant = xyz.applicant.create(applicantRequest);
```
> <h3 style="color:white;text-align:center">RESPONSE</h3>

```json
{
  "first_name": "Jane",
  "last_name": "Doe",
  "dob": "1990-01-31",
  "address": {
    "building_number": "100",
    "town": "London",
    "country": "GBR"
  }
}
```

<aside class="notice">
Using this endpoint in a live context will cause you to send personal data to XYZ website. Always make sure you inform your users about this and obtain any necessary permissions. For more information on how we process uses personal data, view our <a href="">Privacy Policy</a>
</aside>

Creates a single [applicant.](#about-applicants) Returns an [applicant object](#applicant-object).
<br><br>
When you create an applicant, [some characters are forbidden](#forbidden-characters).
<br><br>
Required applicant data differs depending on report type. For example, for [Document reports]().
<br>
### REQUEST BODY PARAMETERS
|                   |               |
|-------------------|---------------|
| first_name      | **required** <br> The applicant’s forename.
| last_name             | **required** <br> The applicant’s surname.
| email             |  **required only if creating a check where**<br> `applicant_provides_data` is `true` <br> The applicant’s email address.
|dob           |  **optional**<br> The applicant’s date of birth in yyyy-mm-dd format.
| id_numbers	   |  **optional**<br> A collection of identification numbers belonging to this applicant
|address   |  **optional**<br> The address of the applicant

## Retrieve applicant

> Retrieve an applicant

```http
GET /v3/applicants/<APPLICANT_ID> HTTP/1.1
Host: api.website.com
Authorization: Token token=<YOUR_API_TOKEN>
```

```shell
$ curl https://api.xyz.com/<APPLICANT_ID>
  -H 'Authorization: Token token=<YOUR_API_TOKEN>'
```

```python
api_instance.find_applicant('<APPLICANT_ID>')
```

```php
$xyz->findApplicant('<APPLICANT_ID>');
```

```java
import com.xyz.models.Applicant;

String applicantId = "<APPLICANT_ID>";

Applicant applicant = xyz.applicant.find(applicantId);
```

<span class="get">GET</span><span class="url">https://api.xyz.com/applicants/{applicant_id}</span>
<br><br>
Retrieves a single [applicant](#about-applicants). Returns an [applicant object](#applicant-object).

## Update applicant

> Update an applicant

```http
PUT /v3/applicants/<APPLICANT_ID> HTTP/1.1
Host: api.xyz.com
Authorization: Token token=<YOUR_API_TOKEN>
Content-Type: application/json
```

```shell
$ curl curl https://api.xyz.com/<APPLICANT_ID>/update
  -H 'Authorization: Token token=<YOUR_API_TOKEN>' 
  -H 'Content-Type: application/json' 
  -d '{
  "first_name": "New",
  "last_name": "Name"
}' 
  -X PUT
```

```python
new_applicant_details = xyz.Applicant()

new_applicant_details.first_name = 'New'
new_applicant_details.last_name = 'Name'

api_instance.update_applicant('<APPLICANT_ID>', new_applicant_details)
```

```php
$newApplicantDetails = new xyz\Model\Applicant();
$newApplicantDetails->setFirstName('New');

$xyz->updateApplicant('<APPLICANT_ID>', $newApplicantDetails);
```

```java
import com.xyz.models.Applicant;

Applicant.Request newApplicantDetails = Applicant.request()
        .firstName("New")
        .lastName("Name");

String applicantId = "<APPLICANT_ID>";

Applicant applicant = xyz.applicant.update(applicantId, newApplicantDetails);
```
> <h3 style="color:white;text-align:center;">RESPONSE</h3>

```json
{
  "first_name": "New",
  "last_name": "Name"
}
```
<span class="put">PUT</span><span class="url">https://api.xyz.com/v3/applicants/{applicant_id}</span>
<br>
<aside class="notice">
Using this endpoint in a live context will cause you to send personal data to XYZ website. Always make sure you inform your users about this and obtain any necessary permissions. For more information on how we use personal data, view our [Privacy Policy]().
</aside>

Updates an applicant’s information. Returns the updated [applicant object](#applicant-object).
<br>

* Partial updates
* Addresses and ID numbers present will replace existing ones
* Same applicant validations to create applicant

### REQUEST BODY PARAMETERS
|                   |               |
|-------------------|---------------|
| first_name      | The applicant’s forename.
| last_name             | The applicant’s surname.
| email             |  The applicant’s email address.
|dob           |  The applicant’s date of birth in yyyy-mm-dd format.
| id_numbers	   |  A collection of identification numbers belonging to this applicant
|address   |  The address of the applicant

## Delete applicant

> Delete an applicant

```http
DELETE /v3/applicants/<APPLICANT_ID> HTTP/1.1
Host: api.xyz.com
Authorization: Token token=<YOUR_API_TOKEN>
```

```shell
$ curl curl https://api.xyz.com/<APPLICANT_ID>/delete
  -H 'Authorization: Token token=<YOUR_API_TOKEN>' 
  -X DELETE
```

```python
api_instance.destroy_applicant('<APPLICANT_ID>')
```

```php
$xyz->destroyApplicant('<APPLICANT_ID>');
```

```java
import com.xyz.models.Applicant;

String applicantId = "<APPLICANT_ID>";

xyz.applicant.delete(applicantId);
```

<span class="delete">DELETE</span><span class="url">https://api.xyz.com/v3/applicants/{applicant_id}</span>
<br><br>
Deletes a single applicant. If successful, returns a `204 No Content` response. 
<br><br>
Sending a deletion request adds the applicant object and all associated documents, photos, videos, checks, reports and analytics data to our deletion queue. The objects will be permanently deleted from our production object storage and relational database system, after a deletion delay which can be configured by [emailing our client support team](). After deletion, applicant details cannot be recovered or queried, and we will not be able to troubleshoot. Within the delay period, the applicant can be restored. For more information about our deletion service, visit [Data Deletion FAQ]().

<aside class="notice">
Deleting an applicant that does not have an associated check will immediately delete the applicant and any resources.
</aside>

## Restore applicant

> Restore an applicant

```http
POST /v3/applicants/<APPLICANT_ID>/restore HTTP/1.1
Host: api.xyz.com
Authorization: Token token=<YOUR_API_TOKEN>
```

```shell
$ curl curl https://api.xyz.com/<APPLICANT_ID>/restore 
  -H 'Authorization: Token token=<YOUR_API_TOKEN>' 
  -X POST
```

```python
api_instance.restore_applicant('<APPLICANT_ID>')
```

```php
$xyz->restoreApplicant('<APPLICANT_ID>');
```

```java
import com.xyz.models.Applicant;

String applicantId = "<APPLICANT_ID>";

xyz.applicant.restore(applicantId);
```
<span class="post">POST</span><span class="url">https://api.xyz.com/v3/applicants/{applicant_id}/restore</span>
<br><br>
Restores a single applicant scheduled for deletion. If successful, returns a `204 No Content` response.
<br><br>
A restore request will also restore all associated documents, photos, videos, checks, reports and analytics data.
<br><br>
Applicants that have been permanently deleted cannot be restored.

##  List applicants

> Return all applicants you’ve created

```http
GET /v3/applicants/ HTTP/1.1
Host: api.xyz.com
Authorization: Token token=<YOUR_API_TOKEN>
```

```shell
$ curl curl https://api.xyz.com/<APPLICANT_ID> 
  -H 'Authorization: Token token=<YOUR_API_TOKEN>'
```

```python
api_instance.list_applicants(page=1, per_page=20, include_deleted=True)
```

```php
$xyz->listApplicants();
```

```java
import com.xyz.models.Applicant;

Integer page = 1;
Integer perPage = 20;
Boolean includeDeleted = false; 

List<Applicant> applicants = xyz.applicant.list(page, perPage, includeDeleted);
```
<span class="get">GET</span><span class="url">https://api.xyz.com/v3/applicants/</span>
<br><br>
Lists all applicants you’ve created, sorted by creation date in descending order.
<br><br>
Returns data in the form: <br> `{"applicants": [<LIST_OF_APPLICANT_OBJECTS>]}.`
<br><br>
Requests to this endpoint will be paginated to 20 items by default.

### QUERY STRING PARAMETERS

`include_deleted=true` (optional): include applicants scheduled for deletion.
<br><br>
`per_page` (optional): set the number of results per page. Defaults to 20.
<br><br>
`page` (optional): return specific pages. Defaults to 1.

### LINK HEADER

The `Link` header contains pagination information.<br>
For example:
<br><br>
`Link: https://api.xyz.com/applicants?page=3059>; rel="last", <https://api.xyz.com/v3/applicants?page=2>; rel="next"`
<br><br>
Possible `rel` values are:
<br>

| **Name** |**Link relation (description)**
|----------|---------------------------------
|`next`    |    Next page of results
|`last`    |    Last page of results
|`first`   |    First page of results
|`prev`    |    Previous page of results
<br>
The custom `X-Total-Count` header gives the total resource count.
<br><br>
You can read more about `Link` headers [on the IETF website](https://tools.ietf.org/html/rfc5988).
