---
layout: page
---

## Standard Results (ORU-R01) Outbound Specifications

Interface between IRIS v2.5 and a third-party system

TEC 007 Rev B Standard Results (ORU-R01) Outbound Specifications

### Acknowledgments 

Interface specifications prepared by Adam Diaz. Please send any feedback to support@irishelp.zendesk.com. 

### Confidentiality and Proprietary Rights 

This document is the confidential property of Intelligent Retinal Imaging Systems (IRIS). No part of this document may be reproduced in any form or incorporated into any information retrieval system without the written permission of IRIS. 

### Limitations and Conditions of Use 

Some fields noted in this document need the pre-defined values to be identified by the receiving EMR. Those values will be covered in the Standard Orders (ORM-O01) Interface document and used here. If there are any fields represented in this document that are not populated but required by a receiving EMR, IRIS will populate these value(s)as requested. IRIS uses HL7 v2.4 as a main protocol for message sharing. If the receiving EMR requires a different version, IRIS will provide messages for the corresponding version. This document will describe the standard IRIS ORU message by segments as they appear in an HL7 message. 

### Disclaimers 

Although unlikely, IRIS reserves the right to make changes in specifications and features shown herein at any time without notice. This does not constitute a representation or documentation regarding the service featured. Contact your IRIS Representative for the most current information. 

Intelligent Retinal Imaging Systems™

2 N Palafox Street, Suite 200 

Pensacola, Florida 32502 

1-888-535-2574 | info@retinalscreenings.com 

***


## Table of Contents

