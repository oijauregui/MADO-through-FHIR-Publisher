# MADO Content Modules

## 5 IHE Namespaces, Concept Domains and Vocabularies

### 5.1 IHE MADO Namespaces

The MADO profile defines the following namespaces:

| Namespace | URI | Description |
|-----------|-----|-------------|
| MADO | `http://ihe.net/fhir/mado` | Base namespace for MADO FHIR resources |
| MADO-KOS | `http://ihe.net/mado/dicom/kos` | Namespace for DICOM KOS manifest definitions |

### 5.2 IHE MADO Concept Domains

The following concept domains are defined for the MADO profile:

| Concept Domain | Description | Binding |
|----------------|-------------|---------|
| ManifestType | Types of imaging manifests | Required |
| RetrievalMethod | Methods for retrieving imaging content | Extensible |
| StudyPurpose | Purpose codes for imaging studies | Preferred |

### 5.3 IHE MADO Format Codes and Vocabularies

#### 5.3.1 IHE Format Codes

| Format Code | Description | MIME Type |
|-------------|-------------|-----------|
| urn:ihe:mado:kos:2025 | DICOM Key Object Selection Manifest | application/dicom |
| urn:ihe:mado:fhir:2025 | FHIR-based Imaging Manifest | application/fhir+json |

#### 5.3.2 IHEActCode Vocabulary

| Act Code | Display Name | Definition |
|----------|--------------|------------|
| MADO-MANIFEST | Imaging Manifest | A manifest document that references imaging studies and instances |
| MADO-RETRIEVE | Manifest-based Retrieval | Retrieval of imaging content based on manifest information |

#### 5.3.3 IHERoleCode Vocabulary

| Role Code | Display Name | Definition |
|-----------|--------------|------------|
| MANIFEST-CREATOR | Manifest Creator | System or person responsible for creating imaging manifests |
| MANIFEST-CONSUMER | Manifest Consumer | System that processes and acts upon imaging manifests |

#### 5.3.4 Imaging Study Manifest Search Metadata

The following metadata elements support searching for imaging study manifests:

| Metadata Element | Description | Type |
|------------------|-------------|------|
| patient-id | Patient identifier | Identifier |
| study-uid | Study instance UID | Identifier |
| modality | Imaging modality | CodeableConcept |
| study-date | Study acquisition date | Date |
| manifest-type | Type of manifest | CodeableConcept |

## 7 MADO DICOM Content Definitions

### 7.1 Conventions

#### 7.1.1 DICOM Structured Report

The MADO profile utilizes DICOM Key Object Selection (KOS) documents as one format for imaging manifests. These documents follow the DICOM Structured Report framework.

#### 7.1.2 Display Requirements

DICOM manifests shall support appropriate display mechanisms for human-readable presentation of manifest content.

### 7.2 General Definitions

**Imaging Manifest**: A document that contains references to one or more imaging studies, series, or instances, along with relevant metadata for retrieval and processing.

**Key Object Selection (KOS)**: A DICOM object that identifies and describes a set of DICOM objects (studies, series, instances) for a specific purpose.

**Manifest Consumer**: An entity that processes imaging manifests to facilitate subsequent imaging retrieval operations.

### 7.3 IOD Definitions

#### 7.3.1 Key Object Selection Document IODs

##### 7.3.1.1 Key Object Selection Document IOD

###### 7.3.1.1.1 Key Object Selection Document IOD MADO Context

####### 7.3.1.1.1.1 Referenced Standards

- **DICOM PS3.3**: Information Object Definitions
- **DICOM PS3.4**: Service Class Specifications
- **DICOM PS3.6**: Data Dictionary

####### 7.3.1.1.1.2 IOD Definition

The Key Object Selection Document IOD for MADO purposes shall include the following modules:

| IE | Module | Reference | Usage |
|----|--------|-----------|-------|
| Patient | Patient | C.7.1.1 | M |
| Study | General Study | C.7.2.1 | M |
| Series | Key Object Document Series | C.17.6.1 | M |
| Equipment | General Equipment | C.7.5.1 | M |
| Document | Key Object Document | C.17.6.2 | M |
| Document | SR Document Content | C.17.3 | M |
| Document | SOP Common | C.12.1 | M |

**Legend**: M = Mandatory, U = User Defined, C = Conditional

### 7.4 Module Definitions

#### 7.4.1 Key Object Selection Document Modules

##### 7.4.1.1 Patient Module

###### 7.4.1.1.1 Patient Module MADO Context

####### 7.4.1.1.1.1 Referenced Standards

- **DICOM PS3.3**: Information Object Definitions, Section C.7.1.1

####### 7.4.1.1.1.2 Module Definition

| Attribute Name | Tag | Type | Attribute Description |
|----------------|-----|------|----------------------|
| Patient's Name | (0010,0010) | 2 | Patient's full name |
| Patient ID | (0010,0020) | 2 | Primary identifier for the patient |
| Patient's Birth Date | (0010,0030) | 2 | Patient's date of birth |
| Patient's Sex | (0010,0040) | 2 | Patient's sex |

