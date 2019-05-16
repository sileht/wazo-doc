# Service Authentication

Wazo services expose more and more resources through REST API, but they
also ensure that the access is restricted to the authorized programs.
For this, we use an <span data-role="ref">authentication daemon
\<wazo-auth\></span> who delivers authorizations via tokens.

## Call flow

Here is the call flow to access a REST resource of a Wazo service:

1.  Create a username/password (also called service\_id/service\_key)
    with the right <span data-role="ref">ACLs
    \<rest-api-acl\></span>, via wazo-auth.
2.  <span data-role="ref">Create a token \<wazo-auth\></span> with these
    credentials.
3.  <span data-role="ref">Use this token
    \<rest-api-authentication\></span> to access the REST resource
    defined by the <span data-role="ref">ACL \<rest-api-acl\></span>.

![Call flow of service
authentication](images/service_authentication_workflow.png)

  - Service  
    Service who needs to access a REST resource.

  - xivo-{daemon}  
    Server that exposes a REST resource. This resource must have an
    attached ACL.

  - wazo-auth  
    Server that authenticates the Service and validates the required ACL
    with the token.

Wazo services directly use this system to communicate with each other,
as you can see in their Web Services Access.
