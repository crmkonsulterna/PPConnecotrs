Verified is a Software as a Service provider of electronic identification and digital signing. This connector makes it possible to integrate to the API of Verified to create suffisticated signing flows within your application.

## Prerequisites
You will need the following to proceed:
* [Sign-up](https://verified.eu/en/) for a Verified account

## How to get credentials
This connector uses Basic Authentication. The user has to present username, password and company Id when creating a connection. Username and password are the same you use to login to the Verified portal.
To get the company Id you have to log in to the Verified portal, select the correct company if you have several and copy the company Id from the URL.

## Get started
A common use case is to start and manage a signing process, i.e. setting up automation for sending documents to be signed to Verified and getting signed documents back on a regular basis. Common actions that are used to start a Verified signing process include Create envelope - descriptor or Create envelope - default, Add document to envelope, Add file to document, Add recipient to envelope and Update publish status for envelope. Common actions that are used in order to check the Verified status for documents include Get envelope by id, Get all documents by envelopeid, Get files by documentid, Get file URL by id. Note that action Generate security token is always used as a first step in order to obtain a security token to be used in the following steps. 

## Known Issues and Limitations
The following limitations are currently known.
- In the beginning of every flow one has to create an authentication token to be reused in every following step.
- To add a document one has to send an HTTP request to a certain url.

## FAQ

### How do I upload a document to Verfied?
To Upload an actual document you have to create a file in the Verified API and upload the document hash to the blob URL responded from the Verified API.

### How do I get hold of the envelope after using the Get Envelop Id action?
The steps that create an envelop respond with an uid, which is a complete path to the envelop (example: "/envelopes/ABCDE"). For other steps one does only need the last part of the uid (which is the actual envelop Id, in our example "ABCDE"). 

Here is an example on how to do that

```
substring(outputs('Create_new_default_envelope')?['body/uid'],11)
```

### How do I get hold of the Document Id after using the Add document to envelope action? 
The steps that create a document respond with the Location of the new document in the header. The Location is the complete path to the document (example: "/api/envelopes/ABCDE/documents/FGHIJ"). Only the document Id (in our case: FGHIJ) is needed for following steps.

Here is an example on how to do that

```
substring(outputs('Add_document_to_envelope')?['headers/Location'], add(indexOf(outputs('Add_document_to_envelope')?['headers/Location'], '/documents/'), 11))
```

### How do I get hold of the Flow Id from an envelope?
When one loads information about an envelope the related flow will be part of the response (example: "/flow/default"). In the response it is called the id of a flow, however one needs only the last part when another step (like Get flow status) is asking for the flow id.

Here is an example on how to do that

```
substring(outputs('Get_envelope_by_id')?['body/flow/id'],6)
```

## Actions available
The connector supports the following actions:
* `Generate security token`: Generates an authentication Token from the Verified API.
* `Get company by id`: Gets the Company Information of a given company.
* `Get all envelope recipients`: Gets all recipients of a given envelope.
* `Add recipient to envelope`: Adds a recipient to a given envelope.
* `Get specific recipient for an envelope`: Gets information about a specific recipient of a given envelope.
* `Update recipient by id`: Updates a recipient.
* `Get file URL by id`: Gets the URL to a file.
* `Get user settings`: Gets user settings of given user.
* `Get envelope by id`: Gets envelope information of a given envelope.
* `Delete envelope by id`: Deletes an envelope.
* `Update envelope settings`: Updates settings of a given envelope.
* `Update publish status for envelope`: Updates the publish status of a given envelope.
* `Generate Document from Template`: Generates a document from a preconfigured template.
* `Set envelope abort status`: Aborts a given envelope.
* `Get files by document id`: Gets all files associated to a given document.
* `Add file to document`: Adds a file to a given document.
* `Get user information`: Gets user information of the currently logged in user.
* `Set document abort status`: Aborts a given document.
* `Get descriptor by id`: Gets envelope descriptor information of a given descriptor.
* `Set envelope trash status`: Sets trash status of a given envelope.
* `Query all envelopes`: Queries all envelopes in the current company.
* `Get all descriptors`: Gets all descriptors present in the current company.
* `Get flow state by envelopeid`: Gets the current flow state of a given envelope.
* `Get document by id`: Gets document information of a given document.
* `Delete document by id`: Deletes a document.
* `Create envelope - descriptor`: Creates a new envelope based on a specified descriptor.
* `Get all documents by envelopeid`: Gets all documents related to a given envelope.
* `Add document to envelope`: Adds a document to a given envelope.
* `Create signature link`: Creates a signature link for a given recipient on a given envelope.
* `Get file by id`: Gets file information of a given file.
* `Create envelope - default`: Creates a new envelope based on the default descriptor.
* `Get default descriptor`: Gets descriptor information of the default descriptor.
* `Send notification`: Sends a notification to a given recipient.
