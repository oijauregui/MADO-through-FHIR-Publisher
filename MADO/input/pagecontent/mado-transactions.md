# MADO Transactions

## 3.1xy WADO-RS Get Instances [RAD-1xy]

### 3.1xy.1 Scope

This transaction is used by the Imaging Document Consumer to retrieve DICOM instances from the Imaging Document Source using WADO-RS (Web Access to DICOM Objects - RESTful Services).

### 3.1xy.2 Actor Roles

| Actor | Role |
|-------|------|
| Imaging Document Consumer | Requests DICOM instances using WADO-RS |
| Imaging Document Source | Provides DICOM instances in response to WADO-RS requests |

### 3.1xy.3 Referenced Standards

- **DICOM PS3.18**: Web Services
- **RFC 7230**: Hypertext Transfer Protocol (HTTP/1.1): Message Syntax and Routing
- **RFC 7231**: Hypertext Transfer Protocol (HTTP/1.1): Semantics and Content
- **RFC 7232**: Hypertext Transfer Protocol (HTTP/1.1): Conditional Requests
- **RFC 7233**: Hypertext Transfer Protocol (HTTP/1.1): Range Requests
- **RFC 7234**: Hypertext Transfer Protocol (HTTP/1.1): Caching
- **RFC 7235**: Hypertext Transfer Protocol (HTTP/1.1): Authentication

### 3.1xy.4 Messages

This transaction involves the following message exchanges:

1. Get Instances Request/Response
2. Get Rendered Instances Request/Response

#### 3.1xy.4.1 Get Instances Request Message

##### 3.1xy.4.1.1 Trigger Events

The Imaging Document Consumer triggers this message when:
- A manifest indicates the need to retrieve specific DICOM instances
- The consumer needs to access raw DICOM data for processing
- Clinical workflow requires access to original imaging data

##### 3.1xy.4.1.2 Message Semantics

The Get Instances Request follows the DICOM WADO-RS specification for instance retrieval.

**HTTP Method**: GET

**Request URL Format**:
```
{service}/studies/{studyUID}/series/{seriesUID}/instances/{instanceUID}
```

**Query Parameters**:
- `accept`: Specifies the acceptable media types for the response
- `transferSyntax`: Optionally specifies the desired transfer syntax

**HTTP Headers**:
- `Accept`: Specifies acceptable content types (e.g., `application/dicom`)
- `Authorization`: Contains authentication credentials if required

###### 3.1xy.4.1.2.1 Example of a Get Instances Request Message

```http
GET /wado-rs/studies/1.2.840.113619.2.55.3.604688119.971.1234567890.123/series/1.2.840.113619.2.55.3.604688119.971.1234567890.456/instances/1.2.840.113619.2.55.3.604688119.971.1234567890.789 HTTP/1.1
Host: example-pacs.hospital.org
Accept: application/dicom
Authorization: Bearer <token>
User-Agent: MADO-Client/1.0
```

##### 3.1xy.4.1.3 Expected Actions

The Imaging Document Consumer shall:
1. Construct a valid WADO-RS GET request URL
2. Include appropriate Accept headers for DICOM content
3. Handle authentication as required by the Imaging Document Source
4. Process the response according to HTTP status codes
5. Validate received DICOM instances

#### 3.1xy.4.2 Get Instances Response Message

##### 3.1xy.4.2.1 Trigger Events

The Imaging Document Source generates this response upon receiving a valid Get Instances Request.

##### 3.1xy.4.2.2 Message Semantics

**HTTP Status Codes**:
- `200 OK`: Instance successfully retrieved
- `206 Partial Content`: Partial instance content (if range requests are used)
- `400 Bad Request`: Invalid request parameters
- `401 Unauthorized`: Authentication required or failed
- `403 Forbidden`: Access denied
- `404 Not Found`: Requested instance not found
- `500 Internal Server Error`: Server error occurred

**Response Headers**:
- `Content-Type`: `application/dicom` or `multipart/related`
- `Content-Length`: Size of the response body
- `ETag`: Entity tag for caching support

**Response Body**: 
- DICOM instance data in the requested format

##### 3.1xy.4.2.3 Expected Actions

