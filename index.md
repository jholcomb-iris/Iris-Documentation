---
layout: default
---

# Integrating to the IRIS Platform

The IRIS platform is built with a rich set of integration tools that allows you to operate from your unique platform using the devices you choose.  We are committed to following already adopted standards whenever possible while providing custom solutions where those standards fall short. These tools are segmented into two categories: Device and Order Processing.

## Devices
### Fundus Cameras
IRIS provides varying levels of integration to most Fundus Cameras.
> *The notable exception is Welch Allyn, whose devices are designed to restrict use outside their network.*  

There are two main points of integration with Cameras: Worklists and Images.  
> The best and most reliable workflow is one in which the patients are available for selection from the camera user interface, without the operator being required to manually enter them.  
1. Orders are submitted to IRIS via Order Processing Integration
2. Cameras, linked to Site/Departments or Operators, present the patient list
3. Images are submitted with embedded order details

The primary and preferred form of camera integration is through **DICOM**.  Generally speaking, you will find DICOM support on most desktop cameras.  When DICOM is not available, desktop cameras typically provide some form of file system sharing.  

Handheld cameras are integrated in a variety of ways.

For specific details on all cameras go to our <u>Supported Fundus Cameras</u> page.

### OCT Cameras
IRIS provides integration with OCT Cameras via **DICOM**. For information on specific OCT devices contact IRIS sales support.

## Order Processing

While the IRIS platform is fully operable through a rich suite of <a href="https://portal.retinalscreenings.com">web based tools</a>, a more seamless workflow can be achieved through integration to your own platform. There are two main points of integration: Submission and Delivery.

> Order Submission is the process of submitting patients/orders to IRIS from your system.  This can be as simple as the uploading of a spreadsheet, or as detailed as the implementation of an HL7 interface.  

> Results Delivery is the process of returning the result of an exam to your system (and/or related endpoints).  As with Submissions, there are a wide breadth of options available to meet your specific needs.

### Which integration option is best for me?

#### By Business Sector 


- Primary Care (small) <a href="https://portal.retinalscreenings.com">Basic Integrations</a> page
- Primary Care (large), IDN, FQHC [EMR Integrations](./integration/EMRIntegrations.html) page
- Home Health Care <a href="https://portal.retinalscreenings.com">Cloud Direct Integrations</a> page
- Retail / Kiosk <a href="https://portal.retinalscreenings.com">Cloud Direct Integrations</a> page
- Payers  <a href="https://portal.retinalscreenings.com">Cloud Direct Integrations</a> and/or <a href="https://portal.retinalscreenings.com">Basic Integrations</a> page

#### By Technology 

- For basic integrations revolving around file drops, SFTP or options that suppliment manual workflows, navigate to the <a href="https://portal.retinalscreenings.com">Basic Integrations</a> page
- If you are using a mainstream EMR/EHR (HL7 integrations) navigate to the <a href="https://portal.retinalscreenings.com">EMR Integrations</a> page
- If you are using a Cloud based EMR/EHR Provider (Athena, OCHIN,etc...) navigate to the <a href="https://portal.retinalscreenings.com">Cloud EMR Integrations</a> page
- If you are considering developing software, using batch oriented operations from your platform, using tools from a cloud based platform such as Salesforce, or any other custom type integration navigate to the <a href="https://portal.retinalscreenings.com">Cloud Direct Integrations</a> page