# Appendices to MADO Profile

## Appendix A – Implementation Examples

### A.1 DICOM KOS Manifest Example

#### A.1.1 Sample DICOM KOS Document Structure

The following example shows the structure of a DICOM Key Object Selection document used as an imaging manifest in the MADO profile:

```
Key Object Selection Document
├── Patient Module
│   ├── Patient's Name: "DOE^JOHN"
│   ├── Patient ID: "12345"
│   ├── Patient's Birth Date: "19800101"
│   └── Patient's Sex: "M"
├── General Study Module
│   ├── Study Instance UID: "1.2.840.113619.2.55.3.604688119.971.1234567890.123"
│   ├── Study Date: "20250819"
│   ├── Study Time: "143000"
│   └── Accession Number: "ACC12345"
├── Key Object Document Series Module
│   ├── Modality: "KO"
│   ├── Series Instance UID: "1.2.840.113619.2.55.3.604688119.971.1234567890.456"
│   └── Series Number: "1"
├── Key Object Document Module
│   └── Current Requested Procedure Evidence Sequence
│       └── Study Instance UID: "1.2.840.113619.2.55.3.604688119.971.1234567890.123"
│           └── Referenced Series Sequence
│               ├── Series Instance UID: "1.2.840.113619.2.55.3.604688119.971.1234567890.789"
│               └── Referenced SOP Sequence
│                   ├── Referenced SOP Class UID: "1.2.840.10008.5.1.4.1.1.2"
│                   └── Referenced SOP Instance UID: "1.2.840.113619.2.55.3.604688119.971.1234567890.101"
└── SR Document Content Module
    └── Content Item: "MADO Manifest for Emergency Review"
```

### A.2 FHIR Manifest Example

#### A.2.1 Sample FHIR List Resource for Imaging Manifest

```json
{
  "resourceType": "List",
  "id": "mado-manifest-example",
  "meta": {
    "profile": [
      "http://ihe.net/fhir/mado/StructureDefinition/ImagingManifestList"
    ]
  },
  "status": "current",
  "mode": "working",
  "title": "Emergency Imaging Review Manifest",
  "code": {
    "coding": [
      {
        "system": "http://ihe.net/fhir/mado",
        "code": "imaging-manifest",
        "display": "Imaging Manifest"
      }
    ]
  },
  "subject": {
    "reference": "Patient/patient-example"
  },
  "date": "2025-08-19T14:30:00Z",
  "source": {
    "reference": "Device/manifest-creator"
  },
  "entry": [
    {
      "item": {
        "reference": "ImagingStudy/chest-ct-study"
      }
    }
  ]
}
```

**Associated ImagingStudy Resource**:

```json
{
  "resourceType": "ImagingStudy",
  "id": "chest-ct-study",
  "identifier": [
    {
      "use": "official",
      "system": "urn:dicom:uid",
      "value": "urn:oid:1.2.840.113619.2.55.3.604688119.971.1234567890.123"
    }
  ],
  "status": "available",
  "subject": {
    "reference": "Patient/patient-example"
  },
  "started": "2025-08-19T10:00:00Z",
  "numberOfSeries": 1,
  "numberOfInstances": 150,
  "series": [
    {
      "uid": "1.2.840.113619.2.55.3.604688119.971.1234567890.789",
      "modality": {
        "system": "http://dicom.nema.org/resources/ontology/DCM",
        "code": "CT"
      },
      "numberOfInstances": 150,
      "endpoint": [
        {
          "reference": "Endpoint/wado-rs-endpoint"
        }
      ]
    }
  ]
}
```

## Appendix B – Security Considerations

### B.1 Authentication Methods

The MADO profile supports the following authentication methods:

#### B.1.1 Bearer Token Authentication

```http
Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9...
```

#### B.1.2 Basic Authentication

```http
Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=
```

#### B.1.3 Certificate-based Authentication

Client certificates may be used for mutual TLS authentication in high-security environments.

### B.2 Access Control Patterns

#### B.2.1 Role-Based Access Control (RBAC)

- **Radiologist**: Full access to all imaging content
- **Referring Physician**: Access to studies for their patients
- **Technologist**: Access to create and modify manifests
- **Administrator**: System configuration and audit access

#### B.2.2 Attribute-Based Access Control (ABAC)

Access decisions based on:
- User attributes (role, department, location)
- Resource attributes (study type, patient consent)
- Environmental attributes (time of access, network location)

### B.3 Audit Requirements

All MADO transactions shall generate audit events including:

- **Manifest Creation**: Who created what manifest, when
- **Manifest Access**: Who accessed which manifest, when
- **Image Retrieval**: What images were retrieved based on manifest
- **Access Denied**: Failed access attempts and reasons

#### B.3.1 Sample Audit Event

