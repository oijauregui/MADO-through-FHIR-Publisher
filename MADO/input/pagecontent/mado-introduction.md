# Introduction to MADO Supplement

## Manifest-based Access to DICOM Objects (MADO)

This supplement describes the Manifest-based Access to DICOM Objects (MADO) profile for the IHE Radiology Technical Framework.

### Purpose

The MADO profile defines mechanisms for manifest-based access to DICOM objects, enabling efficient retrieval and management of imaging studies through standardized manifest structures.

### Document Status

- **Revision**: 0.4 – Draft in Preparation for Public Comment
- **Date**: August 18, 2025
- **Author**: IHE-HL7 Europe Sub-team on imaging manifest

**Note**: This document is prepared to become a future supplement to the IHE Radiology Technical Framework. It anticipates the acceptance of a new profile proposal submitted on July 20th 2025 to the IHE Radiology 2025-2026 Cycle. It is developed by the IHE-HL7 Europe Working Group on Imaging with the goal to use this new profile in the context of the EHDS use case on the sharing of imaging studies and related imaging reports.

### FHIR Information

- **HL7® FHIR® STU x**: Using Resources at FMM Level n-n

### Document Development Context

This supplement is intended to be a supplement to the IHE Radiology Technical Framework V22.0. Each supplement undergoes a process of public comment and trial implementation before being incorporated into the volumes of the Technical Frameworks.

**For review and comment only. DO NOT implement this public comment version.**

### Profile Overview

The profile structure includes:

1. **Volume 1**: Profile overview in terms of Actor/Transactions, the overall use case and associated scenarios. Volume 1 states the required and optional transactions, as well as the required/optional grouping.

   - **Document Creator Actor**: Produces the Imaging Manifest with a "convey manifest content" transaction to a "Document Consumer Actor"
   - **Convey Manifest Transaction** with two options:
     - DICOM KOS Based Manifest option
     - FHIR Based Manifest option
   - **Imaging Document Consumer Actor**: Issues the WADO Retrieve Transaction to the Imaging Document Source Actor

2. **Volume 2**: Chapter on the WADO-RS Retrieve Transaction

3. **Volume 3**: Chapter on the Manifest content including:
   - Section A: DICOM KOS based Manifest
   - Section B: FHIR based Manifest  
   - Section C: Mapping between A and B (for information)

### Links and References

- **General IHE Information**: [IHE.net](https://www.ihe.net)
- **IHE Radiology Domain**: [IHE Domains](https://www.ihe.net/ihe_domains)
- **Current Radiology Technical Framework**: [Radiology Technical Framework](https://www.ihe.net/uploadedFiles/Documents/Radiology/IHE_RAD_TF_Rev19.0_Vol1_FT_2020-07-03.pdf)
- **Public Comments**: [Radiology Public Comments](https://www.ihe.net/Radiology_Public_Comments)

### Comment Period

This supplement is published for Public Comment. Comments are invited and must be received by the specified deadline to be considered in development of the Trial Implementation version of the supplement.
