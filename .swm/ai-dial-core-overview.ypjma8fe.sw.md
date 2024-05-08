---
title: ai-dial-core overview
---
The ai-dial-core repository is a HTTP Proxy that provides a unified API for various chat completion and embedding models, assistants, and applications. It acts as a middleware to facilitate communication and data exchange between different AI models and client applications, streamlining the integration and usage of AI technologies in diverse environments.

- <SwmLink doc-title="Getting started">[Getting started](.swm/getting-started.01o4pw77.sw.md)</SwmLink>

## Modules

### Proxy Services

- <SwmLink doc-title="Proxy services implementation and usage">[Proxy services implementation and usage](.swm/proxy-services-implementation-and-usage.mrvct87b.sw.md)</SwmLink>
- <SwmLink doc-title="Performance and security considerations for proxy services">[Performance and security considerations for proxy services](.swm/performance-and-security-considerations-for-proxy-services.wi5cou3l.sw.md)</SwmLink>
- <SwmLink doc-title="Interactions between proxy services">[Interactions between proxy services](.swm/interactions-between-proxy-services.aun5ovnk.sw.md)</SwmLink>
- <SwmLink doc-title="Util classes functionality and implementation">[Util classes functionality and implementation](.swm/util-classes-functionality-and-implementation.fsv3snmm.sw.md)</SwmLink>
- **Proxy**
  - <SwmLink doc-title="Proxy implementation and usage">[Proxy implementation and usage](.swm/proxy-implementation-and-usage.wtca38wb.sw.md)</SwmLink>
  - <SwmLink doc-title="Publication management flow">[Publication management flow](.swm/publication-management-flow.4z9rnzp7.sw.md)</SwmLink>
- **Controller**
  - <SwmLink doc-title="Controller implementation and usage">[Controller implementation and usage](.swm/controller-implementation-and-usage.zc8j81qm.sw.md)</SwmLink>
  - <SwmLink doc-title="Handling http requests flow">[Handling http requests flow](.swm/handling-http-requests-flow.qj7zwkzn.sw.md)</SwmLink>
  - <SwmLink doc-title="Publication rejection flow">[Publication rejection flow](.swm/publication-rejection-flow.mlavdrgj.sw.md)</SwmLink>
- **Service**
  - <SwmLink doc-title="Overview of core services">[Overview of core services](.swm/overview-of-core-services.q3gpfmlo.sw.md)</SwmLink>
  - <SwmLink doc-title="Resource access transfer flow">[Resource access transfer flow](.swm/resource-access-transfer-flow.8l8cn5ee.sw.md)</SwmLink>
  - <SwmLink doc-title="Data retrieval and caching flow in listrules">[Data retrieval and caching flow in listrules](.swm/data-retrieval-and-caching-flow-in-listrules.sm8abrpg.sw.md)</SwmLink>
- **Main Flows**
  - <SwmLink doc-title="Bufferingreadstream functionality and flow">[Bufferingreadstream functionality and flow](.swm/bufferingreadstream-functionality-and-flow.2vu48gkc.sw.md)</SwmLink>

### Security

- <SwmLink doc-title="Security implementations overview">[Security implementations overview](.swm/security-implementations-overview.mztsod2h.sw.md)</SwmLink>
- <SwmLink doc-title="Access control flow">[Access control flow](.swm/access-control-flow.4mpwiaos.sw.md)</SwmLink>
- <SwmLink doc-title="Accesstokenvalidator authentication flow">[Accesstokenvalidator authentication flow](.swm/accesstokenvalidator-authentication-flow.p9t7t2ii.sw.md)</SwmLink>

### Storage

- <SwmLink doc-title="Storage implementations and usage">[Storage implementations and usage](.swm/storage-implementations-and-usage.8k86sqmd.sw.md)</SwmLink>
- <SwmLink doc-title="Management of storage settings and endpoint configuration">[Management of storage settings and endpoint configuration](.swm/management-of-storage-settings-and-endpoint-configuration.ogr2nyek.sw.md)</SwmLink>
- **Blob Storage**
  - <SwmLink doc-title="Blob storage implementation and usage">[Blob storage implementation and usage](.swm/blob-storage-implementation-and-usage.5uwsdw4g.sw.md)</SwmLink>
  - <SwmLink doc-title="Multipart upload process">[Multipart upload process](.swm/multipart-upload-process.bksiefn8.sw.md)</SwmLink>
  - <SwmLink doc-title="Metadata construction flow in blobstorage">[Metadata construction flow in blobstorage](.swm/metadata-construction-flow-in-blobstorage.gtzn6u37.sw.md)</SwmLink>