**Type Legend**: 1 = Required, 2 = Required if known, 3 = Optional

**Additional MADO Requirements**:
- Patient ID shall be present for manifest-based retrieval scenarios
- Patient demographics should be consistent across referenced studies

##### 7.4.1.2 General Study Module

###### 7.4.1.2.1 General Study Module MADO Context

####### 7.4.1.2.1.1 Referenced Standards

- **DICOM PS3.3**: Information Object Definitions, Section C.7.2.1

####### 7.4.1.2.1.2 Module Definition

| Attribute Name | Tag | Type | Attribute Description |
|----------------|-----|------|----------------------|
| Study Instance UID | (0020,000D) | 1 | Unique identifier for the Study |
| Study Date | (0008,0020) | 2 | Date the Study started |
| Study Time | (0008,0030) | 2 | Time the Study started |
| Referring Physician's Name | (0008,0090) | 2 | Name of the physician referring the patient |
| Study ID | (0020,0010) | 2 | User or equipment generated Study identifier |
| Accession Number | (0008,0050) | 2 | RIS generated number identifying the order |
| Study Description | (0008,1030) | 3 | User-defined description of the Study |

**MADO Requirements**:
- Study Instance UID is mandatory for manifest-based retrieval
- Study metadata should support efficient filtering and selection

##### 7.4.1.3 Key Object Document Series Module

###### 7.4.1.3.1 Key Object Document Series Module MADO Context

####### 7.4.1.3.1.1 Referenced Standards

- **DICOM PS3.3**: Information Object Definitions, Section C.17.6.1

####### 7.4.1.3.1.2 Module Definition

| Attribute Name | Tag | Type | Attribute Description |
|----------------|-----|------|----------------------|
| Modality | (0008,0060) | 1 | Type of equipment that originally acquired the data |
| Series Instance UID | (0020,000E) | 1 | Unique identifier of the Series |
| Series Number | (0020,0011) | 1 | A number that identifies this Series |

**MADO Specifications**:
- Modality shall be set to "KO" for Key Object Selection documents
- Series Instance UID shall be unique for each manifest document

##### 7.4.1.4 General Equipment Module

###### 7.4.1.4.1 General Equipment Module MADO Context

####### 7.4.1.4.1.1 Referenced Standards

- **DICOM PS3.3**: Information Object Definitions, Section C.7.5.1

####### 7.4.1.4.1.2 Module Definition

| Attribute Name | Tag | Type | Attribute Description |
|----------------|-----|------|----------------------|
| Manufacturer | (0008,0070) | 2 | Manufacturer of the equipment |
| Institution Name | (0008,0080) | 3 | Institution where equipment is located |
| Software Versions | (0018,1020) | 3 | Manufacturer's software versions |

**MADO Requirements**:
- Equipment information should identify the system creating the manifest

##### 7.4.1.5 Key Object Document Module

###### 7.4.1.5.1 Key Object Document Module MADO Context

####### 7.4.1.5.1.1 Referenced Standards

- **DICOM PS3.3**: Information Object Definitions, Section C.17.6.2

####### 7.4.1.5.1.2 Module Definition

| Attribute Name | Tag | Type | Attribute Description |
|----------------|-----|------|----------------------|
| Instance Number | (0020,0013) | 1 | A number that identifies this KO Document |
| Content Date | (0008,0023) | 1 | Date the document content creation |
| Content Time | (0008,0033) | 1 | Time the document content creation |
| Referenced Request Sequence | (0040,A370) | 1C | Sequence of Items referencing requests |
| Current Requested Procedure Evidence Sequence | (0040,A375) | 1 | Evidence sequence for current procedure |

**Current Requested Procedure Evidence Sequence Items**:

| Attribute Name | Tag | Type | Attribute Description |
|----------------|-----|------|----------------------|
| Study Instance UID | (0020,000D) | 1 | UID of the referenced Study |
| Referenced Series Sequence | (0008,1115) | 1 | Sequence of referenced Series |

**Referenced Series Sequence Items**:

| Attribute Name | Tag | Type | Attribute Description |
|----------------|-----|------|----------------------|
| Series Instance UID | (0020,000E) | 1 | UID of the referenced Series |
| Referenced SOP Sequence | (0008,1140) | 1C | Sequence of referenced SOP Instances |

**Referenced SOP Sequence Items**:

| Attribute Name | Tag | Type | Attribute Description |
|----------------|-----|------|----------------------|
| Referenced SOP Class UID | (0008,1150) | 1 | SOP Class UID of referenced object |
| Referenced SOP Instance UID | (0008,1155) | 1 | SOP Instance UID of referenced object |

**MADO Specifications**:
- Current Requested Procedure Evidence Sequence shall contain all studies referenced in the manifest
- Referenced SOP Sequence may be used to specify individual instances when needed

##### 7.4.1.6 SR Document Content Module

###### 7.4.1.6.1 SR Document Module MADO Context

