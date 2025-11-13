---
title: "KYAPay Profile"
#abbrev: ""
category: info

docname: draft-dz-kyapayprofile-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
#number:
date:
consensus: true
v: 3
# area: AREA
# workgroup: TODO
keyword:
 - agent
venue:
  github: "interop-alliance/kyapay-ietf-draft"
#  group: WG
#  type: Working Group
#  mail: WG@example.com
#  arch: https://example.com/WG
  latest: "https://interop-alliance.github.io/kyapay-ietf-draft/draft-dz-kyapayprofile.html"

author:
 -
    fullname: Dmitri Zagidulin
#    organization: Your Organization Here
#    email: your.email@example.com

normative:
  RFC7518:
  RFC7519:

informative:

...

--- abstract

This document defines a profile for agent identity and payment tokens in
JSON web token (JWT) format. Authorization servers and resource servers from
different vendors can leverage this profile to consume identity and payment
tokens in an interoperable manner.

--- middle

# Introduction

As agents evolve from pre-orchestrated workflow automations to truly autonomous
or semi-autonomous assistants, they need the ability to identify themselves and
more importantly identify their human principals.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

The terms `iss`, `iat`, `exp`, `jti`, `aud`, `typ` are defined in {{RFC7519}}.

The `alg` value `ES256` is a digital signature algorithm described in
{{Section 3.4 of RFC7518}}.

## Roles

Agent:
: todo ...

Agent Platform:
: todo ...

Principal:
: A legal entity (human or organization) on whose behalf / in whose authority
an agent or service is operating.

Buyer Principal:
: A legal entity (human or organization) behind the purchase / consumption of a
  product or service. Typically interacts with the seller via an agent, agentic
  interface, or a programmatic interface (API). The Buyer Principal gives the
  agent or interface permission to act on their behalf.

Seller Service:
: todo ...

Settlement Network:
: todo ...

# KYAPay Token Schemas

## Common Token Claims

The following are claims in common, used within the KYA (Know Your Agent),
PAY (Payment), and KYA+PAY (combined Know Your Agent and Payment) Tokens:

`iss`:
: REQUIRED - The issuer as defined in ...

`sub`:
: ? - Subject Identifier as defined in ...

`aud`:
: REQUIRED - ...

`iat`:
: REQUIRED - as defined in {{Section 4.1.6 of RFC7519}}.

`jti`:
: REQUIRED - Unique ID of this JWT as defined in {{Section 4.1.7 of RFC7519}}.

`exp`:
: REQUIRED - as defined in {{Section 4.1.4 of RFC7519}}.

`env`:
: OPTIONAL - Issuer environment (such as "sandbox" or "production").

`ver`:
: ? - Version of the token schema.

`ssi`:
: ? - Seller Service ID that this token was created for.

`btg`:
: ? - Buyer tag, an opaque reference ID internal to the buyer.

## KYA Token

The following identity related claims are used within KYA and KYA+PAY tokens:

`bid`:
: ? - Buyer identity claims

`apd`:
: OPTIONAL - Agent Platform identity claims.

`aid`:
: OPTIONAL - Agent identity claims.

The following informative example displays a decoded KYA type token.

