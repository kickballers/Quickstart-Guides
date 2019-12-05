## Creating an Identity

The mechanism for creating identities is influenced by how your Ziti network is setup, specifically how the PKI is
established. Identities are itegrally linked to the PKI configured in a given Ziti network and directly affects how
identites are created. There are generally three groups of identities which can be created:

* One Time Token (ott) identites using the configured PKI
* One Time Token (ott) identites using a 3rd Party CA
* 3rd Party auto-enrolled identities

## Choosing an Identity Type

Choosing which type of identity you are creating comes down to whether you are using a 3rd Party CA or not. If the
network does not have a 3rd Party Certificate configured the only option is to use the One Time Token identity.

If one or more 3rd Party CA is installed you will need to understand the intention of each 3rd Party certificate.

Each of the types of identities are secure it just depends on your actual network setup as to which type to choose. If
you don't know - just use the one time token identity. The identity can always be recreated at a later date and replaced
if necessary.

### One Time Token (OTT)

One time token identities are the type of identities available to all Ziti networks.  A one time token identity will
have a token generated at the time of the identity's creation.  This token is then submitted at some point in the future
as part of the [enrollment](./enrolling.md) process.  Once an identity is successfully enrolled - the one time token is
no longer valid and cannot be used to enroll the same identity again.

One time tokens are delivered from the Ziti Controller as a [jwt](https://tools.ietf.org/html/rfc7519) and the token
expires 24 hours after the identity is created.  The token is downloadable via the basic UI provided in the [Ziti Edge -
Developer Edition](https://aws.amazon.com/marketplace/pp/B07YZLKMLV). After you create a user you can go to the Identities page and
click the icon that looks like a certificate to download the .jwt file.

You can also create a one time token identity using the `ziti` cli tool available on the path of the [Ziti Edge -
Developer Edition](https://aws.amazon.com/marketplace/pp/B07YZLKMLV).  This command will create a new identity and output the jwt
to the selected path. You can then transfer the .jwt file to its intended destination.

[!include[](./create-identity-cli.md)]

### 3rd Party CA - Overview

The Ziti controller is capable of using an existing PKI for authentication and authorization rather than to PKI
configured in the Ziti Controller.  Certificates that are not controlled by the Ziti controller are referred to as "3rd
party". If you have an existing PKI setup you wish to reuse or if you are just interested in learning how
to use a 3rd Party CA this section is for you.

> [!NOTE]
Reusing a PKI is not a simple topic and managing and maintaining a PKI is out of the scope of this guide.

A 3rd Party CA will need to be created and the public certificate uploaded into the Ziti Controller. After using an
existing PKI to reuse/generate a certificate, the Ziti Controller will be to create identities which will be expected to
present a certificate during the connection process that is valid per the provided certificate.

#### Adding a 3rd Party CA to the Ziti Controller

Adding a certifate to the Ziti Controller is easy using the Ziti Console provided in the [Ziti Edge -
Developer Edition](https://aws.amazon.com/marketplace/pp/B07YZLKMLV).

#### [New CA via UI](#tab/tabid-new-ca-ui)

1. On the left side click "Certificate Authorities"
1. In the top right corner of the screen click the "plus" image to add a new Certificate Authority
1. Enter the name of the Certificate Authority you would like to create
1. Choose if the CA should be used for Enrollment (yes) and Auth (yes)
1. Click save

#### [New CA via REST](#tab/tabid-new-ca-cli)

[!include[](../../api/rest/create-ca-json.md)]

***

#### 3rd Party CA - One Time Token

3rd Party CA OTT enrollment is closely related to [OTT Enrollment](#one-time-token-ott). The main difference is the
utilization of a 3rd party CA certificate rather than the configured Ziti Edge CA and PKI. In this method, the system
does not have access to the 3rd party CA private key and thus cannot provide CSR fulfillment capabilities. Instead it is
assumed that the enrolling device has a separate process to acquire signed certificates. Rather than submitting a CSR
the client uses an already acquired signed certificate as its client certificate for the enrollment request. The client
certificate is validated against the CA certificate tied to the one time token.

Similar to the [OTT Enrollment](#one-time-token-ott) process, identities must be provisioned ahead of enrollment in
order to generate one time token required and to creat the jwt that can be delivered to enrolling devices. This means
that the provisioning of the Ziti Edge identities and the client certificates must be coordinated. Identities can be enrolled with a one time token flow similar to the [one time token flow](#one-time-token-ott).

#### 3rd Party CA - Auto Enrolled

CA Auto Enrollment is useful in situations where devices are provisioned with certificates en-mass that need to be able
to register as identities within Ziti Edge. This enrollment method allows for device provisioning processes to skip the
manual configuration of Ziti Edge and instead allow clients to present a signed client certificate to generate an
identity during the enrollment process. The identity will grant the client access to authenticate only - any
authorization will need to be done after the device identities have been created.

A certificate can only be used for one identity. The Ziti Edge system does not allow the same certificate to be used for
multiple identities. An enrollment request is comprised of a special enrollment URL used to perform an HTTP POST request
using the signed client certificate as the TLS client certificate and an optional JSON payload that allows the client to
specify the devices display name and internal username. See [enrollment](./enrolling.md) for more details on enrolling.

### [New Identity via UI](#tab/tabid-new-identity-ui)

1. On the left side click "Certificate Authorities"
1. In the top right corner of the screen click the "plus" image to add a new Certificate Authority
1. Enter the name of the Certificate Authority you would like to create
1. Choose if the CA should be used for Enrollment (yes) and Auth (yes)
1. Click save

### [New Identity via UI](#tab/tabid-new-identity-cli)

[!include[](./create-identity-cli.md)]

***