[![npm](https://img.shields.io/npm/v/id4me-rp?style=flat-square)](https://www.npmjs.com/package/id4me-rp)

# node-id4me-rp

An Node.js ID4me Relying Party library implemented according to the [official guide](https://gitlab.com/ID4me/documentation/blob/master/id4ME%20Relying%20Party%20Implementation%20Guide.adoc)\
[Demo Application](https://id4me-demo.felisk.io)

## Installation

`npm install id4me-rp`\
or\
`yarn add id4me-rp`

## (Temporary) Documentation

### Methods available

#### Discovery

* validateDnsRecord(record: string): boolean
* parseDnsRecord(record: string): [ParsedDnsRecord](https://github.com/feliskio/node-id4me-rp/blob/355c4caacf6f96372e674d7c7d0456b6ac577015/src/types.ts#L1)
* findDnsRecord(domain: string): [ParsedDnsRecord](https://github.com/feliskio/node-id4me-rp/blob/355c4caacf6f96372e674d7c7d0456b6ac577015/src/types.ts#L1)

#### Registration

* getConfigurationUrl(iss: string): string
* `async` getConfiguration(iss: string, forceRefetch = false): [AuthorityConfiguration](https://github.com/feliskio/node-id4me-rp/blob/355c4caacf6f96372e674d7c7d0456b6ac577015/src/types.ts#L16)
* `async` registerApplication(iss: string, config: [ApplicationRegistrationData](https://github.com/feliskio/node-id4me-rp/blob/355c4caacf6f96372e674d7c7d0456b6ac577015/src/types.ts#L55), forceReset = false): [ApplicationResponse](https://github.com/feliskio/node-id4me-rp/blob/355c4caacf6f96372e674d7c7d0456b6ac577015/src/types.ts#L75)

#### Authentication

* `async` getAuthenticationUrl(config: [AuthenticationUrlConfig](https://github.com/feliskio/node-id4me-rp/blob/355c4caacf6f96372e674d7c7d0456b6ac577015/src/types.ts#L111)): string
* `async` getTokens(iss: string, clientId: string, clientSecret: string, code: string, redirectUri: string): [TokenResponse](https://github.com/feliskio/node-id4me-rp/blob/355c4caacf6f96372e674d7c7d0456b6ac577015/src/types.ts#L7)
* decodeIdToken(token: string): [DecodedIdToken](https://github.com/feliskio/node-id4me-rp/blob/355c4caacf6f96372e674d7c7d0456b6ac577015/src/types.ts#L121)

#### Claims

* `async` getClaims(iss: string, token: string): [ClaimsOverview](https://github.com/feliskio/node-id4me-rp/blob/355c4caacf6f96372e674d7c7d0456b6ac577015/src/types.ts#L132)
* `async` getClaim(claims: [ClaimsOverview](https://github.com/feliskio/node-id4me-rp/blob/355c4caacf6f96372e674d7c7d0456b6ac577015/src/types.ts#L132), name: string): string | number | null

Claims Client: Used to cut down on duplicate code when requesting multiple claims
```javascript
  const claimsClient = new id4me.ClaimsClient(req.session!.iss, tokens.access_token);
  await claimsClient.loadClaims();
  req.session!.userData = {
    email: await claimsClient.getClaim('email'),
    givenName: await claimsClient.getClaim('given_name'),
    name: await claimsClient.getClaim('name')
  };
```

All methods can be required/imported from the package directly.\
For now I recommend you also take a look at the [example code](https://github.com/feliskio/node-id4me-rp/tree/master/demo) to see how the methods are used.\
While the library and the example app are written in TypeScript you can also use them with regular JavaScript without any problems.\

## TODO

* Simplify general usage
* Support for emails as user identifier
* Support for encryption (Looking for help)
* Fix HTTPS requests failing due to bad certificates
* Ability to save credentials for registered applications
* Create more automated tests
