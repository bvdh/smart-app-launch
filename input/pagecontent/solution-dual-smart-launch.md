The OAuth2 flow requires a redirect for login. Multiple redirects in a row will cause issues for the application as it becomes difficult to maintain state. This is required though when notifying the user of the access it will provide (see [Considerations for Scope Consent](https://www.hl7.org/fhir/smart-app-launch/best-practices.html#considerations-for-scope-consent). This can not be avoided when the application is not controlled by the same organization.

In this section we explore the use case where it is required when the user has to provide permission to access the content in secure manner.

The approach taken in this flow is the same as a normal SMART launch, but couples several of these back-to-back. The advantage of the approach is that it does not compromise the SMART on FHIR approach and requires little to no additional specification. The disadvantage is that it requires multiple redirects with state maintained between them.

The patient context of the first launch is passed when performing the first SMART launch, triggered by the launch token. This information needs to be passed to the second launch. This requires alignment between the different authorization systems. In a way this is similar to the token exchange approach where a similar alignment is required. This is only required when patient specific scopes are used.

This is achieved by generating a `launch-token` in the EHR flow. This will be used as the `launchId` in the second flow.

#### Discovery 

The first step in the flow is discovery.

<figure>
  {% include solution-multiple-smart-launch-discovery.svg %}
</figure>

The Application retrieves the conformance information from the EHR-FHIR server (`iss1`). Based on the user_brand_endpoint, it discovers the other server(s) it wants to access (`iss1`). Next, the application retrieves the conformance information of `iss2`.

**TODO:** Determine whether it is required to indicate support for using `launch-token`s in `capabilities`.

#### Retrieve EHR Authorization token and Access token

Next, the application retrieves an authorization token and access-token from the EHR.

<figure>
  {% include solution-multiple-smart-launch-ehr-token.svg %}
</figure>

The application retrieves an authorization code as is specified by [SMART app launch](https://www.hl7.org/fhir/smart-app-launch/app-launch.html#obtain-authorization-code). Different in this approach is that the application requests the `launch-token` scope in order to get access access to a launch-token to be used in the token exchange. 

#### Retrieve Other Authorization token and Access token

Next, the application retrieves an authorization token and access-token from the other server. First it has to ensure that the current state (application state, tokens) can be retrieved after the SMART launch against the second server.

The way to do this is outside the scope of this specifications, options include: storing it on a server, storing it encrypted in session-storage, ... . Optionally all, or part of the state can be passed as the `state` in the authorization step.

<figure>
  {% include solution-multiple-smart-launch-other-token.svg %}
</figure>

The application retrieves an authorization code as is specified by [SMART app launch](https://www.hl7.org/fhir/smart-app-launch/app-launch.html#obtain-authorization-code). The `launch-token` retrieved in the previous step is used as `launchId`. 

The OtherAS retrieves a SMART backend token which is used to introspect the retrieved `launchId`. The server to access can be derived from the `aud` and/or `iss` field. When Patient scopes are required, the Patient resource is retrieved and the corresponding patient is located.

In the case the scopes contain patient specific scopes, and the corresponding patient can not be found, these scopes should be refused.

Optionally, also the equivalent resources defined in the fhirContext can be retrieved.

The user is presented with the access it is granting and an access-token is provided. After receiving the access-token, the stored state is restored.

#### Access content

Using the access-tokens, the content on both servers can be accessed.

<figure>
  {% include solution-multiple-smart-launch-content.svg %}
</figure>

#### Required changes

Changes required in the SMART application launch specification.

**CHANGE**: Allow issuing launch-tokens.
