# Manifest-based Access to DICOM Objects (MADO) Profile

## 59.1 MADO Actors, Transactions, and Content Modules

The MADO profile defines the following actors, transactions, and content modules for manifest-based access to DICOM objects.

### Actors

The MADO profile includes the following actors:

- **Content Creator**: Creates and manages imaging manifests
- **Content Consumer**: Consumes and processes imaging manifests  
- **Imaging Document Consumer**: Retrieves imaging documents based on manifest information
- **Imaging Document Source**: Provides imaging documents for retrieval

### Transactions

The MADO profile defines the following transactions:

- **Convey Manifest Content [RAD-TBD]**: Transaction for conveying manifest content between Content Creator and Content Consumer
- **WADO-RS Get Instances [RAD-1xy]**: Transaction for retrieving DICOM instances

### Content Modules

The profile defines content modules for:

- DICOM Key Object Selection (KOS) based manifests
- FHIR-based manifests
- Mapping specifications between DICOM and FHIR formats

## Actor Descriptions and Requirements

### Content Creator

The Content Creator actor is responsible for:

- Creating imaging manifests in either DICOM KOS format or FHIR format
- Managing manifest content and metadata
- Conveying manifest content to Content Consumer actors

**Profile Requirements**: The Content Creator shall support at least one of the manifest format options (DICOM KOS or FHIR).

### Content Consumer  

The Content Consumer actor is responsible for:

- Receiving and processing imaging manifests
- Interpreting manifest content for subsequent imaging retrieval operations
- Supporting both DICOM KOS and FHIR manifest formats as applicable

**Profile Requirements**: The Content Consumer shall be grouped with an Imaging Document Consumer for actual image retrieval.

### Imaging Document Consumer

The Imaging Document Consumer actor is responsible for:

- Issuing WADO-RS retrieve transactions based on manifest information
- Managing the retrieval of DICOM instances and rendered content
- Processing retrieved imaging content

**Profile Requirements**: This actor shall be grouped with a Content Consumer actor.

### Imaging Document Source

The Imaging Document Source actor is responsible for:

- Responding to WADO-RS retrieve requests
- Providing DICOM instances and rendered imaging content
- Supporting various retrieve options and formats

## MADO Actor Options

### Option Name

*[Options to be defined based on specific implementation requirements]*

## MADO Required Actor Groupings

The following actor groupings are required:

- **Content Consumer** shall be grouped with **Imaging Document Consumer**
- Additional groupings may be defined based on implementation scenarios

## MADO Overview

### Concepts

#### Intra-community Sharing Infrastructure

The MADO profile operates within intra-community sharing infrastructures to enable efficient access to imaging studies through manifest-based approaches.

#### Imaging Reports

The profile considers the relationship between imaging manifests and associated imaging reports, enabling comprehensive access to imaging-related information.

#### DICOMweb Study Service Retrieve Transaction URI

The profile leverages DICOMweb standards for study service retrieve operations, providing standardized URIs for accessing imaging content.

### Use Cases

#### Use Case #1: DICOM Instance Retrieval

##### Instance Retrieval Use Case Description

This use case describes the process of retrieving DICOM instances based on manifest information.

**Actors Involved**:
- Content Creator
- Content Consumer  
- Imaging Document Consumer
- Imaging Document Source

**Basic Flow**:
1. Content Creator generates an imaging manifest
2. Content Consumer receives and processes the manifest
3. Imaging Document Consumer uses manifest information to request specific DICOM instances
4. Imaging Document Source provides the requested instances

##### Instance Retrieval Process Flow

###### Pre-conditions

- Imaging studies are available in the Imaging Document Source
- Content Creator has access to study metadata for manifest creation
- Network connectivity exists between all actors

###### Main Flow

1. **Manifest Creation**: Content Creator creates an imaging manifest containing references to relevant DICOM instances
2. **Manifest Transmission**: Content Creator transmits the manifest to Content Consumer via Convey Manifest Content transaction
3. **Manifest Processing**: Content Consumer processes the manifest and extracts instance reference information
4. **Instance Retrieval**: Imaging Document Consumer issues WADO-RS Get Instances requests to Imaging Document Source
5. **Instance Delivery**: Imaging Document Source responds with requested DICOM instances

###### Post-conditions

- Requested DICOM instances are successfully retrieved
- Content Consumer has access to both manifest information and actual imaging content
- Audit logs are generated as appropriate

## MADO Security Considerations

The MADO profile shall address the following security considerations:

- Authentication and authorization for manifest access
- Secure transmission of manifest content
- Access control for imaging document retrieval
- Audit logging of manifest and retrieval operations
- Privacy protection for patient information in manifests

## MADO Cross Profile Considerations

The MADO profile may interact with other IHE profiles including:

- **XDS/XCA**: For document sharing infrastructure
- **ATNA**: For audit and security requirements
- **PIX/PDQ**: For patient identity management
- **XUA**: For user authentication assertions

Additional considerations for cross-profile interactions will be defined based on implementation requirements.
