---
title: "Transaction Processor & Families"
weight: 5
description: >
  Overview and examples of the ConsenSource Transaction Processor and Transaction Families
---

{{< alert title="GitHub Link">}}
https://github.com/target/consensource/tree/master/processor
{{< /alert >}}

ConsenSource is a certificate registry, as data pertaining to the process of factory certification is captured in the ledger state. This Certificate Registry (henceforth referred to as CR) Transaction family displays the interactions currently implemented for any participant that affects state data. Furthermore, this lays out the logic behind all interactions to verify the data submitted to the ledger.

## Addressing

CR data is stored in state using addresses generated from the CR Transaction
family name and the object type being stored. In particular, a
CR address consists of 4 parts creating a 70 hex character string.

* The first 6 characters of the SHA-256 hash of the UTF-8 encoding of the string "certificate_registry" (`439a56`).

* The next 2 characters reserve a namespace and we will start with the namespace `00`.

* The next 2 characters are determined by the object type.
  * `00` will signify an *agent* object.
  * `01` will signify a *certificate* object.
  * `02` will signify an *organization* object.
  * `03` will signify a *standard* object.
  * `04` will signify a *request* object.

* The final 60 characters is a truncated value of the SHA-256 hash of the UTF-8 encoding of:
  * The public key string of the agent creating the object (first 60 chars of the hash) for an *agent* object.
  * The certificate id for a *certificate* object.
  * The organization id for an *organization* object.
  * The standard id for a *standard* object.
  * The request id for a *request* object.

For example, the CR address for the creation of an agent object would look like the following:
`439a560000DFB4D55EE01720E8F098CA4F5063C67BDEA82732113C9D41B3F345B1EAFA`

## State

### Agent State

A CR Agent state entry is required to consist of the following protobuf message:

