## Tokens

**Tokens** is a Java library for verifying and storing OAuth 2.0 service access tokens. It is resilient, configurable, and production-tested, and works with all JVM languages.

[![Build Status](https://travis-ci.org/zalando/tokens.svg?branch=master)](https://travis-ci.org/zalando-stups/tokens)
[![Javadoc](https://javadoc-emblem.rhcloud.com/doc/org.zalando.stups/tokens/badge.svg)](http://www.javadoc.io/doc/org.zalando.stups/tokens)
![Maven Central](https://img.shields.io/maven-central/v/org.zalando.stups/tokens.svg)
[![Coverage Status](https://coveralls.io/repos/zalando-stups/tokens/badge.svg?branch=master)](https://coveralls.io/r/zalando-stups/tokens?branch=master)
[![codecov.io](https://codecov.io/github/zalando-stups/tokens/coverage.svg?branch=master)](https://codecov.io/github/zalando-stups/tokens?branch=master)

### Project Features and Functionality

Some of the features Tokens offers:

- support for credential rotation, by reading them on-demand from the file system
- extensiblity with a credentials provider
- configuration flexibility; specify multiple tokens with different scopes
- the ability to inject fixed OAuth2 access tokens

Tokens can be useful to devs (at any company, large or small) who are working with highly-distributed microservices deployed in the cloud and need to authenticate the traffic generated when accessing APIs. For example, if your team wants to consume an API with OAuth2 credentials, Tokens will fetch the tokens for you. Then you just [add scopes](#Usage) in the token. 

When creating tokens, it's easy to make a lot of mistakes. Tokens aims to save you hassle and time.

### Prerequisites

- Maven
- [Apache-HttpClient](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient)

#### Maven Dependency

Add it with:

```xml
<dependency>
    <groupId>org.zalando.stups</groupId>
    <artifactId>tokens</artifactId>
    <version>see above</version>
</dependency>
```

####Apache-HttpClient

Add it with:

```xml
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpclient</artifactId>
    <version>your version</version>
</dependency>
```

### Usage

```java
AccessTokens tokens = Tokens.createAccessTokensWithUri(new URI("https://example.com/access_tokens"))
                            .manageToken("exampleRW")
                                .addScope("read")
                                .addScope("write")
                                .done()
                            .manageToken("exampleRO")
                                .addScope("read")
                                .done()
                            .start();

while (true) {
    final String token = tokens.get("exampleRO");

    Request.Get("https://api.example.com")
           .addHeader("Authorization", "Bearer " + token)
           .execute():

    Thread.sleep(1000);
}
```

### Local Testing

With Tokens, you can inject fixed OAuth2 access tokens via the `OAUTH2_ACCESS_TOKENS` environment variable and test applications locally with personal OAuth2 tokens. As an example:

```bash
$ MY_TOKEN_1=$(zign token -n mytok1)
$ MY_TOKEN_2=$(zign token -n mytok2)
$ export OAUTH2_ACCESS_TOKENS=mytok1=$MY_TOKEN_1,mytok2=$MY_TOKEN_2
$ lein repl # start my local Clojure app using the tokens library
```

In production on EC2 instances, Tokens fetches access tokens by requesting an authorization server with credentials, found in `client.json` and `user.json`. It's also possible to provide `client.json` and `user.json` with valid content and point this library to that directory.

### Contributing

This project welcomes contributions, including bug fixes and documentation enhancements. To contribute, please use the Issues Tracker to let us know what you would like to do. We'll respond, and go from there.

### License

Copyright © 2015 Zalando SE

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   [http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0)

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
