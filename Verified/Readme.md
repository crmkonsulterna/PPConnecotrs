## Verified Connector
Verified is a Software as a Service provider of electronic identification and digital signing. This connector makes it possible to integrate to the API of Verified and create simple and intuitive signing processes within your application.

## Publisher: CRM Konsulterna i Sverige AB
The publisher for this connector is [CRM Konsulterna i Sverige AB](https://crmkonsutlerna.se) on behalf of [Verified](https://verified.eu).

## Prerequisites
You will need the following to proceed:
* [Sign-up](https://verified.eu/en/) for a Verified account

## How to get credentials
This connector uses Basic Authentication. The user has to present username, password and company Id when creating a connection. Username and password are the same you use to login to the Verified portal.
To get the company Id you have to log in to the Verified portal, select the correct company if you have several and copy the company Id from the URL.

## Get started
A common use case is to start and manage a signing process, i.e. setting up automation for sending documents to be signed to Verified and getting signed documents back on a regular basis. Common actions that are used to start a Verified signing process include Create envelope - descriptor or Create envelope - default, Add document to envelope, Add file to document, Add recipient to envelope and Update publish status for envelope. Common actions that are used in order to check the Verified status for documents include Get envelope by id, Get all documents by envelope id, Get files by document id, Get file URL by id. Note that action Generate security token is always used as a first step in order to obtain a security token to be used in the following steps. 

## Known Issues and Limitations
The following limitations are currently known.
- In the beginning of every flow one has to create an authentication token to be reused in every following step.
- To add a document one has to send an HTTP request to a certain url.

## FAQ

### How do I upload a document to Verified?
To Upload an actual document you have to create a file in the Verified API and upload the document hash to the blob URL responded from the Verified API.

### How do I get hold of the envelope after using the Get Envelope id action?
The steps that create an envelope respond with an uid, which is a complete path to the envelope (example: "/envelopes/ABCDE"). For other steps one does only need the last part of the uid (which is the actual envelope id, in our example "ABCDE"). 

Here is an example on how to do that

```
substring(outputs('Create_new_default_envelope')?['body/uid'],11)
```

### How do I get hold of the document id after using the Add document to envelope action? 
The steps that create a document respond with the Location of the new document in the header. The Location is the complete path to the document (example: "/api/envelopes/ABCDE/documents/FGHIJ"). Only the document id (in our case: FGHIJ) is needed for following steps.

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