[agent.proto](https://github.com/target/consensource/blob/master/protos/agent.proto)
```protobuf
message Agent {
    // Public key associated with the agent.
    string public_key = 1;

    // A human-readable name identifying the agent.
    string name = 2;

    // A human-readable organization ID identifying the associated org.
    string organization_id = 3;

    // Approximately when the agent was registered.
    // Format: UTC timestamp
    uint64 timestamp = 4;
}

message AgentContainer {
    repeated Agent entries = 1;
}
```

In the event of a hash collision (i.e. two or more state entries
sharing the same address), the colliding state entries will be stored in the agent container defined in the proto file above.


#### Organization State
A CR Organization state entry is required to consist of the following protobuf message:

[organization.proto](https://github.com/target/consensource/blob/master/protos/organization.proto)

```
message Organization {
    enum Type {
        UNSET_TYPE = 0;
        CERTIFYING_BODY = 1;
        STANDARDS_BODY = 2;
        FACTORY = 3;
    }

    message Authorization {
        enum Role {
            UNSET_ROLE = 0;
            ADMIN = 1;
            TRANSACTOR = 2;
        }
        // Public key of the authorized agent.
        string public_key = 1;

        // Agent's role within the organization.
        Role role = 2;
    }

    message Contact {
        // Name of the person.
        string name = 1;

        // Contact's phone number.
        string phone_number = 2;

        // ISO 639‑1 language code of the contact.
        string language_code = 3;
    }

    // UUID of the organization.
    string id = 1;

    // Name of the organization.
    string name = 2;

    // List of authorized agents and their roles within the organization.
    repeated Authorization authorizations = 3;

    // List contacts within the organization.
    repeated Contact contacts = 4;

    // What type of organization this data represents.
    Type organization_type = 5;


    // Only one of these fields will be filled based on the organization type.
    CertifyingBody certifying_body_details = 6;
    StandardsBody standards_body_details = 7;
    Factory factory_details = 8;
}

message CertifyingBody {
    message Accreditation {
        // Standard for which the accreditation has been issued.
        string standard_id = 1;

        // Standard version for which the accreditation has been issued.
        string standard_version = 2;

        // Standards body that issued the accreditation.
        string accreditor_id = 3;

        // Time range that the accreditation is valid (UTC Timestamps)
        uint64 valid_from = 4;
        uint64 valid_to = 5;
    }

    // List of accreditations that the certifying body holds.
    repeated Accreditation accreditations = 1;
}

message StandardsBody {
    // This is message could be used to capture information that is only
    // pertinent to a StandardsBody
}

message Factory {
    message Address {
        // Street address, Line 1
        string street_line_1 = 1;

        // Street address, Line 2 (optional)
        string street_line_2 = 2;

        // City
        string city = 3;

        // State or province, (optional)
        string state_province = 4;

        // Country
        string country = 5;

        // Postal Code, (optional)
        string postal_code = 6;
    }

    // Location of the factory
    Address address = 1;
}

message OrganizationContainer {
    repeated Organization entries = 1;
}
```

In the event of a hash collision (i.e. two or more state entries
sharing the same address), the colliding state entries will be stored in the organization container defined in the proto file above.

#### Certificate State
A CR Certificate state entry is required to consist of the following protobuf message:

[certificate.proto](https://github.com/target/consensource/blob/master/protos/certificate.proto)

```protobuf
message Certificate {
    message CertificateData {
        // Name of data field associated with certificate data.
        string field = 1;

        // Data stored within the data field.
        string data = 2;
    }

    // This certificate's ID.
    string id = 1;

    // Certifying body that issued the certificate.
    string certifying_body_id = 2;

    // Factory the certificate was issued to.
    string factory_id = 3;

    // Standard that this certificate is for.
    string standard_id = 4;

    // Standard version that the certificate is for.
    string standard_version = 5;

    // Additional certificate data.
    repeated CertificateData certificate_data = 6;

    // Time certificate was issued.
    // Format: UTC timestamp
    uint64 valid_from = 7;

    // Approximately when the certificate will become invalid.
    // Format: UTC timestamp
    uint64 valid_to = 8;
}

message CertificateContainer {
    repeated Certificate entries = 1;
}
```

In the event of a hash collision (i.e. two or more state entries
sharing the same address), the colliding state entries will be stored in the certificate container defined in the proto file above.

#### Request State

A CR Request state entry is required to consist of the following
protobuf message:

[request.proto](https://github.com/target/consensource/blob/master/protos/request.proto)

```protobuf
message Request {
    enum Status {
        UNSET_STATUS = 0;
        OPEN = 1;
        IN_PROGRESS = 2;
        CLOSED = 3;
        CERTIFIED = 4;
    }

    // UUID of this request.
    string id = 1;

    // Status of the request.
    Status status = 2;

    // The standard the factory is requesting certification against.
    string standard_id = 3;

    // UUID of the factory that created this request.
    string factory_id = 4;

    // Time request was made
    // Format: UTC timestamp
    uint64 request_date = 5;
}

message RequestContainer {
    repeated Request entries = 1;
}
```

#### Standard State

A CR Standard state entry is required to consist of the following
protobuf message:

[request.proto](https://github.com/target/consensource/blob/master/protos/request.proto)

```protobuf
message Standard {
    message StandardVersion {
        // Standard version
        string version = 1;

        // Short description of the standard.
        string description = 2;

        // Link to the standard's documentation.
        string link = 3;

        // Date the standard is officially issued.
        uint64 approval_date = 4;
    }

    // Sha256 of the standard name
    string id = 1;

    // Id of the organization that created this Standard.
    string organization_id = 2;

    // Name of the standard.
    string name = 3;

    // List of different versions of the standard.
    repeated StandardVersion versions = 4;

}

message StandardContainer {
    repeated Standard entries = 1;
}
```

In the event of a hash collision (i.e. two or more state entries sharing the
same address), the colliding state entries will be stored in the request
container defined in the proto file above.


## Transaction Payload
CR transaction request payloads are defined by the following protobuf structure:

[payload.proto](https://github.com/target/consensource/blob/master/protos/payload.proto##L7-L38)
```protobuf
message CertificateRegistryPayload{
    enum Action {
        UNSET_ACTION = 0;
        CREATE_AGENT = 1;
        CREATE_ORGANIZATION = 2;
        UPDATE_ORGANIZATION = 3;
        AUTHORIZE_AGENT = 4;
        ISSUE_CERTIFICATE = 5;
        CREATE_STANDARD = 6;
        UPDATE_STANDARD = 7;
        OPEN_REQUEST_ACTION = 8;
        CHANGE_REQUEST_STATUS_ACTION = 9;
        ACCREDIT_CERTIFYING_BODY_ACTION = 10;
    }

    // Whether the payload contains a create agent, create organization,
    // create certificate, or authorize agent.
    Action action = 1;

    // The transaction handler will read from just one of these fields
    // according to the action.
    CreateAgentAction create_agent = 2;
    CreateOrganizationAction create_organization = 3;
    UpdateOrganizationAction update_organization = 4;
    AuthorizeAgentAction authorize_agent = 5;
    IssueCertificateAction issue_certificate = 6;
    CreateStandardAction create_standard = 7 ;
    UpdateStandardAction update_standard = 8;
    OpenRequestAction open_request_action = 9;
    ChangeRequestStatusAction change_request_status_action = 10;
    AccreditCertifyingBodyAction accredit_certifying_body_action = 11;
}
```
Based on the selected type, the data field will contain the appropriate transaction data (these messages would be defined within the CertificateRegistryPayload):


### CreateAgentAction transaction

The CreateAgentAction transaction creates an agent object that is able to sign transactions and perform actions on the behalf of their associated organization. This agent object will be initialized with no associated organization ID.

[CreateAgentAction protobuf](https://github.com/target/consensource/blob/master/protos/payload.proto##L40-L47)
```protobuf
message CreateAgentAction {
    // A name identifying the agent.
    string name = 1;

    // Approximately when the agent was registered.
    // Format: UTC timestamp
    uint64 timestamp = 2;
}
```
This transaction is considered invalid if one of the following occurs:
 - Name is not provided
 - Signing public key already associated with an agent


### CreateOrganizationAction transaction

The CreateOrganizationAction transaction creates an organization object. An organization may either be an STANDARDS_BODY, CERTIFYING_BODY or a FACTORY depending on the actions the organization will perform, such as creating standards, issuing or requesting certificates. These actions are performed by authorized agents associated with the organization. The organization object created will be initialized with the agent that signed the transaction as an ADMIN within the organization's authorizations list.

[CreateOrganizationAction protobuf](https://github.com/target/consensource/blob/master/protos/payload.proto##L49-L64)
```protobuf
message CreateOrganizationAction {
    // UUID of the organization.
    string id = 1;

    // Type of the organization.
    Organization.Type organization_type = 2;

    // Name of the organization.
    string name = 3;

    // Initial contact info for the organization.
    repeated Organization.Contact contacts = 4;

    // Address of the organization (if the organization is a Factory).
    Factory.Address address = 5;
}
```
This transaction will be considered invalid if one of the following occurs:
 - Organization ID, name, and/or organization type are not provided
 - Organization ID already exists
 - Signing public key is not associated with a valid Agent object
 - Agent submitting the transaction already has an associated organization
 - Address is provided if the type is Standards Body or Certifying Body
 - Address is not provided if the type is Factory


### UpdateOrganizationAction transaction

The UpdateOrganizationAction transaction modifies the value of an Organization in state. Both the address (for factories) and the contact information may be updated. The values provided will be applied exactly as submitted with the transaction. If one or the other should stay the same, the original values should be supplied.

[UpdateOrganizationAction protobuf](https://github.com/target/consensource/blob/master/protos/payload.proto##L66-L72)
```protobuf
message UpdateOrganizationAction {
    // Updated contact info.
    repeated Organization.Contact contacts = 1;

    // Updated address (if Factory).
    Factory.Address address = 2;
}
```
This transaction is considered invalid if one of the following occurs:
 - The signer of the transaction is not listed as an admin of their organization
 - Provided contacts or address objects are not fully filled out
 - Address is provided if the organization is not a factory


### AuthorizeAgentAction transaction

The AuthorizeAgentAction transaction creates an entry within an organization's authorizations list for the specified public key with the specified role. This action may only be performed by an agent authorized as an ADMIN by their associated organization.

[AuthorizeAgentAction protobuf](https://github.com/target/consensource/blob/master/protos/payload.proto##L74-L83)
```protobuf
message AuthorizeAgentAction {
    // Public key associated with the agent.
    string public_key = 1;

    // Role to update the specified agent entry.
    // Roles grant permissions for an agent to act on behalf of the
    // organization.
    // Whether the agent is an ADMIN or ISSUER.
    Organization.Authorization.Role role = 2;
}
```
This transaction is considered invalid if one of the following occurs:
 - Public key is not provided
 - Role is not provided
 - Signing public key is not associated an Agent
 - Public key provided is not associated an Agent
 - Agent submitting the transaction is not authorized as an ADMIN within their associated organization
 - Public key provided specifies an Agent already associated with an organization
 - Invalid authorization role is provided


### IssueCertificateAction transaction

The IssueCertificateAction transaction creates a certificate object that contains information pertaining to the specified factory and their adherence to certain policies. A Certificate object is created by an agent associated with a certifying body.

[IssueCertificateAction protobuf](https://github.com/target/consensource/blob/master/protos/payload.proto##L85-L121)
```protobuf
message IssueCertificateAction {
    enum Source {
        UNSET_SOURCE = 0;
        FROM_REQUEST = 1;
        INDEPENDENT= 2;
    }

    // UUID of the certificate.
    string id = 1;

    // ID of the factory that the certificate is being issued to.
    string factory_id = 2;

    // The source that triggered the IssueCertificate Trasaction.
    // If set to FROM_REQUEST, it means the IssueCertificateAction is associated
    // to a request made by a factory. The field request_id must be set.
    //  If set to INDEPENDENT, it means the IssueCertificateAction is not associated
    //  with a request made by a factory. The field factory_id and standard_id must be set.
    Source source = 3;

    // ID of the request (if source is FROM_REQUEST)
    string request_id = 4;

    // Standard that this certificate is for.
    string standard_id = 5;

    // Additional certificate data.
    repeated Certificate.CertificateData certificate_data = 6;

    // Time certificate was issued.
    // Format: UTC timestamp
    uint64 valid_from = 7;

    // Approximately when the certificate will become invalid.
    // Format: UTC timestamp
    uint64 valid_to = 8;
}
```
This transaction is considered invalid if one of the following occurs:
 - ID, factory ID, standard name, valid from timestamp and/or valid to timestamp are not provided
 - Certificate ID is already associated with a Certificate object
 - Factory ID does not reference a valid factory
 - Signing public key is not associated with an agent
 - Agent submitting the transaction is not associated with a certifying body
 - Certifying Body associated with the issuing agent is not accredited to issue the standard
 - Agent submitting the transaction is not an authorized TRANSACTOR within their associated organization
 - Standard name is not associated with an existing standard
 - Invalid dates are provided, pertaining to current date as well as format


### CreateStandardAction transaction

The CreateStandardAction transaction creates a new certification standard in state. It will also create a StandardVersion sub-object, which contains details specific to the version of the standard which was supplied. A CreateStandardAction transaction is submitted by an agent associated with a standards body.

[CreateStandardAction protobuf](https://github.com/target/consensource/blob/master/protos/payload.proto##L143-162)
```protobuf
message CreateStandardAction {
    // Sha256 of the standard name
    string standard_id = 1;

    // Name of the standard.
    string name = 2;

    // Current version of the standard.
    string version = 3;

    // Short description of the standard.
    string description = 4;

    // Link to the standard's documentation.
    string link = 5;

    // Date the standard is officially issued.
    uint64 approval_date = 6;

}
```
This transaction is considered invalid if one of the following occurs:
 - The standard_id, name, version, description, link, or approval date are not provided
 - The standard_id is already associated with an existing standard
 - The signer is not associated with a standards body
 - The signer is not authorized as a transactor within their organization


### UpdateStandardAction transaction

The UpdateStandardAction transaction creates a new standard version in state, under the associated standard object. An UpdateStandardAction transaction is submitted by an agent associated with a standards body.

[UpdateStandardAction protobuf](https://github.com/target/consensource/blob/master/protos/payload.proto##L164-L179)
```protobuf
message UpdateStandardAction {
    // Standard that is being updated.
    string standard_id = 1;

    // New version of the standard.
    string version = 2;

    // Short description of the standard.
    string description = 3;

    // Link to the standard's documentation.
    string link = 4;

    // Date the standard is officially issued.
    uint64 approval_date = 5;
}
```
This transaction is considered invalid if one of the following occurs:
 - The standard_id, version, description, link, or approval date are not provided
 - The standard_id is not associated with an existing standard
 - The version is already associated with an existing standard version
 - The signer is not associated with a standards body
 - The signer is not authorized as a transactor within their organization
 - The standard is not associated with the signer's organization



### AccreditCertifyingBodyAction transaction

The AccreditCertifyingBodyAction transaction adds an accreditation to a certifying body. An UpdateStandardAction transaction is submitted by an agent associated with a standards body.

[AccreditCertifyingBodyAction protobuf](https://github.com/target/consensource/blob/master/protos/payload.proto##L181-L195)
```protobuf
message AccreditCertifyingBodyAction {
    // UUID of the certifying body that is being accredited.
    string certifying_body_id = 1;

    // Standard that the certifying body is being accredited for.
    string standard_id = 2;

    // Time the accreditation was issued.
    // Format: UTC timestamp
    uint64 valid_from = 3;

    // When the accreditation will become invalid.
    // Format: UTC timestamp
    uint64 valid_to = 4;
}
```
This transaction is considered invalid if one of the following occurs:
 - The signer is not associated with a standards body
 - The signer is not authorized as a transactor within their organization
 - The certifying body ID is not associated with a certifying body
 - The name is not associated with an existing standard
 - Invalid dates are provided, pertaining to current date as well as format


### OpenRequestAction transaction

The OpenRequestAction transaction opens a request for certification for a factory. This transaction is submitted by an agent associated with a factory and authorized as a TRANSACTOR for their associated factory.

[OpenRequestAction protobuf](https://github.com/target/consensource/blob/master/protos/payload.proto##L123-133)
```protobuf
message OpenRequestAction {
    // UUID of the request
    string request_id = 1;

    // Certification standard of which the factory is requesting from an certification body
    Request.Certification_Standard certification_standard = 2;

    // Name of the factory that is opening the request
    string factory_name = 3;

    // Time request was made
    // Format: UTC timestamp
    uint64 request_date = 4;
}
```
This transaction is considered invalid if one of the following occurs:
- The signer is not associated with a factory
- The signer is not authorized as a transactor within their organization
- The id is not unique
- The standards name is not associated with a valid standard
- If any of these fields are empty


### ChangeRequestStatusAction transaction
The ChangeRequestStatusAction transaction is performed when a factory's certification request status is changed, such as to CLOSED, IN_PROGRESS, or CERTIFIED. This transaction results from either a factory changing the status to CLOSED or IN_PROGRESS, which would be submitted by an agent associated with the factory and authorized as a TRANSACTOR.

[ChangeRequestStatusAction protobuf](https://github.com/target/consensource/blob/master/protos/payload.proto##L135-141)
```protobuf
message ChangeRequestStatusAction {
    // Request for which the status is being changed.
    string request_id = 1;

    // New status of the request.
    Request.Status status = 2;
}
```
This transaction is considered invalid if one of the following occurs:
- The signer is not associated with a factory
- The signer is not authorized as a transactor within their organization
- A request with the provided id does not exist
- The status is not a valid status enum: IN_PROGRESS or CLOSED
- The status is already CLOSED or CERTIFIED


## Transaction Header

### Family

* family_name: "certificate_registry"
* family_version: "0.1"

### Inputs and Outputs

The inputs and outputs for Certification Registry transactions will differ depending on the transaction type.
Inputs are the address(es) of all the state objects required to validate a transaction.
Outputs are the address(es) of all the state objects modified by the transaction.

### CreateAgentAction Transaction

Inputs:

 - Address of the Agent to be created

Outputs:

 - Address of the Agent created

#### CreateOrganizationAction Transaction

Inputs:

 - Address of the Agent submitting the transaction

 - Address of the Organization to be created

Outputs:

 - Address of the Agent that submitted the transaction

 - Address of the Organization created

### AuthorizeAgent Transaction

Inputs:

 - Address of the Agent submitting the transaction

 - Address of the Organization with the authorization list being modified

 - Address of the Agent to be added to the Organization's authorization list

Outputs:

 - Address of the Organization with a modified authorization list

 - Address of the Agent added to the authorization list

### IssueCertificateAction Transaction

Inputs:

 - Address of the Agent submitting the transaction

 - Address of the Organization the certificate is being submitted on behalf of

 - Address of the Certificate to be created

 - Address of the Standard the certificate is being created against

Outputs:

 - Address of the Certificate created

### CreateStandardAction Transaction

Inputs:

 - Address of the Agent submitting the transaction

 - Address of the Standard that is being created

 - Address of the Organization the Standard is being created on behalf of

Outputs:

 - Address of the Standard that is being created

 - Address of the Organization the Standard is being created on behalf of

### UpdateStandardAction Transaction

Inputs:

 - Address of the Agent submitting the transaction

 - Address of the Standard that is being created

 - Address of the Organization the Standard is being created on behalf of

Outputs:

 - Address of the Standard that is being created


### AccreditCertifyingBodyAction Transaction

Inputs:

 - Address of the Agent submitting the transaction

 - Address of the Standard that is being accredited for

 - Address of the Organization that the transaction is being submitted on behalf of

 - Address of the Organization that is being accredited

Outputs:

- Address of the Organization that is being accredited

### OpenRequestAction Transaction

Inputs:

 - Address of the Request to be created

 - Address of the Organization the Request is being made for

 - Address of the Standard the Request is being made for

Outputs:

 - Address of the Request to be created

### ChangeRequestStatusAction Transaction

Inputs:

 - Address of the Request to be updated

 - Address of the Organization the Request is being made for

Outputs:

 - Address of the Request to be updated

## Execution

The result of a Certificate Registry transaction will differ depending on the type of transaction being executed.

A successful CreateAgentAction transaction will result in a new Agent object created in state.

A successful CreateOrganizationAction transaction will result in a new Organization object created in state. This Organization object will be initialized with a single authorization entry corresponding to the Agent that submitted the successful CreateOrganizationAction transaction with a role of ADMIN.

A successful UpdateOrganizationAction transaction will result in an updated Organization object in state.

A successful AuthorizeAgentAction transaction will result in a new entry to the specified Organization's authorizations list. The Agent being added to the Authorizations list will also be updated with an associated Organization ID.

A successful IssueCertificateAction transaction will result in a new Certificate object created in state.

A successful CreateStandardAction transaction will result in a new Standard object in state, with one StandardVersion sub-object. The Standard will be added to the Standards Body's standards list.

A successful CreateStandardAction transaction will result in a new Standard object in state, with a new StandardVersion sub-object.

A successful AccreditCertifyingBodyAction will result in a new Accreditation being added to an Organization object.

A successful OpenRequestAction transaction will result in a new Request object in state.

A successful ChangeRequestStatusAction transaction will result in an updated Request object in state with the status of provided.
This Request will be submitted on behalf of a Factory.


.. Licensed under Creative Commons Attribution 4.0 International License
.. https://creativecommons.org/licenses/by/4.0/