~~~
{
  "kid": "<JWK Key ID>",
  "alg": "ES256",
  "typ": "kya+JWT"
}.{
  "iss": "<URL of the token issuer>",
  "iat": 1742245254,
  "exp": 1773867654,
  "jti": "b9821893-7699-4d24-af06-803a6a16476b",
  "sub": "<Buyer Agent Account ID>",
  "aud": "<Seller Agent Account ID>",

  "env": "<Issuer environment (sandbox, production, etc)>",
  "ver": "1",
  "ssi": "<Seller Service ID>",
  "btg": "<Buyer Tag (Buyer's internal reference ID)>",

  "bid": {
    "email": "buyer@buyer.com”,
    ...
  },
  "apd": {
  },
  "aid": {
  }
}
~~~
{: #example-decoded-kya-token align="left" title="A KYA type token"}

## PAY Token

The following payment related claims are used within PAY and KYA+PAY type tokens:

`spr`:
: OPTIONAL - Seller service price.

`sps`:
: OPTIONAL - Seller service pricing model (for example, `PAY_PER_USE`).

`amount`:
: ? - Token amount in currency units.

`cur`:
: ? - Currency unit, as defined in ...

`value`:
: ? - Token amount in settlement network

`mnr`:
: OPTIONAL - Maximum number of requests when `sps` is `PAY_PER_USE`

`stp`:
: OPTIONAL - Settlement type (`COIN` | `CARD` | `BANK`).

`sti`:
: OPTIONAL - Meta information for payment settlement, depending on settlement
  type.

The following informative example displays a decoded PAY type token.

~~~
{
  "kid": "<JWK Key ID>",
  "alg": "ES256",
  "typ": "pay+JWT"
}.{
  "iss": "<URL of the token issuer>",
  "iat": 1742245254,
  "exp": 1773867654,
  "jti": "b9821893-7699-4d24-af06-803a6a16476b",
  "sub": "<Buyer Agent Account ID>",
  "aud": "<Seller Agent Account ID>",

  "env": "<Issuer environment (sandbox, production, etc)>",
  "ver": "1",
  "ssi": "<Seller Service ID>",
  "btg": "<Buyer Tag (Buyer's internal reference ID)>",

  "spr": "0.010000",
  "sps": "PAY_PER_USE",
  "amount": "15.000000",
  "cur": "USD",
  "value": "15000000",
  "mnr": "1500",
  "stp": "<COIN | CARD | BANK>",
  "sti": {
    "type": "<'type' is dependant on 'sti' for COIN - USDC | x402; for CARD - VISA_VIC;>",
    "dynamicDataExpiration": "<Timestamp when DTVV expires>",
    "dynamicDataId": "<Visa specific identifier>",
    "dynamicDataType": "DAVV",
    "dynamicDataValue": "<DAVV value>",
    "paymentToken": "<16 Digit Virtual Payment Card Number>",
    "tokenExpirationMonth": "<Expiration Month Number>",
    "tokenExpirationYear": "<Expiration Year>"
  }
}

~~~
{: #example-decoded-pay-token align="left" title="A PAY type token"}

## KYA+PAY Token

The following informative example displays a decoded KYA+PAY type token.

~~~
{
  "kid": "<JWK Key ID>",
  "alg": "ES256",
  "typ": "kya+pay+JWT"
}.{
  "iss": "<URL of the token issuer>",
  "iat": 1742245254,
  "exp": 1773867654,
  "jti": "b9821893-7699-4d24-af06-803a6a16476b",
  "sub": "<Buyer Agent Account ID>",
  "aud": "<Seller Agent Account ID>",

  "env": "<Issuer environment (sandbox, production, etc)>",
  "ver": "1",
  "ssi": "<Seller Service ID>",
  "btg": "<Buyer Tag (Buyer's internal reference ID)>",

  "bid": {
    "email": "buyer@buyer.com”,
    ...
  },
  "apd": {
  },
  "aid": {
  },

  "spr": "0.010000",
  "sps": "PAY_PER_USE",
  "amount": "15.000000",
  "cur": "USD",
  "value": "15000000",
  "mnr": "1500",
  "stp": "<COIN | CARD | BANK>",
  "sti": {
    "type": "<'type' is dependant on 'sti'>",
    "dynamicDataExpiration": "<Timestamp when DTVV expires>",
    "dynamicDataId": "<Visa specific identifier>",
    "dynamicDataType": "DAVV",
    "dynamicDataValue": "<DAVV value>",
    "paymentToken": "<16 Digit Virtual Payment Card Number>",
    "tokenExpirationMonth": "<Expiration Month Number>",
    "tokenExpirationYear": "<Expiration Year>"
  }
}

~~~
{: #example-decoded-kya-pay-token align="left" title="A KYA+PAY type token"}

# Security Considerations

TODO Security

# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
