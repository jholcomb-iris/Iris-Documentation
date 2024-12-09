---
layout: page
---

## EMR Integrations
IRIS Provides integration services to  *on premise* amd most cloud based EMR systems.  

# On Premise EMR/EHR systems
On Premise systems are supported by the IRIS EMR Proxy application.  

### IRIS EMR Proxy application
IRIS is a SAAS service hosted in the Azure cloud environment.  In order to provide seamless integration to your on premise EMR/EHR IRIS provides the EMR Proxy application that you install on a Windows server in the data center where your EMR/EHR application resides. The primary function of the EMR Proxy is to provide encryption services for HL7 data in transit between your Integration engine and the IRIS Cloud services.

For details on the EMR Proxy application follow this link to [IRIS EMR Proxy Application Requirements and Specifications](./EMRProxyReqAndSpecs.html)


## Cloud based EMR/EHR systems
Cloud based EMRs have unique integration requirements involving the provider but can typically be setup quickly. 

Examples of Cloud EMR systems include: Athena, OCHIN and ECW.



## HL7
Integration to your EMR system is accomodated by using standard HL7 messages for receiving orders and returning results.

Orders are submitted from your integration engine to the IRIS system using the standard [HL7 ORM Message](ORMSpecifications.html)

Results are returned to your integration engine with the standard [HL7 ORU Message](./TEC_007_Rev_B_Standard_Results.html)





