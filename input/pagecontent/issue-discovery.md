### Issue: discovery of supported mechanisms

An App that wants to access multiple resource servers needs a way to detect what servers are available and in what way it can access those servers.

#### Discovery of available resource servers

Discovery consists of multiple elements: discovery of available servers and discovery of what mechanism to use to access those servers. 

##### Discovery of available servers

Discovery of other could be provided as part of the app-configuration or discovered. The [Brand](https://www.hl7.org/fhir/smart-app-launch/brands.html) provides an overview of other Endpoints that are available to the App. This mechanism provides a convenient mechanism to discover these services and provide information related to to them. In this specification, we will use the brand approach.

**CHOICE:** Endpoint discovery is based on Brands.

##### Discovery of mechanism to access server

This information could be added as extensions to the resources provided in the Brand bundle. Alternatively, these can be included in the configuration information. The configuration information holds the security related information and should be used as source of truth. Optionally, extensions can be added to the Brand bundle to more easily filter the endpoints to use.

**CHOICE:** Information on in what way to access a server is provided by the conformance endpoint.

##### Detect servers that can be accessed using the same access token

The client needs to detect that the other server can be addressed using the same access-token. According to the current specification this is achieved by listing the other server in  `associated-endpoints` but this will provide no information on the type of server. It would be better to move this in the brand-bundle.

The fact that access-token could be reused could be derived from the fact that both hold the same `authorization_endpoint` and `token-endpoint`. A better approach is to list it in the `capabilities` array, e.g. using:

* `auth-multi-aud-token`: support for access tokens for more than one resources server (aud).