```json
{
  "resourceType": "AuditEvent",
  "type": {
    "system": "http://dicom.nema.org/resources/ontology/DCM",
    "code": "110100",
    "display": "Application Activity"
  },
  "action": "R",
  "recorded": "2025-08-19T14:30:00Z",
  "outcome": "0",
  "agent": [
    {
      "type": {
        "coding": [
          {
            "system": "http://terminology.hl7.org/CodeSystem/extra-security-role-type",
            "code": "humanuser"
          }
        ]
      },
      "who": {
        "identifier": {
          "value": "radiologist@hospital.org"
        }
      }
    }
  ],
  "source": {
    "site": "MADO-System",
    "identifier": {
      "value": "urn:oid:1.2.3.4.5.6.7.8.9.10"
    }
  },
  "entity": [
    {
      "what": {
        "identifier": {
          "value": "1.2.840.113619.2.55.3.604688119.971.1234567890.123"
        }
      },
      "type": {
        "system": "http://terminology.hl7.org/CodeSystem/audit-entity-type",
        "code": "2",
        "display": "System Object"
      },
      "role": {
        "system": "http://terminology.hl7.org/CodeSystem/object-role",
        "code": "1",
        "display": "Patient"
      }
    }
  ]
}
```

## Appendix C – Implementation Guidelines

### C.1 Performance Considerations

#### C.1.1 Manifest Size Optimization

- **Selective References**: Include only necessary studies/series/instances
- **Metadata Filtering**: Include only essential metadata elements
- **Compression**: Use appropriate compression for large manifests

#### C.1.2 Caching Strategies

- **Manifest Caching**: Cache frequently accessed manifests
- **Metadata Caching**: Cache study/series metadata separately
- **Image Caching**: Implement tiered caching for images

### C.2 Error Handling

#### C.2.1 Common Error Scenarios

| Error Type | HTTP Status | Description | Mitigation |
|------------|-------------|-------------|------------|
| Invalid Manifest | 400 | Malformed manifest structure | Validate manifest before processing |
| Missing Study | 404 | Referenced study not found | Check study availability |
| Access Denied | 403 | Insufficient permissions | Verify user authorization |
| Server Error | 500 | Internal processing error | Implement retry logic |

#### C.2.2 Error Response Format

```json
{
  "resourceType": "OperationOutcome",
  "issue": [
    {
      "severity": "error",
      "code": "not-found",
      "details": {
        "text": "Referenced study 1.2.3.4.5 not found"
      },
      "location": [
        "ImagingStudy.identifier"
      ]
    }
  ]
}
```

### C.3 Testing and Validation

#### C.3.1 Manifest Validation

- **Structure Validation**: Verify DICOM/FHIR conformance
- **Reference Validation**: Ensure all references are valid
- **Metadata Validation**: Check required metadata presence

#### C.3.2 Integration Testing

- **End-to-End Workflows**: Test complete manifest-to-retrieval workflows
- **Cross-Format Testing**: Validate DICOM-FHIR interoperability
- **Performance Testing**: Verify performance under load

#### C.3.3 Conformance Testing

Systems implementing MADO shall support:
- Standard conformance testing suites
- Interoperability testing events
- Certification processes as defined by IHE

## Appendix D – Use Case Scenarios

### D.1 Clinical Decision Support

**Scenario**: A clinical decision support system needs to retrieve relevant prior imaging studies for comparison with current studies.

**Flow**:
1. CDS system creates manifest with relevant prior studies
2. Manifest includes selection criteria and study priorities
3. Imaging consumer retrieves studies based on manifest
4. Studies are presented in context of current case

### D.2 Multi-Site Case Review

**Scenario**: A radiologist needs to review cases from multiple imaging sites for a tumor board meeting.

**Flow**:
1. Case coordinator creates manifest including studies from multiple sites
2. Manifest includes access credentials for each site
3. Review system retrieves studies from all sites
4. Studies are presented in unified interface

### D.3 Research Data Collection

**Scenario**: Researchers need to collect imaging data matching specific criteria for a study.

**Flow**:
1. Research system creates manifest based on search criteria
2. Manifest includes anonymization requirements
3. Data collection system retrieves and processes studies
4. Anonymized data is delivered to research repository

### D.4 Emergency Access

**Scenario**: Emergency department needs immediate access to patient's recent imaging studies.

**Flow**:
1. ED system creates urgent manifest for patient
2. Manifest bypasses normal authorization checks
3. Critical studies are retrieved with high priority
4. Emergency team has immediate access to imaging

## Appendix E – Glossary

**DICOM**: Digital Imaging and Communications in Medicine - Standard for medical imaging information

**FHIR**: Fast Healthcare Interoperability Resources - HL7 standard for health information exchange

**ImagingStudy**: FHIR resource representing a collection of imaging series

**Key Object Selection (KOS)**: DICOM object that references other DICOM objects for a specific purpose

**List**: FHIR resource that represents a collection of other resources

**Manifest**: Document containing references to imaging studies or instances

**WADO-RS**: Web Access to DICOM Objects - RESTful Services for accessing DICOM content

**Endpoint**: FHIR resource representing a network service for accessing resources
