HTTPD mod_security configuration
==

Available Environment Variables
-- 

Items in **bold** are required.

| Name                      | Default                      | Description |
| -------------             | -------------                | ----------- |
| **PROXY_PASS_LINE1**      | N/A                          | This should contain a line describing the connection to the backend service. For example ```ProxyPass "http://helloworld:8080/"```. Note that it is not necessary for the backend to expose a route, and this should point to the OpenShift Service itself. |
| TLS_KEY_PATH              | /etc/pki/httpd/tls.key       | This value should contain an absolute path to your SSL private key. |
| TLS_CERT_PATH             | /etc/pki/httpd/tls.crt       | This should contain an absolute path to your SSL certificate chain. |
| SSL_CA_PATH               | /etc/pki/rhitcerts/trust-chain.crt       | This is only required if using SSL_CLIENT_CERT_REQUIRED |
| SSL_CLIENT_CERT_REQUIRED  | Blank                        | If this property is set, then the system will expect a client certificate (2-way or mutual authentication) that is trusted based upon the file at SSL_CA_PATH. The certificate details will be passed along to the application via HTTP headers. Further details are documented below. |


If mutual authentication is enabled, then the following HTTP Headers will be added to requests to the backend:

 - SSL_CLIENT_S_DN - The Subject DN in the client certificate
 - SSL_CLIENT_I_DN - The Issuer DN in the client certificate
 - SSL_CLIENT_CERT - The client certificate, encoded in PEM format
 - SSL_CLIENT_VERIFY - Either "NONE", "SUCCESS", "GENEROUS", or "FAILED:reason"
 
 These headers will always be either set by HTTPD or empty, so they can be trusted. The connection between httpd and your backend service should be via SSL in order to insure the integrity of the connection.
 