### Configuration

- <SwmLink doc-title="Configuration">[Configuration](.swm/configuration.25jdb3yw.sw.md)</SwmLink>

### Upstream Management

- <SwmLink doc-title="Upstream management implementation">[Upstream management implementation](.swm/upstream-management-implementation.aifm15h8.sw.md)</SwmLink>

### Invitation

- <SwmLink doc-title="Invitation class implementation and usage">[Invitation class implementation and usage](.swm/invitation-class-implementation-and-usage.x0k0g07w.sw.md)</SwmLink>

### Resource

- <SwmLink doc-title="Resource implementation and usage">[Resource implementation and usage](.swm/resource-implementation-and-usage.b5g1jipc.sw.md)</SwmLink>
- <SwmLink doc-title="Resource access management flow">[Resource access management flow](.swm/resource-access-management-flow.7odp0szo.sw.md)</SwmLink>
- <SwmLink doc-title="Rule retrieval and caching process">[Rule retrieval and caching process](.swm/rule-retrieval-and-caching-process.brdh6w87.sw.md)</SwmLink>
- <SwmLink doc-title="Exploring resource existence checks across multiple storage backends">[Exploring resource existence checks across multiple storage backends](.swm/exploring-resource-existence-checks-across-multiple-storage-backends.o1j9x36b.sw.md)</SwmLink>
- <SwmLink doc-title="Transactional behaviors in resource operations">[Transactional behaviors in resource operations](.swm/transactional-behaviors-in-resource-operations.7zxxgv6l.sw.md)</SwmLink>

### Log

### Limiter

- <SwmLink doc-title="Limiter functionality and implementation">[Limiter functionality and implementation](.swm/limiter-functionality-and-implementation.d9p374x1.sw.md)</SwmLink>
- <SwmLink doc-title="Rate limiting process flow">[Rate limiting process flow](.swm/rate-limiting-process-flow.1u0nmvtv.sw.md)</SwmLink>

### Main Flows

- <SwmLink doc-title="Access control flow">[Access control flow](.swm/access-control-flow.05d0paot.sw.md)</SwmLink>
- <SwmLink doc-title="Handling resource movement and access rights">[Handling resource movement and access rights](.swm/handling-resource-movement-and-access-rights.ucog7t6y.sw.md)</SwmLink>

## Classes

- <SwmLink doc-title="Accesscontrolbasecontroller overview">[Accesscontrolbasecontroller overview](.swm/accesscontrolbasecontroller-overview.0yx8u6ze.sw.md)</SwmLink>
- <SwmLink doc-title="Deployment class overview">[Deployment class overview](.swm/deployment-class-overview.xfjcfcns.sw.md)</SwmLink>
- <SwmLink doc-title="Deploymentdata class overview">[Deploymentdata class overview](.swm/deploymentdata-class-overview.7def0xb0.sw.md)</SwmLink>
- <SwmLink doc-title="Credentialprovider overview">[Credentialprovider overview](.swm/credentialprovider-overview.5885pz8f.sw.md)</SwmLink>
- <SwmLink doc-title="Metadatabase class overview">[Metadatabase class overview](.swm/metadatabase-class-overview.bsi35ts5.sw.md)</SwmLink>

## Build Tools

- <SwmLink doc-title="Gradle configuration and usage in the project">[Gradle configuration and usage in the project](.swm/gradle-configuration-and-usage-in-the-project.6y5ytbmi.sw.md)</SwmLink>
- <SwmLink doc-title="Configuring docker for the ai dial core project">[Configuring docker for the ai dial core project](.swm/configuring-docker-for-the-ai-dial-core-project.ubqf6bcy.sw.md)</SwmLink>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYWktZGlhbC1jb3JlJTNBJTNBc3dpbW1pbw==" repo-name="ai-dial-core"><sup>Powered by [Swimm](/)</sup></SwmMeta>