####### 7.4.1.6.1.1 Referenced Standards

- **DICOM PS3.3**: Information Object Definitions, Section C.17.3

####### 7.4.1.6.1.2 Module Definition

| Attribute Name | Tag | Type | Attribute Description |
|----------------|-----|------|----------------------|
| Value Type | (0040,A040) | 1 | Type of value stored in this content item |
| Concept Name Code Sequence | (0040,A043) | 1 | Coded concept name of this content item |
| Content Sequence | (0040,A730) | 1C | Sequence of content items |

**MADO Content Structure**:
- Root content item shall have concept name "MADO Manifest"
- Child content items shall describe manifest purpose and scope
- Referenced studies and selection criteria should be documented

##### 7.4.1.7 SOP Common Module

###### 7.4.1.7.1 SOP Common Module MADO Context

####### 7.4.1.7.1.1 Referenced Standards

- **DICOM PS3.3**: Information Object Definitions, Section C.12.1

####### 7.4.1.7.1.2 Module Definition

| Attribute Name | Tag | Type | Attribute Description |
|----------------|-----|------|----------------------|
| SOP Class UID | (0008,0016) | 1 | Uniquely identifies the SOP Class |
| SOP Instance UID | (0008,0018) | 1 | Uniquely identifies the SOP Instance |
| Specific Character Set | (0008,0005) | 1C | Character set used for text values |
| Instance Creation Date | (0008,0012) | 3 | Date the SOP Instance was created |
| Instance Creation Time | (0008,0013) | 3 | Time the SOP Instance was created |

**MADO Requirements**:
- SOP Class UID shall be "1.2.840.10008.5.1.4.1.1.88.59" (Key Object Selection Document Storage)
- SOP Instance UID shall be unique for each manifest document

## 8 MADO HL7 FHIR Content Definitions

The MADO profile supports FHIR-based manifests using the following resource types:

### 8.1 ImagingStudy Resource

The ImagingStudy resource is used to represent imaging studies referenced in FHIR-based manifests.

**Key Elements**:
- `identifier`: Study identifiers including Study Instance UID
- `status`: Status of the imaging study  
- `subject`: Reference to the Patient
- `encounter`: Reference to the encounter context
- `started`: Date/time when study was started
- `series`: Series within the study
- `endpoint`: DICOM endpoints for retrieval

### 8.2 List Resource

The List resource serves as the primary manifest container for FHIR-based implementations.

**Key Elements**:
- `status`: Status of the manifest list
- `mode`: Mode of the list (typically "working")
- `title`: Human-readable title for the manifest
- `code`: Code identifying this as an imaging manifest
- `subject`: Reference to the Patient
- `date`: Date the manifest was created
- `source`: Creator of the manifest
- `entry`: References to ImagingStudy resources

### 8.3 Endpoint Resource

The Endpoint resource defines DICOM endpoints for image retrieval.

**Key Elements**:
- `status`: Endpoint status
- `connectionType`: Type of connection (e.g., DICOM WADO-RS)
- `address`: Base URL for the endpoint
- `payloadType`: Supported payload types

## 9 MADO DICOM – FHIR Manifest Mapping Specification

### 9.1 Overview

This section defines the mapping between DICOM KOS-based manifests and FHIR-based manifests to enable interoperability between the two formats.

### 9.2 DICOM to FHIR Mapping

| DICOM Element | FHIR Element | Mapping Notes |
|---------------|--------------|---------------|
| Patient ID (0010,0020) | Patient.identifier | Direct mapping |
| Study Instance UID (0020,000D) | ImagingStudy.identifier | Use DICOM UID format |
| Series Instance UID (0020,000E) | ImagingStudy.series.uid | Direct mapping |
| SOP Instance UID (0008,0018) | ImagingStudy.series.instance.uid | Direct mapping |
| Study Date (0008,0020) | ImagingStudy.started | Convert to FHIR dateTime |
| Referring Physician (0008,0090) | ImagingStudy.referrer | Map to Practitioner reference |

### 9.3 FHIR to DICOM Mapping

| FHIR Element | DICOM Element | Mapping Notes |
|--------------|---------------|---------------|
| Patient.identifier | Patient ID (0010,0020) | Use primary identifier |
| ImagingStudy.identifier | Study Instance UID (0020,000D) | Extract DICOM UID |
| ImagingStudy.series.uid | Series Instance UID (0020,000E) | Direct mapping |
| ImagingStudy.series.instance.uid | SOP Instance UID (0008,0018) | Direct mapping |
| ImagingStudy.started | Study Date/Time (0008,0020/0030) | Convert from FHIR dateTime |

### 9.4 Mapping Implementation Considerations

- **Identifier Handling**: Ensure consistent identifier mapping between formats
- **Date/Time Conversion**: Handle timezone and precision differences appropriately  
- **Reference Resolution**: Maintain referential integrity across format boundaries
- **Extension Handling**: Define extensions for FHIR elements without DICOM equivalents
