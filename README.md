# SiebelAzureADSAML
Siebel Integration with Azure AD


Siebel doesn't provie any documentation for integration specific to Azure AD nor does Azure AD provides any such documentation for Siebel.
Both Oracle and Mircrosoft have provided generic instructions in which there products can be integrated.

For instance, it's Oracle says that on its MOS article as below:

"Oracle does not provide specific documentation about Microsoft Azure AD, but it provides the requirements for integrating Siebel CRM with a standards-based Web SSO system in the Bookshelf Security Guide , chapter 6 ("Single Sign On Authentication").

According to the information on MS Support site, it is possible to use either SAML or header-based (using PingAccess for Azure AD plugin).

see: https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/what-is-single-sign-on

 

All configurations related to SSO must be performed outside Siebel. Siebel expects SSO authentication to be performed before the request reaches Siebel and looks at the HTTP request header injection for the subject. Contact Third party vendor for instructions on the setup"


So this means that something needs to  sit at the application tier that talks to Azure and in case of a successful authentication and does the HTTP header injection  accordingly.
Siebel SWSE recognizes this header and passes it to the Security Adapter .
For Siteminder integration there is a Siteminder agent that sits on the Webserver and at app server (CustomSecurity Adapter).

Microsoft doesn't provide anything like an agent to do this for you.
Hence, Azure AD implementation with Siebel wasn't straightforward.

Two ways for this are OIDC and SAML.
There is literature online for OIDC integration using Apache HTTPD reverse proxy.
Our application architecture has STM and I used its default capability to send SAML request to the target endpoint.
But STM doesn't provide any default capability to parese the SAML Response, this I did by creating my own traffic script which would parse the response and set the HTTP header that SWSE requires.
