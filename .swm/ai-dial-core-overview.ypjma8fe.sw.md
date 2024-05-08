---
title: ai-dial-core overview
---
The ai-dial-core repository is a HTTP Proxy that provides a unified API for various chat completion and embedding models, assistants, and applications. It acts as a middleware to facilitate communication and data exchange between different AI models and client applications, streamlining the integration and usage of AI technologies in diverse environments.

- <SwmLink doc-title="Getting started">[Getting started](.swm/getting-started.01o4pw77.sw.md)</SwmLink>

## Modules

### Proxy Services

- <SwmLink doc-title="Proxy Services Implementation and Usage">[Proxy Services Implementation and Usage](/.swm/proxy-services-implementation-and-usage.mrvct87b.sw.md)</SwmLink>
- <SwmLink doc-title="Performance and Security Considerations for Proxy Services">[Performance and Security Considerations for Proxy Services](/.swm/performance-and-security-considerations-for-proxy-services.wi5cou3l.sw.md)</SwmLink>
- <SwmLink doc-title="Interactions Between Proxy Services ">[Interactions Between Proxy Services ](/.swm/interactions-between-proxy-services.aun5ovnk.sw.md)</SwmLink>
- <SwmLink doc-title="Util Classes Functionality and Implementation">[Util Classes Functionality and Implementation](/.swm/util-classes-functionality-and-implementation.fsv3snmm.sw.md)</SwmLink>
- **Proxy**
  - <SwmLink doc-title="Proxy Implementation and Usage">[Proxy Implementation and Usage](/.swm/proxy-implementation-and-usage.wtca38wb.sw.md)</SwmLink>
  - <SwmLink doc-title="Publication Management Flow">[Publication Management Flow](/.swm/publication-management-flow.4z9rnzp7.sw.md)</SwmLink>
- **Controller**
  - <SwmLink doc-title="Controller Implementation and Usage">[Controller Implementation and Usage](/.swm/controller-implementation-and-usage.zc8j81qm.sw.md)</SwmLink>
  - <SwmLink doc-title="Handling HTTP Requests Flow">[Handling HTTP Requests Flow](/.swm/handling-http-requests-flow.qj7zwkzn.sw.md)</SwmLink>
  - <SwmLink doc-title="Publication Rejection Flow">[Publication Rejection Flow](/.swm/publication-rejection-flow.mlavdrgj.sw.md)</SwmLink>
- **Service**
  - <SwmLink doc-title="Overview of Core Services">[Overview of Core Services](/.swm/overview-of-core-services.q3gpfmlo.sw.md)</SwmLink>
  - <SwmLink doc-title="Resource Access Transfer Flow">[Resource Access Transfer Flow](/.swm/resource-access-transfer-flow.8l8cn5ee.sw.md)</SwmLink>
  - <SwmLink doc-title="Data Retrieval and Caching Flow in listRules">[Data Retrieval and Caching Flow in listRules](/.swm/data-retrieval-and-caching-flow-in-listrules.sm8abrpg.sw.md)</SwmLink>
- **Main Flows**
  - <SwmLink doc-title="BufferingReadStream Functionality and Flow">[BufferingReadStream Functionality and Flow](/.swm/bufferingreadstream-functionality-and-flow.2vu48gkc.sw.md)</SwmLink>

### Security

- <SwmLink doc-title="Security Implementations Overview">[Security Implementations Overview](/.swm/security-implementations-overview.mztsod2h.sw.md)</SwmLink>
- <SwmLink doc-title="Access Control Flow">[Access Control Flow](/.swm/access-control-flow.05d0paot.sw.md)</SwmLink>
- <SwmLink doc-title="AccessTokenValidator Authentication Flow">[AccessTokenValidator Authentication Flow](/.swm/accesstokenvalidator-authentication-flow.p9t7t2ii.sw.md)</SwmLink>

### Storage

- <SwmLink doc-title="Storage Implementations and Usage">[Storage Implementations and Usage](/.swm/storage-implementations-and-usage.8k86sqmd.sw.md)</SwmLink>
- <SwmLink doc-title="Management of Storage Settings and Endpoint Configuration">[Management of Storage Settings and Endpoint Configuration](/.swm/management-of-storage-settings-and-endpoint-configuration.ogr2nyek.sw.md)</SwmLink>
- **Blob Storage**
  - <SwmLink doc-title="Blob Storage Implementation and Usage">[Blob Storage Implementation and Usage](/.swm/blob-storage-implementation-and-usage.5uwsdw4g.sw.md)</SwmLink>
  - <SwmLink doc-title="Multipart Upload Process">[Multipart Upload Process](/.swm/multipart-upload-process.bksiefn8.sw.md)</SwmLink>
  - <SwmLink doc-title="Metadata Construction Flow in BlobStorage">[Metadata Construction Flow in BlobStorage](/.swm/metadata-construction-flow-in-blobstorage.gtzn6u37.sw.md)</SwmLink>