1. [Basic Interface Structure and Description](#basic-interface-structure-and-description) 
2. [MSH Segment – Message Header](#msh-segment---message-header)
3. [PID Segment - Patient Identification](#pid-segment---patient-identification)
4. [PV1 Segment – Patient Visit](#pv1-segment-–-patient-visit)
5. [ORC – Common Order](#orc-–-common-order)
6. [OBR – Observation Request](#obr-–-observation-request) 
7. [DG1 – Diagnosis](#dg1-segment-–-diagnosis) 
8. [OBX – Observation Result](#obx-–-observation-result-segment)
9. [First OBX](#first-obx) 
10. [Second OBX](#second-obx) 
11. [Third OBX](#third-obx)
12. [Fourth OBX](#fourth-obx)
13. [Fifth OBX](#fifth-obx)
14. [Sixth OBX](#sixth-obx)
15. [Seventh OBX](#seventh-obx)
16. [Eighth OBX](#eighth-obx) 
17. [Ninth OBX](#ninth-obx)
18. [Tenth and Above OBX](#tenth-and-above-obx) 
19. [Last OBX](#last-obx)
20. [Example Messages](#example-messages) 
21. [Revision History](#revision-history) 

***

## Basic Interface Structure and Description

This document outlines the values sent in an HL7 results message (ORU-R01) from IRIS to a third-party system. This third-party system could be another application, an interface engine, or an EMR. For the purposes of this documentation, this third-party will be referred to an EMR. 

IRIS uses HL7 v2.4 as a main protocol for message sharing. If there are any fields represented in this document that are not populated but required by a receiving EMR, IRIS will populate these value(s) as requested. If the receiving EMR requires a different version, IRIS can provide messages for the corresponding version. If the EMR expects messages to be delivered in a format other than HL7, this will need to be discussed with an IRIS Interface Engineer as this will not be covered in the scope of this document – please contact IRIS support for more information. 

Following this section, the rest of the document will be divided by HL7 segment types as they would appear in a delivered HL7 message. IRIS has two different methods for delivering the document in an OBX segment: encoded PDF or pointer link (static URL). This will be described in detail later in that section. The last section of this document contains an example HL7 message. 

### Customization
This document provides the default implementation of IRIS results to an HL7 ORU message.  These mappings can be customized to suite your requirements. Customization can either be performed by an IRIS integration specialist or if desired you may designate an individual within your organization with the Integrator role providing them access to the IRIS Integration application.

## MSH Segment - Message Header

The message header segment includes information that defines the structure of the message.

### Sample MSH segment: 
```
MSH|^\~\&|IRIS|IRIS|VENDOR|VENDOR|20170410145907||ORU^R01|170410145907|T|2.4 
```

| Field  | Name and type  | Sample value  | Description  |
| -- | -- | -- | -- |
| MSH-1  | Field Separator (ST)  | |  | Auto generated  |
| MSH-2  | Encoding Characters (ST)  | ^\~\&  | Auto generated  |
| MSH-3  | Sending Application (HD)  | IRIS  | “IRIS” is used as the default value  |
| MSH-4  | Sending Facility (HD)  | IRIS  | “IRIS” is used as the default value  |
| MSH-5  | Receiving Application (HD)  | VENDOR  | The name of the EMR. This needto be pre-defined  |
| MSH-6  | Receiving Facility (HD)  | VENDOR  | The name or id of clinic. This need to be pre-defined.  |
| MSH-7  | Date/Time of Message (TS)  | yyyyMMddHHmmss format, e.g. "20190410145907"  | Auto generated based off servertime (UTC if hosted by Iris)  |
| MSH-8  | Security (ST)  |   | Empty  |
| MSH-9  | Message Type (MSG)  | ORU^R01  | Unchanging  |
| MSH-10  | Message Control ID (ST)  | 190410145907  | Auto generated from server time (UTC if hosted by Iris)  |
| MSH-11  | Processing ID (PT)  | T  | P (Production) or T (Test) will be used based on the system environment  |
| MSH-12  | Version ID (VID)  | 2.4  | HL7 Version  |


[back to Table of Contents](#table-of-contents)


## PID Segment - Patient Identification 

The patient identification segment will include patient-specific demographic information. 

### Sample PID segment: 
```
PID||ITCC20170410^^^^MRN|ITCC20170410||DOE^JOHN||19581012|M||||||||||1234|111-22-3333 
```

| Field  | Name and type  |  Sample value  | Description  |
| -- | -- | -- | -- |
| PID-1  | Set ID - PID (SI)  |  |  Empty by default  |
|  PID-2  | Patient ID (CX)  | ITCC20170410^^^^MRN  |  |
|   | PID-2.1 ID  |  ITCC20170410  | Related Patient's MRN value  |
|   | ….  |  | PID-2.2 through PID-2.4 empty by default  |
|  | PID-2.5 Identifier Type Code  | MRN  | Related Patient's ID type if requested  |
| PID-3  | Patient Identifier List (CX)  | ITCC20170410   | Reflects the same value as PID-2.1 by default  |
| PID-4  | Alternate Patient ID - PID (CX)  |  | Empty by default  |
| PID-5  | Patient Name (XPN)  | DOE^JOHN   | Related Patient's family name &given name  |
| PID-6  | Mother's Maiden Name (XPN)  |  | Empty by default  |
| PID-7  | Date/Time of Birth (TS)  | 19581012  | Related Patient's Date of birth; YYYYMMDD format  |
| PID-8  | Administrative Sex (IS)  |  M  | Related patient's sex (M or F)  |
| PID-9  | Patient Alias (XPN)  |   | Empty by default  |
| PID-10  | Race (CE)  |   | Empty by default  |
| PID-11  | Patient Address (XAD)  |   | Empty by default  |
| PID-12  | County Code (IS)  |   | Empty by default  |
| PID-13  | Phone Number - Home (XTN)  |   | Empty by default  |
| PID-14  | Phone Number - Business (XTN)  |   | Empty by default  |
| PID-15  | Primary Language (CE)  |   | Empty by default  |
| PID-16  | Marital Status (CE)  |   | Empty by default  |
| PID-17  | Religion (CE)  |  | Empty by default  |
| PID-18  | Patient Account Number (CX)  | 1234  | Related Patient's CSN/Account value/Encounter number  |
| PID-19  | SSN Number - Patient (ST)  | 111-22-3333  | Iris does not store SSN  |


[back to Table of Contents](#table-of-contents)


## PV1 Segment – Patient Visit 

The patient visit segment includes patient-specific visit information.

### Sample PV1 segment: 
```
PV1|1|O|POC01||||GR0001^DOE^JANE^^^MD^MD^^^^^^NPI|OP0001^DOE^JACK^^^MD^M D^^^^^^NPI|||||||||GR0001^DOE^JANE^^^MD^MD^^^^^^NPI|||||||||||||||||||||||||||20170410145803|20170410145803 
```

| Field  | Name and type  | Sample value  | Description  |
| -- | -- | -- | -- |
| PV1-1  | Set ID - PV1 (SI)  | 1  | Always 1  |
| PV1-2  | Patient Class (IS)  | O  | Always "O" (for outpatient)  |
| PV1-3  | Assigned Patient Location (PL)  | POC01  | Point of care information; received from the ORM HL7 message  |
| PV1-4  | Admission Type (IS)  |   | Empty by default  |
| PV1-5  | Preadmit Number (CX)  |   | Empty by default  |
| PV1-6  | Prior Patient Location (PL)  |   | Empty by default  |
| PV1-7  | Attending Doctor (XCN)  | OP0001^DOE^JACK^^ ^MD^MD^^^^^^NPI  | This reflects the ordering provider received in the ORM HL7 message  |
|   | PV1-7.1 ID  | OP0001  | Reflects the ORM HL7 messageORC-12.1 value  |
|   | PV1-7.2 Family Name  | DOE  | Reflects the ORM HL7 messageORC-12.2 value  |
|   | PV1-7.3 Given Name  | JANE  | Reflects the ORM HL7 messageORC-12.3 value  |
|   | …..  |   | PV1-7.4 & PV1-7.5 empty by default  |
|   | PV1-7.5 Prefix  | MD  | Credentials of the ordering physician if available  |
|   | PV1-7.6 Degree  | MD  | Credentials of the ordering physician if available  |
|   | …..  |   | PV1-7.7 through PV1-7.12 Empty by default  |
|   | PV1-7.13 Identifier Type Code  | NPI  | Provider type identifying code; typically “NPI”  |
| PV1-8  | Referring Doctor (XCN)  | OP0001^DOE^JACK^^ ^MD^MD^^^^^^NPI  | This reflects the ordering provider received in the ORM HL7 message  |
|   | PV1-8.1 ID  | OP0001  | Reflects the ORM HL7 messageORC-12.1 value  |
|   | PV1-8.2 Family Name  | DOE  | Reflects the ORM HL7 messageORC-12.2 value  |
| -- | -- | -- | -- |
|   | PV1-8.3 Given Name  | JACK  | Reflects the ORM HL7 messageORC-12.3 value  |
|   | …..  |   | PV1-8.4 & PV1-8.5 empty by default  |
|   | PV1-8.5 Prefix  | MD  | Credentials of the ordering physician if available  |
|   | PV1-8.6 Degree  | MD  | Credentials of the ordering physician if available  |
|   | …..  |   | PV1-8.7 through PV1-8.12 Empty by default  |
|   | PV1-8.13 Identifier Type Code  | NPI  | Provider type identifying code; typically “NPI”  |
| PV1-10  | Hospital Service (IS)  |   | Empty by default  |
| PV1-11  | Temporary Location (PL)  |   | Empty by default  |
| PV1-12  | Preadmit Test Indicator (IS)  |   | Empty by default  |
| PV1-13  | Re-admission Indicator (IS)  |   | Empty by default  |
| PV1-14  | Admit Source (IS)  |   | Empty by default  |
| PV1-15  | Ambulatory Status (IS)  |   | Empty by default  |
| PV1-16  | VIP Indicator (IS)  |   | Empty by default  |
| PV1-17  | Admitting Doctor (XCN)  | OP0001^DOE^JACK^^ ^MD^MD^^^^^^NPI  | This reflects the ordering provider received in the ORM HL7 message  |
|   | PV1-17.1 ID  | OP0001  | Reflects the ORM HL7 messageORC-12.1 value  |
|   | PV1-17.2 Family Name  | DOE  | Reflects the ORM HL7 messageORC-12.2 value  |
|   | PV1-17.3 Given Name  | JANE  | Reflects the ORM HL7 messageORC-12.3 value  |
|   | …..  |   | PV1-17.4 & PV1-17.5 empty bydefault  |
|   | PV1-17.5 Prefix  | MD  | Credentials of the ordering physician if available  |
|   | PV1-17.6 Degree  | MD  | Credentials of the ordering physician if available  |
|   | …..  |   | PV1-17.7 through PV1-17.12 Empty by default  |
|   | PV1-17.13 Identifier Type Code  | NPI  | Provider type identifying code; typically “NPI”  |
| PV1-18  | Patient Type (IS)  |   | Empty by default  |
| PV1-19  | Visit Number (CX)  |   | Related Patient's CSN/Account value/Encounter number  |
| PV1-20  | Financial Class (FC)  |   | Empty by default  |
| -- | -- | -- | -- |
| PV1-21  | Charge Price Indicator (IS)  |   | Empty by default  |
| PV1-22  | Courtesy Code (IS)  |   | Empty by default  |
| PV1-23  | Credit Rating (IS)  |   | Empty by default  |
| PV1-24  | Contract Code (IS)  |   | Empty by default  |
| PV1-25  | Contract Effective Date (DT)  |   | Empty by default  |
| PV1-26  | Contract Amount (NM)  |   | Empty by default  |
| PV1-27  | Contract Period (NM)  |   | Empty by default  |
| PV1-28  | Interest Code (IS)  |   | Empty by default  |
| PV1-29  | Transfer to Bad Debt Code (IS)  |   | Empty by default  |
| PV1-30  | Transfer to Bad Debt Date (DT)  |   | Empty by default  |
| PV1-31  | Bad Debt Agency Code (IS)  |   | Empty by default  |
| PV1-32  | Bad Debt Transfer Amount (NM)  |   | Empty by default  |
| PV1-33  | Bad Debt Recovery Amount (NM)  |   | Empty by default  |
| PV1-34  | Delete Account Indicator (IS)  |   | Empty by default  |
| PV1-35  | Delete Account Date (DT)  |   | Empty by default  |
| PV1-36  | Discharge Disposition (IS)  |   | Empty by default  |
| PV1-37  | Discharged to Location (DLD)  |   | Empty by default  |
| PV1-38  | Diet Type (CE)  |   | Empty by default  |
| PV1-39  | Servicing Facility (IS)  |   | Empty by default  |
| PV1-40  | Bed Status (IS)  |   | Empty by default  |
| PV1-41  | Account Status (IS)  |   | Empty by default  |
| PV1-42  | Pending Location (PL)  |   | Empty by default  |
| PV1-43  | Prior Temporary Location (PL)  |   | Empty by default  |
| PV1-44  | Admit Date/Time (TS)  | yyyyMMddHHmmss format  | Date of image captured |
| PV1-45  | Discharge Date/Time (TS)  | yyyyMMddHHmmss format  | Date of image captured |
| PV1-46  | Current Patient Balance (NM)  |   | Empty by default  |
| PV1-47  | Total Charges (NM)  |   | Empty by default  |
| PV1-48  | Total Adjustments (NM)  |   | Empty by default  |
| PV1-49  | Total Payments (NM)  |   | Empty by default  |
| PV1-50  | Alternate Visit ID (CX)  |   | Empty by default  |
| PV1-51  | Visit Indicator (IS)  |   | Empty by default  |
| PV1-52  | Other Healthcare Provider (XCN)  |   | Empty by default  |


[back to Table of Contents](#table-of-contents)
 

## ORC – Common Order

The Common Order segment is used to transmit fields that are common to all orders. The ORC segment is required in the Order (ORM) message. 

### Sample ORC segment: 
```
ORC|RE|2017041006|273013^IRIS||F||||20170410145907|||OP0001^DOE^JACK^^^MD^MD ^^^^^^NPI 
```

| Field  | Name and type  | Sample value  | Description  |
| -- | -- | -- | -- |
| ORC-1  | Order Control (ID)  | RE  | Always "RE" (Observations/Performed  Service to follow)  |
|  ORC-2  | Placer Order Number (EI)  | 2017041006  |  |
|   | ORC-2.1 Entity Identifier  | 2017041006   | Order number provided in ORM HL7 message  |
|  | ORC-2.2 Namespace ID  |  |  Empty by default  |
|  ORC-3  | Filler Order Number (EI)  | 273013^IRIS  |  |
|   | ORC-3.1 Entity Identifier  | 273013  | Iris’ internal order number; this number is a unique value  |
|  | ORC-3.2 Namespace ID  | IRIS   | “IRIS” is used as the default value  |
| ORC-4  | Placer Group Number (EI)  |  | Empty by default  |
| ORC-5  | Order Status (ID)  | F   | “F” (for Final) is used by default;Iris does not send preliminary orcorrected reports.  |
| ORC-6  | Response Flag (ID)  |   | Empty by default  |
| ORC-7  | Quantity/Timing (TQ)  |   | Empty by default  |
| ORC-8  | Parent Order (EIP)  |  | Empty by default  |
| ORC-9  | Date/Time of Transaction (TS)  | yyyyMMddHHmmss format   | Current time based on server time (UTC if hosted by Iris)  |
| ORC-10  | Entered By (XCN)  |   | Empty by default  |
| ORC-11  | Verified By (XCN)  |  | Empty by default  |
| ORC-12   | Ordering Provider (XCN)  | OP0001^DOE^JACK^^ ^MD^MD^^^^^^NPI  | This reflects the ordering provider received in the ORM HL7 message  |
|   | ORC-12.1 ID  | OP0001  | Reflects the ORM HL7 message ORC-12.1 value  |
|   | ORC-12.2 Family Name  | DOE  | Reflects the ORM HL7 message ORC-12.2 value  |
|  | ORC-12.3 Given Name  | JACK  | Reflects the ORM HL7 message ORC-12.3 value  |
| …..   |  | ORdef | C-12.4 & ORC-12.5 empty byault  |
| -- | -- | -- | -- |
| ORC-12.5 P  | refix MD  | Crephy | dentials of the ordering sician if available  |
| ORC-12.6 D  | egree MD   | Crephy | dentials of the ordering sician if available  |
| …..   |  | OREm | C-12.7 through ORC-12.12 pty by default  |
| ORC-12.13 Code  | Identifier Type NPI  | Protypi | vider type identifying code; cally “NPI”  |

[back to Table of Contents](#table-of-contents)

## OBR – Observation Request 

The observation request segment transmits information about an exam, diagnostic study, or assessment that is specific to an order or result. 

### Sample OBR segment: 
```
OBR|1|2017041006|273013^IRIS|92250^FUNDUS PHOTOGRAPHY^EAP^^FUNDAL PHOTO|||20170410145803|||||||20170410145803||OP0001^DOE^JACK^^^MD^MD^^^^^^NPI||||||20170410145901|||F|||||||GR0001^DOE^JANE^20170410145901^JANE DOE, MD,NPI: 1234567890, Taxonomy: 207W00000X 
```

| Field  | Name and type  | Sample value  | Description  |
| -- | -- | -- | -- |
| OBR-1  | Set ID - OBR (SI)  | 1  | Always "1"; Iris never sends multiple OBR segments  |
| OBR-2  | Placer Order Number (EI)  | 2017041006  |   |
|   | OBR-2.1 Entity Identifier  | 2017041006  | Order number provided in ORMHL7 message  |
|   | OBR-2.2 Namespace ID  |   | Empty by default  |
| OBR-3  | Filler Order Number (EI)  | 273013^IRIS  |   |
|   | OBR-3.1 Entity Identifier  | 273013  | Iris’ internal order number. Thisnumber is unique value.  |
|   | OBR-3.2 Namespace ID  | IRIS  | “IRIS” is used as the default value  |
| OBR-4  | Universal Service Identifier (CE)  | 92250^FUNDUS PHOTOGRAPHY ^EAP^^FUNDAL PHOTO  | CPT code in OBR.4.1 followed bythe description in OBR.4.2. This value is populated with the values present in the creating ORM message unless otherwiserequested.  See below for Sample examples  |
|   |   | EAP777357^Ophthalmology Retinal Scan  | Sample 1  |
|   |   | 57554^DIABETIC EYE EXAM IN OFFICE ^IRISEAP^^DIABETIC EYE EXAM IN OFFICE  | Sample 2  |
|   |   | IMG0555^DIABETIC RETINAL SCREENING (IRIS CAMERA) ^MRGECP^^DIABETIC RETINAL SCREENING  | Sample 3  |
|   |   | OPH112^IRIS RETINAL ASSESSMENT^LAB^^OPH94  | Sample 4  |
| -- | -- | -- | -- |
| OBR-5  | Priority (ID)  |   | Empty by default  |
| OBR-6  | Requested Date/Time (TS)  |   | Empty by default  |
| OBR-7  | Observation Date/Time # (TS)  | yyyyMMddHHmmss format  | Date of image capture  |
| OBR-8  | Observation End Date/Time # (TS)  |   | Empty by default  |
| OBR-9  | Collection Volume (CQ)  |   | Empty by default  |
| OBR-10  | Collector Identifier (XCN)  |   | Empty by default  |
| OBR-11  | Specimen Action Code (ID)  |   | Empty by default  |
| OBR-12  | Danger Code (CE)  |   | Empty by default  |
| OBR-13  | Relevant Clinical Info. (ST)  |   | Empty by default  |
| OBR-14  | Specimen Received Date/Time (TS)  | yyyyMMddHHmmss format  | Date of image capture  |
| OBR-15  | Specimen Source (SPS)  |   | Empty by default  |
| OBR-16  | Ordering Provider (XCN)  | OP0001^DOE^JACK^^ ^MD^MD^^^^^^NPI  | Same value as ORC-12  |
| OBR-17  | Order Callback Phone Number (XTN)  |   | Empty by default  |
| OBR-18  | Placer Field 1 (ST)  |   | Empty by default  |
| OBR-19  | Placer Field 2 (ST)  |   | Empty by default  |
| OBR-20  | Filler Field 1 (ST)  |   | Empty by default  |
| OBR-21  | Filler Field 2 (ST)  |   | Empty by default  |
| OBR-22  | Results Report/Status Change - Date/Time (TS)  | yyyyMMddHHmmss format  | Date of grading (signing)  |
| OBR-23  | Charge to Practice (MOC)  |   | Empty by default  |
| OBR-24  | Diagnostic Serv Sect ID (ID)  |   | Empty by default  |
| OBR-25  | Result Status (ID)  | F  | F = Final, C = Corrected*  |
| OBR-26  | Parent Result (PRL)  |   | Empty by default  |
| OBR-27  | Quantity/Timing (TQ)  |   | Empty by default  |
| OBR-28  | Result Copies To (XCN)  |   | Empty by default  |
| OBR-29  | Parent Number (EIP)  |   | Empty by default  |
| OBR-30  | Transportation Mode (ID)  |   | Empty by default  |
| OBR-31  | Reason for Study (CE)  |   | Empty by default  |
| OBR-32  | Principal Result Interpreter (NDL)  | GR0001^DOE^JANE ^20170410145901^JANE DOE, MD, NPI: 1234567890, Taxonomy: 207W00000X  | The default configuration is tosend the grader’s informationhere.  |
|   | OBR-32.1  | GR0001  | Grader ID in IRIS  |
|   | OBR-32.2  | DOE  | Grader Family Name  |
|   | OBR-32.3  | JANE  | Grader Given Name  |
|   | OBR-32.4  | yyyyMMddHHmmss format  | Date of grading (signing)  |
|   | OBR-32.5  | JANE1234207W |  DOE, MD, NPI: 567890, Taxonomy: 00000X  | The grader’s full name withcredentials  |
| -- | -- | -- | -- |


*Iris does not send preliminary or corrected reports but upon request can send a status of “C” when a report has been sent back to be regraded. These are not corrected reports but instead a new report for the existing order.* 
 
## DG1 Segment – Diagnosis

*Optional*
This is the diagnosis segment that can be used to send multiple diagnoses. This can be generated upon request but is otherwise not part of the Iris standard ORU HL7 message.

### Sample DG1 segment: 
```
DG1|1||E1141^Type 2 diabetes mellitus with diabetic mononeuropathy^ICD10 
DG1|2||E113291^Type 2 diabetes mellitus with mild nonproliferative diabetic retinopathy without macular edema, right eye^ICD10 without macular edema, right eye^ICD10 
DG1|3||E113512^Type 2 diabetes mellitus with proliferative diabetic retinopathy with macular edema, left eye^ICD10 
```

| Field  | Name and type  | Sample value  | Description  |
| -- | -- | -- | -- |
| DG1-1  | Set ID - DG1 (SI)  | 1  | The Set ID that corresponds with the number of diagnoses  |
| DG1-2  | Diagnosis Coding Method (ID)  |   | Empty by default  |
| DG1-3  | Diagnosis Code (CE)  | E1141^Type 2 diabetes mellitus with diabetic mononeuropathy^ICD10  | This reflects the diagnoses added after the image has been graded  |
|   | DG1-3.1 Identifier  |  E1141  | ICD10 code for diagnosis  |
|   | DG1-3.2 Text  | Type 2 diabetes mellitus with diabetic mononeuropathy  | Description of diagnosis code  |
|   | DG1-3.3 Name of Coding System  | ICD10  | The coding system: Iris currentlyutilizes ICD10  |


[back to Table of Contents](#table-of-contents)
 

## OBX – Observation Result Segment 

The observation result segment is used to transmit a single observation or observation fragment. OBX segments can repeat and each one contains following information. 

IRIS’ default OBX HL7 configuration contains the discrete results in the first 9 OBX segments, followed by the textual portion of the result findings, and then the document modality. The breakdown follows this configuration (the numbers refer to the Set ID): OBX 1 contains the overall severity for discrete findings of both eyes, OBX 2-5 contain details of the right eye, OBX 6-9 contain details of the left eye, OBX 10-30 grading results, and the last OBX is where either an encoded PDF or pointer link (static URL) is sent. 

### Sample Discrete Results (Default): 
```
OBX|1|ST|SEVERITY^^IRIS|1|NORMAL||||||F 
OBX|2|ST|RIGHTDIABRETIN^^IRIS|2|None||||||F 
OBX|3|ST|RIGHTMACEDEMA^^IRIS|3|None||||||F 
OBX|4|ST|RIGHTOTHERRETIN^^IRIS|4|None||||||F 
OBX|5|ST|RIGHTQUALAPP^^IRIS|5|Gradable Image||||||F 
OBX|6|ST|LEFTDIABRETIN^^IRIS|6|None||||||F 
OBX|7|ST|LEFTMACEDEMA^^IRIS|7|None||||||F 
OBX|8|ST|LEFTOTHERRETIN^^IRIS|8|None||||||F 
OBX|9|ST|LEFTQUALAPP^^IRIS|9|Gradable Image||||||F 
```

### Sample Discrete Results with Alternating OBX/NTE segments (Secondary Option): 
```
OBX|1|ST|^^IRIS|1|NORMAL|||
NTE|1|ST|SEVERITY 
OBX|2|ST|^^IRIS|2|None||||||F
NTE|1|ST|RIGHTDIABRETIN
OBX|3|ST|^^IRIS|3|None||||||F 
NTE|1|ST|RIGHTMACEDEMA 
OBX|4|ST|^^IRIS|4|None||||||F 
NTE|1|ST|RIGHTOTHERRETIN 
OBX|5|ST|RIGHTQUALAPP^^IRIS|5|Gradable Image||||||F 
OBX|6|ST|^^IRIS|6|None||||||F 
NTE|1|ST|LEFTDIABRETIN 
OBX|7|ST|^^IRIS|7|None||||||F 
NTE|1|ST|LEFTMACEDEMA 
OBX|8|ST|^^IRIS|8|None||||||F 
NTE|1|ST|LEFTOTHERRETIN 
OBX|9|ST|LEFTQUALAPP^^IRIS|9|Gradable Image||||||F 
```

## First OBX
Severity section

### Sample OBX segment: First OBX 
```
OBX|1|ST|SEVERITY^^IRIS|1|NORMAL||||||F
```

| Field  | Name and type  | Sample value  | Description  |
| -- | -- | -- | -- |
| OBX-1  | Set ID - OBX (SI)  | 1  | Increases sequentially by 1  |
| OBX-2  | Value Type (ID)  | ST  | The literal value "ST" (i.e. String for OBX-5)  |
|  OBX-3  | Observation Identifier (CE)  | SEVERITY^^IRIS  |  |
|  | OBX-3.1 Identifier  | SEVERITY  | The literal value “SEVERITY”  |
|    | OBX-3.2 Text  |   | Empty by default  |
|  | OBX-3.3 Name of Coding System  | IRIS  | The literal value “IRIS”  |
| OBX-4  | Observation Sub-Id (ST)  | 1  | Sequential counter that matches the Set ID  |
| OBX-5  | Observation Value (Varies)  | CRITICAL   | One of three values based on the result findings: NORMAL, ALERT, or CRITICAL  |
| OBX-6  | Units (CE)  |   | Empty by default  |
| OBX-7  | References Range (ST)  |  | Empty by default  |
| OBX-8  | Abnormal Flags (IS)  | AA   | One of three values based on the result findings: empty (Normal), A (ALERT), or AA (CRITICAL)  |
| OBX-9  | Probability (NM)  |   | Empty by default  |
| OBX-10  | Nature of Abnormal Test (ID)  |  | Empty by default  |
| OBX-11  | Observation Result Status (ID)  | F  | F = Final, C = Corrected*  |


[back to Table of Contents](#table-of-contents)

## Second OBX

### Sample OBX segment: 
```
OBX|2|ST|RIGHTDIABRETIN^^IRIS|2|None||||||F
```

| Field  | Name and type  | Sample value  | Description  |
| -- | -- | -- | -- |
| OBX-1  | Set ID - OBX (SI)  | 2  | Increase sequentially by 1  |
| OBX-2  | Value Type (ID)  | ST  | Always "ST" (i.e. String for OBX- 5)  |
|  OBX-3  | Observation Identifier (CE)  | RIGHTDIABRETIN^^IRIS  |  |
|   | OBX-3.1 Identifier  | RIGHTDIABRETIN   | Always "RIGHTDIABRETIN"; stands for RIGHT eye diabetic retinopathy  |
|   | OBX-3.2 Text  |  | Empty by default  |
|  | OBX-3.3 Name of Coding System  | IRIS  | Always "IRIS"  |
| OBX-4  | Observation Sub-Id (ST)  | 2  | Sequential counter that matches the Set ID  |
| OBX-5  | Observation Value (Varies)  | None   | One of three values based on the result findings: None, Mild, Moderate, Severe, or Proliferative  |
| OBX-6  | Units (CE)  |   | Empty by default  |
| OBX-7  | References Range (ST)  |   | Empty by default  |
| OBX-8  | Abnormal Flags (IS)  |   | One of three values based on the result findings: empty (Normal), A (ALERT), or AA (CRITICAL)  |
| OBX-9  | Probability (NM)  |   | Empty by default  |
| OBX-10  | Nature of Abnormal Test (ID)  |  | Empty by default  |
| OBX-11  | Observation Result Status (ID)  | F  | F = Final, C = Corrected*  |


[back to Table of Contents](#table-of-contents)


## Third OBX 

### Sample OBX segment: Third 
```
OBX|3|ST|RIGHTMACEDEMA^^IRIS|3|Severe|||AA|||F 
```

| Field  | Name and type  | Sample value  | Description  |
| -- | -- | -- | -- |
| OBX-1  | Set ID - OBX (SI)  | 3  | Increase sequentially by 1  |
| OBX-2  | Value Type (ID)  | ST  | Always "ST" (i.e. String for OBX- 5)  |
|  OBX-3  | Observation Identifier (CE)  | RIGHTMACEDEMA^^IRIS  |  |
|   | OBX-3.1 Identifier  | RIGHTMACEDEMA   | Always "RIGHTMACEDEMA"; stands for RIGHT eye macular edema  |
|   | OBX-3.2 Text  |  | Empty by default  |
|  | OBX-3.3 Name of Coding System  | IRIS  | Always "IRIS"  |
| OBX-4  | Observation Sub-Id (ST)  | 3  | Sequential counter that matches the Set ID  |
| OBX-5  | Observation Value (Varies)  | Severe   | One of three values based on the result findings: None, Mild, Moderate, or Severe  |
| OBX-6  | Units (CE)  |   | Empty by default  |
| OBX-7  | References Range (ST)  |  | Empty by default  |
| OBX-8  | Abnormal Flags (IS)  | AA   | One of three values based on the result findings: empty (Normal), A (ALERT), or AA (CRITICAL)  |
| OBX-9  | Probability (NM)  |   | Empty by default  |
| OBX-10  | Nature of Abnormal Test (ID)  |  | Empty by default  |
| OBX-11  | Observation Result Status (ID)  | F  | F = Final, C = Corrected*  |


[back to Table of Contents](#table-of-contents)

## Fourth OBX 

### Sample OBX segment: 
```
OBX|4|ST|RIGHTOTHERRETIN^^IRIS|4|None||||||F
```

| Field  | Name and type  | Sample value  | Description  |
| -- | -- | -- | -- |
| OBX-1  | Set ID - OBX (SI)  | 4  | Increase sequentially by 1  |
| OBX-2  | Value Type (ID)  | ST  | Always "ST" (i.e. String for OBX- 5)  |
|  OBX-3  | Observation Identifier (CE)  | RIGHTOTHERRETIN^^IRIS  |  |
|   | OBX-3.1 Identifier  | RIGHTOTHERRETIN   | Always "RIGHTOTHERRETIN"; stands for RIGHT eye OTHER retinopathy  |
|   | OBX-3.2 Text  |  | Empty by default  |
|  | OBX-3.3 Name of Coding System  | IRIS  | Always "IRIS"  |
| OBX-4  | Observation Sub-Id (ST)  | 4  | Sequential counter that matches the Set ID  |
| OBX-5  | Observation Value (Varies)  | None, Suspected Glaucoma, Suspected Dry AMD, Suspected Wet AMD, Suspected Vein Occlusion, Suspected HTN Retinopathy, Suspected Epiretinal Membrane, Suspected Macular Hole, Suspected Cataracts, Suspected Geographic Atrophy, Other  based on result.  | Providers can choose to apply a predefined description (see Sample Value) or populate with a free-text note  |
| OBX-6  | Units (CE)  |   | Empty by default  |
| OBX-7  | References Range (ST)  |   | Empty by default  |
| OBX-8  | Abnormal Flags (IS)  |   | One of three values based on the result findings: empty (Normal), or A (ALERT)  |
| OBX-9  | Probability (NM)  |   | Empty by default  |
| OBX-10  | Nature of Abnormal Test (ID)  |  | Empty by default  |
| OBX-11  | Observation Result Status (ID)  | F  |  |


[back to Table of Contents](#table-of-contents)

## Fifth OBX 

### Sample OBX segment: 
```
OBX|5|ST|RIGHTQUALAPP^^IRIS|5|Gradeable Image||||||F 
```

| Field  | Name and type  | Sample value  | Description  |
| -- | -- | -- | -- |
| OBX-1  | Set ID - OBX (SI)  | 5  | Increase sequentially by 1  |
| OBX-2  | Value Type (ID)  | ST  | Always "ST" (i.e. String for OBX- 5)  |
|  OBX-3  | Observation Identifier (CE)  | RIGHTQUALAPP^^IRIS  |  |
|   | OBX-3.1 Identifier  | RIGHTQUALAPP   | Always "RIGHTQUALAPP"; stands for RIGHT eye image quality.  |
|   | OBX-3.2 Text  |  | Empty by default  |
|  | OBX-3.3 Name of Coding System  | IRIS  | Always "IRIS"  |
| OBX-4  | Observation Sub-Id (ST)  | 5  | Sequential counter that matches the Set ID  |
| OBX-5  | Observation Value (Varies)  | Gradable Image   | Either “Gradable Image” or “Not Gradable Image” based on the image quality  |
| OBX-6  | Units (CE)  |   | Empty by default  |
| OBX-7  | References Range (ST)  |   | Empty by default  |
| OBX-8  | Abnormal Flags (IS)  |   | Empty by default  |
| OBX-9  | Probability (NM)  |   | Empty by default  |
| OBX-10  | Nature of Abnormal Test (ID)  |  | Empty by default  |
| OBX-11  | Observation Result Status (ID)  | F  |  |


[back to Table of Contents](#table-of-contents)

## Sixth OBX 

### Sample OBX segment: 
```
OBX|6|ST|LEFTDIABRETIN^^IRIS|6|Proliferative|||AA|||F 
```

| Field  | Name and type  | Sample value  | Description  |
| -- | -- | -- | -- |
| OBX-1  | Set ID - OBX (SI)  | 6  | Increase sequentially by 1  |
| OBX-2  | Value Type (ID)  | ST  | Always “ST” (i.e. String for OBX- 5)  |
|  OBX-3  | Observation Identifier (CE)  | LEFTDIABRETIN^^IRIS  |  |
|   | OBX-3.1 Identifier  | LEFTDIABRETIN   | Always "LEFTDIABRETIN"; stands for LEFT eye diabetic retinopathy  |
|   | OBX-3.2 Text  |  | Empty by default  |
|  | OBX-3.3 Name of Coding System  | IRIS  | Always "IRIS"  |
| OBX-4  | Observation Sub-Id (ST)  | 6  | Sequential counter that matches the Set ID  |
| OBX-5  | Observation Value (Varies)  | Proliferative   | One of three values based on the result findings: None, Mild, Moderate, Severe, or Proliferative  |
| OBX-6  | Units (CE)  |   | Empty by default  |
| OBX-7  | References Range (ST)  |  | Empty by default  |
| OBX-8  | Abnormal Flags (IS)  | AA   | One of three values based on the result findings: empty (Normal), A (ALERT), or AA (CRITICAL)  |
| OBX-9  | Probability (NM)  |   | Empty by default  |
| OBX-10  | Nature of Abnormal Test (ID)  |  | Empty by default  |
| OBX-11  | Observation Result Status (ID)  | F  | F = Final, C = Corrected*  |


[back to Table of Contents](#table-of-contents)

## Seventh OBX 

### Sample OBX segment: Seventh OBX 
```
OBX|7|ST|LEFTMACEDEMA^^IRIS|7|None||||||F 
```

| Field  | Name and type  | Sample value  | Description  |
| -- | -- | -- | -- |
| OBX-1  |  Set ID - OBX (SI)  | 7  | Increase sequentially by 1  |
| OBX-2  |  Value Type (ID)  | ST  | Always “ST” (i.e. String for OBX- 5)  |
|  OBX-3  |  Observation Identifier (CE)  | LEFTMACEDEMA^^IRIS  |  |
|   | OBX-3.1 Identifier  | LEFTMACEDEMA   | Always "LEFTMACEDEMA"; stands for LEFT eye macular edema  |
|   | OBX-3.2 Text  |  | Empty by default  |
|  | OBX-3.3 Name of Coding System  | IRIS  | Always "IRIS"  |
| OBX-4  |  Observation Sub-Id (ST)  | 7  | Sequential counter that matches the Set ID  |
| OBX-5  |  Observation Value (Varies)  | None   | One of three values based on the result findings: None, Mild, Moderate, or Severe  |
| OBX-6  |  Units (CE)  |   | Empty by default  |
| OBX-7  |  References Range (ST)  |   | Empty by default  |
| OBX-8  |  Abnormal Flags (IS)  |   | One of three values based on the result findings: empty (Normal), A (ALERT), or AA (CRITICAL)  |
| OBX-9  |  Probability (NM)  |   | Empty by default  |
| OBX-10  |  Nature of Abnormal Test (ID)  |  | Empty by default  |
| OBX-11  |  Observation Result Status (ID)  | F  | F = Final, C = Corrected*  |


[back to Table of Contents](#table-of-contents)

## Eighth OBX 

### Sample OBX segment: 
```
OBX|8|ST|LEFTOTHERRETIN^^IRIS|8|None||||||F 
```

| Field  | Name and type  | Sample value  | Description  |
| -- | -- | -- | -- |
| OBX-1  | Set ID - OBX (SI)  | 8  | Increase sequentially by 1  |
| OBX-2  | Value Type (ID)  | ST  | Always “ST” (i.e. String for OBX-5)  |
|  OBX-3  | Observation Identifier (CE)  | LEFTOTHERRETIN^^IRIS  | Empty by default  |
|   | OBX-3.1 Identifier  | LEFTOTHERRETIN   | Always "LEFTOTHERRETIN". Identify this OBX as a Left eye other retinopathy.  |
|   | OBX-3.2 Text  |  | Empty by default  |
|  | OBX-3.3 Name of Coding System  | IRIS  | Always "IRIS"  |
| OBX-4  | Observation Sub-Id (ST)  | 8  | Sequential counter that matches the Set ID  |
| OBX-5  | Observation Value (Varies)  | None, Suspected Glaucoma, Suspected Dry AMD, Suspected Wet AMD, Suspected Vein Occlusion, Suspected HTN Retinopathy, Suspected Epiretinal Membrane, Suspected Macular Hole, Suspected Cataracts, Suspected Geographic Atrophy, Other  based on result.  | Providers can choose to apply a predefined description (see Sample Value) or populate with a free-text note  |
| OBX-6  | Units (CE)  |   | Empty by default  |
| OBX-7  | References Range (ST)  |   | Empty by default  |
| OBX-8  | Abnormal Flags (IS)  |   | One of three values based on the result findings: empty (Normal), or A (ALERT)  |
| OBX-9  | Probability (NM)  |   | Empty by default  |
| OBX-10  | Nature of Abnormal Test (ID)  |  | Empty by default  |
| OBX-11  | Observation Result Status (ID)  | F  |  |


[back to Table of Contents](#table-of-contents)

## Ninth OBX 

### Sample OBX segment: 
```
OBX|9|ST|LEFTQUALAPP^^IRIS|9|Gradeable Image||||||F 
```

| Field  | Name and type  | Sample value  | Description  |
| -- | -- | -- | -- |
| OBX-1  | Set ID - OBX (SI)  | 9  | Increase sequentially by 1  |
| OBX-2  | Value Type (ID)  | ST  | Always “ST” (i.e. String for OBX- 5)  |
|  OBX-3  | Observation Identifier (CE)  | LEFTQUALAPP^^IRIS  |  |
|   | OBX-3.1 Identifier  | LEFTQUALAPP   | Always "LEFTQUALAPP"; stands for LEFT eye image quality  |
|   | OBX-3.2 Text  |  | Empty by default  |
|  | OBX-3.3 Name of Coding System  | IRIS  | Always "IRIS"  |
| OBX-4  | Observation Sub-Id (ST)  | 9  | Sequential counter that matches the Set ID  |
| OBX-5  | Observation Value (Varies)  | Gradable Image   | Either “Gradable Image” or “Not Gradable Image” based on the image quality  |
| OBX-6  | Units (CE)  |   | Empty by default  |
| OBX-7  | References Range (ST)  |   | Empty by default  |
| OBX-8  | Abnormal Flags (IS)  |   | Empty by default  |
| OBX-9  | Probability (NM)  |   | Empty by default  |
| OBX-10  | Nature of Abnormal Test (ID)  |  | Empty by default  |
| OBX-11  | Observation Result Status (ID)  | F  |  |


[back to Table of Contents](#table-of-contents)

## Tenth Through Second-to-Last OBX 

Sample OBX segment (default): 
```
OBX|10|FT|Result^^IRIS|001|Retinal Study Result for DOE, JOHN||||||F 
OBX|11|FT|Result^^IRIS|002|||||||F 
OBX|12|FT|Result^^IRIS|003|DOE, JOHN, a 58 y/o, M (DOB: 10-12-1958, MRN:ITCC20170410)||||||F 
OBX|28|FT|Result^^IRIS|019|This result was electronically signed by DOE, JANE, MD on 04-102017 09:59:02 UTC time.||||||F 
OBX|29|FT|Result^^IRIS|020|||||||F 
OBX|30|FT|Result^^IRIS|021|NOTE: Any pathology noted on this diabetic retinal evaluation should be confirmed by an appropriate ophthalmic examination.||||||F 
```
Secondary Option (Single OBX): Iris can send the text results can be sent in a single OBX segment 

Tertiary Option (NTE Segments): Iris can send the text results as NTE segments instead of OBX segments 


| Field  | Name and type  | Sample value  | Description  |
| -- | -- | -- | -- |
| OBX-1  | Set ID - OBX (SI)  | 10  | Increase sequentially by 1  |
| OBX-2  | Value Type (ID)  | FT  | Formatted Text, but can be defined differently like "ST" if  necessary  |
|  OBX-3  | Observation Identifier (CE)  | Result^^IRIS  |  |
|   | OBX-3.1 Identifier  | Result   | "Result" but can be defined withother value if necessary  |
|   | OBX-3.2 Text  |  | Usually be empty but can be defined with any value if necessary  |
|  | OBX-3.3 Name of Coding System  | IRIS  | “IRIS” is used as the default value  |
| OBX-4  | Observation Sub-Id (ST)  | 001  | Grading specific identifier; this number increases sequentially by 1 for each additional OBX segment  |
| OBX-5  | Observation Value (Varies)  |   | All the text from the Iris report will be displayed here on its own OBX line. Carriage returns will also receive their own OBX segment.  |
| -- | -- | -- | -- |
| OBX-6  | Units (CE)  |   | Empty by default  |
| OBX-7  | References Range (ST)  |   | Empty by default  |
| OBX-8  | Abnormal Flags (IS)  |   | Empty by default  |
| OBX-9  | Probability (NM)  |   | Empty by default  |
| OBX-10  | Nature of Abnormal Test (ID)  |  | Empty by default  |
| OBX-11  | Observation Result Status (ID)  | F  |  |


[back to Table of Contents](#table-of-contents) 

## Last OBX 
*Reference Pointer Option*

### Sample OBX segment: 
```
OBX|31|RP|LINK^^PDFLINK|31|https://api.retinalscreenings.com/api/PatientOrders/GetSingle ResultForDisplayInEmr?patientOrderId=273013&asPdf=True&isPreliminary=False&auth=257805B4B262A18271B60A62026671703D0F3F335DC9E0643662358952E7DCBA19C1C2CCC6C781A3444D651C0AC6695750DA87C9A5CE0F5976E8A206D9FB1915||||||F 
```

| Field  | Name and type  | Sample value  | Description  |
| -- | -- | -- | -- |
| OBX-1  | Set ID - OBX (SI)  | 31  | Increase sequentially by 1  |
| OBX-2  | Value Type (ID)  | RP  | Reference Pointer (RP): used when a pointer URL is configured  |
| OBX-3  | Observation Identifier (CE)  |   |   |
|   | Reference Pointer (RP) Configuration  | LINK^^PDFLINK  | This is the default delivery method  |
|   | OBX-3.1 Identifier  | LINK  | "LINK" is the default value  |
|   | OBX-3.2 Text  |   | Empty by default  |
|   | OBX-3.3 Name of Coding System  | PDFLINK  | "PDFLINK" is used as the default value  |
| OBX-4  | Observation Sub-Id (ST)  | 31  | Sequential counter that corresponds with the Set ID  |
| OBX-5  | Observation Value (Varies)  |   | This is a link to the PDF stored in Iris  |
|   | OBX-5.1 Reference Pointer  | See Note 1.0 Below  | See Note 1.1 Below  |
| OBX-6  | Units (CE)  |   | Empty by default  |
| OBX-7  | References Range (ST)  |   | Empty by default  |
| OBX-8  | Abnormal Flags (IS)  |   | Empty by default  |
| OBX-9  | Probability (NM)  |   | Empty by default  |
| OBX-10  | Nature of Abnormal Test (ID)  |   | Empty by default  |
| OBX-11  | Observation Result Status (ID)  | F  |   |


Note 1.0: 

## Example URL: 

https://api.retinalscreenings.com/api/PatientOrders/GetSingleResultForDisplayIn Emr?patientOrderId=12345&asPdf=True&isPreliminary=False&auth=xxxxx

## Note 1.1: 

IRIS will populate "&" as a value instead of HL7 standard value "\T\" for client. Upon request, IRIS can populate “\T\” instead of"8". Example: 

o "…?patientOrderId=273013\T\asPdf..." to "…?patientOrderId=273013&asPdf..." 

Patient MRN is displayed here: 

o patientOrderId=12345

Document type: 

o  aspdf=True

Document status (will always be set to False) 

o isPreliminary=False

Document pointer 

o  auth=xxxxx

## Sample OBX segment (Encapsulated Data):
```
OBX|27|ED|||PDF^TEXT^^Base64^[ Base64 Encoded PDF]||||||F 
```

| Field  | Name and type  | Sample value  | Description  |
| -- | -- | -- | -- |
| OBX-1  | Set ID - OBX (SI)  | 27  | Increase sequentially by 1  |
| OBX-2  | Value Type (ID)  | ED  | Encapsulated Data (ED): used when an encoded PDF is configured; this configuration can be used in addition to or in lieu of the RP configuration  |
| OBX-3  | Observation Identifier (CE)  |   | Empty by default  |
| OBX-4  | Observation Sub-Id (ST)  |   | Empty by default  |
| OBX-5  | Observation Value (Varies)  |   |   |
|   | Encapsulated Data (ED)  | PDF^TEXT^^Base64^[Base64 Encoded PDF]  |   |
|   | OBX-5.1 Source application  | PDF  | This is the document type  |
|   | OBX-5.2 Type of data  | TEXT  | “TEXT” is the default value  |
|   | OBX-5.3 Data subtype  |   | Empty by default  |
|   | OBX-5.4 Encoding  | Base64  | Always "Base64" which matchesthe encoding  |
|   | OBX-5.4 Data (ST)  | See Note 2.0 Below  | Actual PDF data using base64 encoding mechanism  |
| OBX-6  | Units (CE)  |   | Empty by default  |
| OBX-7  | References Range (ST)  |   | Empty by default  |
| OBX-8  | Abnormal Flags (IS)  |   | Empty by default  |
| OBX-9  | Probability (NM)  |   | Empty by default  |
| OBX-10  | Nature of Abnormal Test (ID)  |   | Empty by default  |
| OBX-11  | Observation Result Status (ID)  | F  | F = Final, C = Corrected*  |


Note 2.0: This will contain a Base64 encoded pdf document. An example can be provided upon request.

[back to Table of Contents](#table-of-contents)

***

## Example Messages

### Example A
```
MSH|^\~\&|IRIS|IRIS|Vendor|Vendor|20191220174508||ORU^R01|191220174508|T|2.3 
PID||Test2106462|Test2106462||TestLN^TestFN||19630130|M||||||||||293907374|
PV1|1|O|WRSIM||||FakeNPInum^DocLastName^ROBERT^^^MD^MD^^^^^^NPI|FakeNPInum^DocLastName^ROBERT^^^MD^MD^^^^^^NPI|||||||||FakeNPInum^DocLastName^ROBERT^^^MD^MD^^^^^^NPI||293907374|||||||||||||||||||||||||20191220054252|20191220054252 
ORC|RE|111997409|799248^IRIS||F||||20191220174508|||FakeNPInum^DocLastName^ROBERT^^^MD^MD^^^^^^NPI 
OBR|1|111997409|799248^IRIS|^FUNDUS PHOTOGRAPHY^EAP^^FUNDAL PHOTO|||20191220054252|||||||20191220054252||FakeNPInum^DocLastName^ROBERT^^^MD^M D^^^^^^NPI||||||20191220054412|||F|||||||adiaz@retinalscreenings.com^Diaz^Adam^^Adam Diaz, MD, NPI: NPI0987654, Taxonomy: 207W00000X 
OBX|1|ST|SEVERITY^^IRIS|1|NORMAL||||||F 
OBX|2|ST|RIGHTDIABRETIN^^IRIS|2|None||||||F 
OBX|3|ST|RIGHTMACEDEMA^^IRIS|3|None||||||F 
OBX|4|ST|RIGHTOTHERRETIN^^IRIS|4|None||||||F
OBX|5|ST|RIGHTQUALAPP^^IRIS|5|Gradable Image||||||F 
OBX|6|ST|LEFTDIABRETIN^^IRIS|6|None||||||F 
OBX|7|ST|LEFTMACEDEMA^^IRIS|7|None||||||F 
OBX|8|ST|LEFTOTHERRETIN^^IRIS|8|None||||||F 
OBX|9|ST|LEFTQUALAPP^^IRIS|9|Gradable Image||||||F 
OBX|10|FT|Result^^IRIS|001|Retinal Study Result for TestFN TestLN||||||F 
OBX|11|FT|Result^^IRIS|002|||||||F 
OBX|12|FT|Result^^IRIS|003|TestFN TestLN, a 56 y/o, M (DOB: 01-30-1963, MRN: Test2106462)||||||F 
OBX|13|FT|Result^^IRIS|004|presented to Iris Test Client on 12-20-2019 for a retinal imaging study of the left and right eyes.||||||F 
OBX|14|FT|Result^^IRIS|005|||||||F 
OBX|15|FT|Result^^IRIS|006|Based on the findings of the study, the following is recommended for TestFN TestLN||||||F TestFN TestLN||||||F 
OBX|16|FT|Result^^IRIS|007|Normal Scan: Please advise the patient to return for a scan in 12 months.||||||F 
OBX|17|FT|Result^^IRIS|008|||||||F
OBX|18|FT|Result^^IRIS|009|Interpreting Provider's Comments: No comments provided||||||F 
OBX|19|FT|Result^^IRIS|010|||||||F 
OBX|20|FT|Result^^IRIS|011|Right eye findings: Normal Result. Negative for Diabetic Retinopathy.||||||F 
OBX|21|FT|Result^^IRIS|012|||||||F
OBX|22|FT|Result^^IRIS|013|Left eye findings: Normal Result. Negative for Diabetic Retinopathy.||||||F 
OBX|23|FT|Result^^IRIS|014|||||||F
OBX|24|FT|Result^^IRIS|015|||||||F
OBX|25|FT|Result^^IRIS|016|This result was electronically signed by Diaz, Adam, on 12-20-2019 05:44:12 UTC time.||||||F 
OBX|26|FT|Result^^IRIS|017|||||||F
OBX|27|FT|Result^^IRIS|018|NOTE: Any pathology noted on this diabetic retinal evaluation should be confirmed by an appropriate ophthalmic examination.||||||F 
OBX|27|ED|||PDF^TEXT^^Base64^&#36;{ENCODED DOCUMENT REMOVED}||||||F 
```
### Example B
```
MSH|^\~\&|IRIS|IRIS|Vendor|Vendor|20191223193115||ORU^R01|191223193115|P|2.3 
PID||MRNtest123|MRNtest123||TESTB^IRIS||19391126|F||||||||||123456789|
PV1|1|O|WRSIM||||NPI5556666^DocLastName^MAZHAR^^^MD^MD^^^^^^NPI|NPI5556666^DocLastName^MAZHAR^^^MD^MD^^^^^^NPI|||||||||NPI5556666^DocLastName^MAZHAR^^^MD^MD^^^^^^NPI||123456789|||||||||||||||||||||||||20191223012424|20191223012424 
ORC|RE|12378912|799932^IRIS||F||||20191223193115|||NPI5556666^DocLastName^MAZHAR^^^MD^MD^^^^^^NPI 
OBR|1|12378912|799932^IRIS|^FUNDUS PHOTOGRAPHY^EAP^^FUNDAL PHOTO|||20191223012424|||||||20191223012424||NPI5556666^DocLastName^MAZHAR^^^MD^M D^^^^^^NPI||||||20191223012555|||F|||||||1234567890^Diaz^Adam^^^MD^MD^^^^^^NPI 
OBX|1|ST|SEVERITY^^IRIS|1|CRITICAL|||AA|||F 
OBX|2|ST|LEFTDIABRETIN^^IRIS|2|Severe|||AA|||F 
OBX|3|ST|LEFTMACEDEMA^^IRIS|3|Severe|||AA|||F 
OBX|4|ST|LEFTOTHERRETIN^^IRIS|4|Suspected Vein Occlusion|||A|||F 
OBX|5|ST|LEFTQUALAPP^^IRIS|5|Gradable Image||||||F 
OBX|6|ST|RIGHTDIABRETIN^^IRIS|6|Moderate|||A|||F 
OBX|7|ST|RIGHTMACEDEMA^^IRIS|7|Moderate|||A|||F
OBX|8|ST|RIGHTOTHERRETIN^^IRIS|8|Suspected Dry AMD|||A|||F 
OBX|9|ST|RIGHTQUALAPP^^IRIS|9|Gradable Image||||||F 
OBX|10|FT|Result^^IRIS|001|Retinal Study Result for IRIS TESTB||||||F 
OBX|11|FT|Result^^IRIS|002|||||||F 
OBX|12|FT|Result^^IRIS|003|IRIS TESTB, a 80 y/o, F (DOB: 11-26-1939, MRN: MRNtest123)||||||F 
OBX|13|FT|Result^^IRIS|004|presented to Southcoast Health - WRS Internal Medicine on 12-23-2019 for a retinal imaging study of the left and right eyes.||||||F 
OBX|14|FT|Result^^IRIS|005|||||||F 
OBX|15|FT|Result^^IRIS|006|Based on the findings of the study, the following is recommended for IRIS TESTB||||||F 
OBX|16|FT|Result^^IRIS|007|Next Available Appointment: Refer patient to a retina specialist, next available appointment.||||||F available appointment.||||||F 
OBX|17|FT|Result^^IRIS|008|||||||F
OBX|18|FT|Result^^IRIS|009|Interpreting Provider's Comments: No comments provided||||||F 
OBX|19|FT|Result^^IRIS|010|Diagnoses Present: E11.3311 - Type 2 diabetes mellitus with moderate nonproliferative diabetic retinopathy with macular edema, right eye||||||F 
OBX|20|FT|Result^^IRIS|011|E11.3412 - Type 2 diabetes mellitus with severe nonproliferative diabetic retinopathy with macular edema, left eye||||||F 
OBX|21|FT|Result^^IRIS|012|||||||F
OBX|22|FT|Result^^IRIS|013|Right eye findings: Diabetic Retinopathy: Moderate||||||F 
OBX|23|FT|Result^^IRIS|014|Macular Edema: Moderate||||||F 
OBX|24|FT|Result^^IRIS|015|Other: Suspected Dry AMD||||||F 
OBX|25|FT|Result^^IRIS|016|||||||F
OBX|26|FT|Result^^IRIS|017|Left eye findings: Diabetic Retinopathy: Severe||||||F 
OBX|27|FT|Result^^IRIS|018|Macular Edema: Severe||||||F 
OBX|28|FT|Result^^IRIS|019|Other: Suspected Vein Occlusion||||||F 
OBX|29|FT|Result^^IRIS|020|||||||F
OBX|30|FT|Result^^IRIS|021|||||||F
OBX|31|FT|Result^^IRIS|022|This result was electronically signed by Diaz, Adam, on 12-23-2019 07:25:55 UTC time.||||||F 07:25:55 UTC time.||||||F 
OBX|32|FT|Result^^IRIS|023|||||||F 
OBX|33|FT|Result^^IRIS|024|NOTE: Any pathology noted on this diabetic retinal evaluation should be confirmed by an appropriate ophthalmic examination.||||||F 
OBX|34|RP|LINK^^PDFLINK|34|https://api.retinalscreenings.com/api/PatientOrders/GetSingleResultFo rDisplayInEmr?patientOrderId=799932\T\asPdf=True\T\isPreliminary=False\T\auth=6DCAFF6AC2A555F 00F9E470D221B6A077C3497A668B1EEBBB4983C8D98672F8FBA00707190026B817325C2A088725B5A0E5D7AB659AC0790C1C1D22B2C50F897\T\asAddendum=False||||||F 
```

### Example C

```
MSH|^\~\&|IRIS|IRIS|Vendor|Vendor|20200103185623||ORU^R01|200103185623|T|2.3 
PID||15852|15852||Tester^Patient1||19990312|M|||||||||||
PV1|1|O|Iris Test Client||||aalderman@retinalscreenings.com^Alderman^Allan^^^PhD^PhD^^^^^^NPI|aalderman@reti nalscreenings.com^Alderman^Allan^^^PhD^PhD^^^^^^NPI|||||||||aalderman@retinalscreenings.co m^Alderman^Allan^^^PhD^PhD^^^^^^NPI||Encounter|||||||||||||||||||||||||20191218044543|20191218044543 
ORC|RE|7e39fa9b-27d6-4792-87692f9f422f187a|797619^IRIS||F||||20200103185623|||aalderman@retinalscreenings.com^Alderman^A llan^^^PhD^PhD^^^^^^NPI 
OBR|1|7e39fa9b-27d6-4792-8769-2f9f422f187a|797619^IRIS|92250^FUNDUS PHOTOGRAPHY^EAP^^FUNDAL PHOTO|||20191218044543|||||||20191218044543||aalderman@retinalscreenings.com^Alderman^A llan^^^PhD^PhD^^^^^^NPI||||||20191220023008|||F|||||||NPIorCredentials^LastName^Ryan^20191220023008^LastName Carter, MD, NPI: 1902127947, Taxonomy: 207W00000X 
OBX|1|ST|SEVERITY^^IRIS|1|NORMAL||||||F 
OBX|2|ST|LEFTDIABRETIN^^IRIS|2|None||||||F 
OBX|3|ST|LEFTMACEDEMA^^IRIS|3|None||||||F 
OBX|4|ST|LEFTOTHERRETIN^^IRIS|4|None||||||F 
OBX|5|ST|LEFTQUALAPP^^IRIS|5|Gradable Image||||||F 
OBX|6|ST|RIGHTDIABRETIN^^IRIS|6|None||||||F 
OBX|7|ST|RIGHTMACEDEMA^^IRIS|7|None||||||F 
OBX|8|ST|RIGHTOTHERRETIN^^IRIS|8|None||||||F 
OBX|9|ST|RIGHTQUALAPP^^IRIS|9|Gradable Image||||||F 
NTE|1|ST|Retinal Study Result for Patient1 Tester||||||||F
NTE|2|ST|||||||||F 
NTE|3|ST|Patient1 Tester, a 20 y/o, M (DOB: 03-12-1999, MRN: 15852)||||||||F
NTE|4|ST|presented to Iris Test Client on 12-18-2019 for a retinal imaging study of the left and right eyes.||||||||F 
NTE|5|ST|||||||||F 
NTE|6|ST|Based on the findings of the study, the following is recommended for Patient1 Tester||||||||F 
NTE|7|ST|Normal Scan: Please advise the patient to return for a scan in 12 months.||||||||F NTE|8|ST|||||||||F 
NTE|9|ST|Interpreting Provider's Comments: No comments provided||||||||F
NTE|10|ST|||||||||F 
NTE|11|ST|Right eye findings: Normal Result. Negative for Diabetic Retinopathy.||||||||F 
NTE|12|ST|||||||||F 
NTE|13|ST|Left eye findings: Normal Result. Negative for Diabetic Retinopathy.||||||||F 
NTE|14|ST|||||||||F 
NTE|15|ST|||||||||F 
NTE|16|ST|This result was electronically signed by LastName, Ryan, MD, NPI: NPIorCredentials, Taxonomy: 207W00000X on 12-20-2019 02:30:08 UTC time.||||||||F 
NTE|17|ST|||||||||F 
NTE|18|ST|NOTE: Any pathology noted on this diabetic retinal evaluation should be confirmed by an appropriate ophthalmic examination.||||||||F 
```

***

## Revision History


| Row No.  | Revision Date  | Version Update  | Revision Notes  |
| -- | -- | -- | -- |
| 1  | Sept. 26, 2018  | Version 1.0  | Initial documentation creation Updated version  |
| 2  | Oct. 3, 2018  | Version 2.0  | Corrected typos in several segment section descriptions Added footnotes to some sections to add additional descriptions (where applicable) Added “Examples” section Updated version  |
| 3  | Jan. 8, 2019  | Version 2.1  | Added a “Revision History” section Updated version  |
| 4  | Jan. 3, 2020  | Version 2.2  | Updated examples and improved readability Corrected grammatical typos  |
| 5  | Jan. 21, 2020  | Version 2.3  | Added criteria for an optional DG1 segment Updated version  |
| 6  | Jun. 6, 2021  | Version 2.4  | Updated version  |
| 7  | Aug. 31, 2021  | OBS  | Moving to OBS – will re-implement as TEC document |
| 8  | Aug 31, 2021  | TEC 005, Rev A  | Re-implemented as TEC document  |
| 9  | Sep. 22, 2023  | Version 2.5  | Removed version from Acknowledgement section. Updated PID.19 description. Updated PV1.7.13, PV1.8.13, and PV1.17.13 descriptions. Updated ORC.12.13 description. Updated OBR.4 description. Updated OBX Summary description. Updated all OBX.11 descriptions. Updated Other descriptions (OBX.5). Corrected typos in Text Report description (OBX.5). Updated Version.  |

 [back to Table of Contents](#table-of-contents)
