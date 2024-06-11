This section defines a `launch-token`. A `launch-token` is an open-id token (see [Scopes for requesting identity data](https://www.hl7.org/fhir/smart-app-launch/scopes-and-launch-context.html#scopes-for-requesting-identity-data)).

This token is extended with the following claims:
* `fhirContext`, holding a json string with a serialized representation of the fhirContext from the token-response holding the full-url references of the resources in context.

**NOTE:** inclosing this as a json string is sub-optimal, a nested structure is preferred.