### Configuration

- <SwmLink doc-title="Configuration ">[Configuration ](/.swm/configuration.25jdb3yw.sw.md)</SwmLink>

### Upstream Management

- <SwmLink doc-title="Upstream Management Implementation">[Upstream Management Implementation](/.swm/upstream-management-implementation.aifm15h8.sw.md)</SwmLink>

### Invitation

- <SwmLink doc-title="Invitation Class Implementation and Usage">[Invitation Class Implementation and Usage](/.swm/invitation-class-implementation-and-usage.x0k0g07w.sw.md)</SwmLink>

### Resource

- <SwmLink doc-title="Resource Implementation and Usage">[Resource Implementation and Usage](/.swm/resource-implementation-and-usage.b5g1jipc.sw.md)</SwmLink>
- <SwmLink doc-title="Resource Access Management Flow">[Resource Access Management Flow](/.swm/resource-access-management-flow.7odp0szo.sw.md)</SwmLink>
- <SwmLink doc-title="Rule Retrieval and Caching Process">[Rule Retrieval and Caching Process](/.swm/rule-retrieval-and-caching-process.brdh6w87.sw.md)</SwmLink>
- <SwmLink doc-title="Exploring Resource Existence Checks Across Multiple Storage Backends">[Exploring Resource Existence Checks Across Multiple Storage Backends](/.swm/exploring-resource-existence-checks-across-multiple-storage-backends.o1j9x36b.sw.md)</SwmLink>
- <SwmLink doc-title="Transactional Behaviors in Resource Operations">[Transactional Behaviors in Resource Operations](/.swm/transactional-behaviors-in-resource-operations.7zxxgv6l.sw.md)</SwmLink>

### Log

### Limiter

- <SwmLink doc-title="Limiter Functionality and Implementation">[Limiter Functionality and Implementation](/.swm/limiter-functionality-and-implementation.d9p374x1.sw.md)</SwmLink>
- <SwmLink doc-title="Rate Limiting Process Flow">[Rate Limiting Process Flow](/.swm/rate-limiting-process-flow.1u0nmvtv.sw.md)</SwmLink>

### Main Flows

- <SwmLink doc-title="Access Control Flow">[Access Control Flow](/.swm/access-control-flow.05d0paot.sw.md)</SwmLink>
- <SwmLink doc-title="Handling Resource Movement and Access Rights">[Handling Resource Movement and Access Rights](/.swm/handling-resource-movement-and-access-rights.ucog7t6y.sw.md)</SwmLink>

## Classes

- <SwmLink doc-title="AccessControlBaseController Overview">[AccessControlBaseController Overview](/.swm/accesscontrolbasecontroller-overview.0yx8u6ze.sw.md)</SwmLink>
- <SwmLink doc-title="Deployment Class Overview">[Deployment Class Overview](/.swm/deployment-class-overview.xfjcfcns.sw.md)</SwmLink>
- <SwmLink doc-title="DeploymentData Class Overview">[DeploymentData Class Overview](/.swm/deploymentdata-class-overview.7def0xb0.sw.md)</SwmLink>
- <SwmLink doc-title="CredentialProvider Overview">[CredentialProvider Overview](/.swm/credentialprovider-overview.5885pz8f.sw.md)</SwmLink>
- <SwmLink doc-title="MetadataBase Class Overview">[MetadataBase Class Overview](/.swm/metadatabase-class-overview.bsi35ts5.sw.md)</SwmLink>

## Build Tools

- <SwmLink doc-title="Gradle Configuration and Usage in the Project">[Gradle Configuration and Usage in the Project](/.swm/gradle-configuration-and-usage-in-the-project.6y5ytbmi.sw.md)</SwmLink>
- <SwmLink doc-title="Configuring Docker for the ai-dial-core Project">[Configuring Docker for the ai-dial-core Project](/.swm/configuring-docker-for-the-ai-dial-core-project.ubqf6bcy.sw.md)</SwmLink>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYWktZGlhbC1jb3JlJTNBJTNBc3dpbW1pbw==" repo-name="ai-dial-core"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>