The Imaging Document Source shall:
1. Validate the request parameters
2. Authenticate the requester if required
3. Authorize access to the requested instance
4. Return the DICOM instance in the requested format
5. Generate appropriate audit logs

#### 3.1xy.4.3 Get Rendered Instances Request Message

##### 3.1xy.4.3.1 Trigger Events

The Imaging Document Consumer triggers this message when:
- Rendered imaging content is needed for display purposes
- The consumer requires images in web-friendly formats
- Clinical applications need processed/rendered image data

##### 3.1xy.4.3.2 Message Semantics

**HTTP Method**: GET

**Request URL Format**:
```
{service}/studies/{studyUID}/series/{seriesUID}/instances/{instanceUID}/rendered
```

**Query Parameters**:
- `accept`: Specifies acceptable media types (e.g., `image/jpeg`, `image/png`)
- `viewport`: Specifies the viewport dimensions
- `window`: Specifies window/level parameters
- `quality`: Specifies compression quality for JPEG

**HTTP Headers**:
- `Accept`: Specifies acceptable image formats
- `Authorization`: Contains authentication credentials if required

###### 3.1xy.4.3.2.1 Example of a Get Rendered Instances Request Message

```http
GET /wado-rs/studies/1.2.840.113619.2.55.3.604688119.971.1234567890.123/series/1.2.840.113619.2.55.3.604688119.971.1234567890.456/instances/1.2.840.113619.2.55.3.604688119.971.1234567890.789/rendered?viewport=512,512&window=350,40 HTTP/1.1
Host: example-pacs.hospital.org
Accept: image/jpeg
Authorization: Bearer <token>
User-Agent: MADO-Client/1.0
```

##### 3.1xy.4.3.3 Expected Actions

The Imaging Document Consumer shall:
1. Construct a valid WADO-RS GET request for rendered content
2. Specify appropriate rendering parameters
3. Handle various image format responses
4. Process rendered images for display or further processing

#### 3.1xy.4.4 Get Rendered Instances Response Message

##### 3.1xy.4.4.1 Trigger Events

The Imaging Document Source generates this response upon receiving a valid Get Rendered Instances Request.

##### 3.1xy.4.4.2 Message Semantics

**HTTP Status Codes**: Similar to Get Instances Response

**Response Headers**:
- `Content-Type`: Image media type (e.g., `image/jpeg`, `image/png`)
- `Content-Length`: Size of the image data

**Response Body**: 
- Rendered image data in the requested format

##### 3.1xy.4.4.3 Expected Actions

The Imaging Document Source shall:
1. Render the DICOM instance according to request parameters
2. Return the rendered image in the requested format
3. Apply appropriate image processing (windowing, scaling, etc.)
4. Generate audit logs for rendered content access

### 3.1xy.5 Protocol Requirements

- **Transport Protocol**: HTTP/1.1 or HTTP/2
- **Security**: TLS 1.2 or higher for secure communications
- **Authentication**: Support for standard HTTP authentication mechanisms
- **Content Types**: Support for `application/dicom` and standard image formats

### 3.1xy.6 Security Considerations

#### 3.1xy.6.1 Security Audit Considerations

All WADO-RS transactions shall be audited according to ATNA requirements:

- **Event ID**: DICOM Instance Retrieved
- **Event Action Code**: Read (R)
- **Event Outcome**: Success/Failure indication
- **User Identity**: Identity of the requesting user/system
- **Patient Identity**: Patient associated with retrieved instances
- **Study Identity**: Study UID of retrieved instances

#### 3.1xy.6.2 Imaging Document Consumer Specific Security Considerations

The Imaging Document Consumer shall:
- Implement secure authentication mechanisms
- Validate server certificates when using TLS
- Protect retrieved DICOM data according to organizational policies
- Implement appropriate access controls for manifest-driven retrieval

#### 3.1xy.6.3 Imaging Document Source Specific Security Considerations

The Imaging Document Source shall:
- Implement robust authentication and authorization
- Validate all request parameters to prevent injection attacks
- Log all access attempts and their outcomes
- Implement rate limiting to prevent abuse
- Ensure proper handling of patient privacy information
