![DIF Logo](https://raw.githubusercontent.com/decentralized-identity/decentralized-identity.github.io/master/images/logo-small.png)

# Universal Resolver - Java Implementation

This is a Java implementation of a Universal Resolver. See [universal-resolver](https://github.com/decentralized-identity/universal-resolver/) for a general introduction to Universal Resolvers and drivers.

See this [blog post](https://medium.com/decentralized-identity/a-universal-resolver-for-self-sovereign-identifiers-48e6b4a5cc3c) for an introduction.

See https://uniresolver.io/ for a publicly hosted instance of a Universal Resolver.

## Quick Start

You can deploy the Java Universal Resolver on your local machine by cloning this Github repository, and using `docker-compose` to build and run the Java Universal Resolver as well as its drivers:

	git clone https://github.com/decentralized-identity/universal-resolver-java
	cd universal-resolver-java/
	docker-compose -f docker-compose.yml build
	docker-compose -f docker-compose.yml up

You should then be able to resolve identifiers locally using simple `curl` requests as follows:

	curl -X GET  http://localhost:8080/1.0/identifiers/did:sov:WRfXPg8dantKVubE3HX8pw
	curl -X GET  http://localhost:8080/1.0/identifiers/did:btcr:xkrn-xzcr-qqlv-j6sl
	curl -X GET  http://localhost:8080/1.0/identifiers/did:v1:testnet:5431fafa-a38f-4e37-96b6-cdeb8e5d1d40
	curl -X GET  http://localhost:8080/1.0/identifiers/did:ipid:QmbFuwbp7yFDTMX6t8HGcEiy3iHhfvng89A19naCYGKEBj
	curl -X GET  http://localhost:8080/1.0/identifiers/did:uport:2ok9oMAM54TeFMfLb3ZX4i9Qu6x5pcPA7nV
	curl -X GET  http://localhost:8080/1.0/identifiers/did:stack:v0:16EMaNw3pkn3v6f2BgnSSs53zAKH4Q8YJg-0

## Build (native Java)

Run:

	mvn clean install

## Local Resolver

You can use a [Local Resolver](https://github.com/decentralized-identity/universal-resolver-java/tree/master/uni-resolver-client) in your Java project that invokes drivers locally (either directly via their JAVA API or via a Docker REST API).

Dependency:

	<dependency>
		<groupId>decentralized-identity</groupId>
		<artifactId>uni-resolver-local</artifactId>
		<version>0.1-SNAPSHOT</version>
	</dependency>

[Example Use](https://github.com/decentralized-identity/universal-resolver-java/blob/master/examples/src/main/java/uniresolver/examples/TestLocalUniResolver.java):

	LocalUniResolver uniResolver = LocalUniResolver.getDefault();
	uniResolver.getDriver(DidSovDriver.class).setLibIndyPath("./sovrin/lib/");
	uniResolver.getDriver(DidSovDriver.class).setPoolConfigName("live");
	uniResolver.getDriver(DidSovDriver.class).setPoolGenesisTxn("live.txn");
	uniResolver.getDriver(DidBtcrDriver.class).setBitcoinConnection(BlockcypherAPIBitcoinConnection.get());
	
	DIDDocument didDocument1 = uniResolver.resolve("did:sov:WRfXPg8dantKVubE3HX8pw").getDidDocument();
	System.out.println(didDocument1.toJson());
	
	DIDDocument didDocument2 = uniResolver.resolve("did:btcr:xkrn-xzcr-qqlv-j6sl").getDidDocument();
	System.out.println(didDocument2.toJson());
	
	DIDDocument didDocument3 = uniResolver.resolve("did:stack:v0:16EMaNw3pkn3v6f2BgnSSs53zAKH4Q8YJg-0").getDidDocument();
	System.out.println(didDocument3.toJson());

## Web Resolver

You can deploy a [Web Resolver](https://github.com/decentralized-identity/universal-resolver-java/tree/master/uni-resolver-web) that can be called by clients and invokes drivers locally (either directly via their JAVA API or via a Docker REST API).

See the [Example Configuration](https://github.com/decentralized-identity/universal-resolver-java/blob/master/uni-resolver-web/src/main/webapp/WEB-INF/applicationContext.xml).

How to run:

	mvn jetty:run

## Client Resolver

You can use a [Client Resolver](https://github.com/decentralized-identity/universal-resolver-java/tree/master/uni-resolver-client) in your Java project that calls a remote Web Resolver.

Dependency:

	<dependency>
		<groupId>decentralized-identity</groupId>
		<artifactId>uni-resolver-client</artifactId>
		<version>0.1-SNAPSHOT</version>
	</dependency>

[Example Use](https://github.com/decentralized-identity/universal-resolver-java/blob/master/examples/src/main/java/uniresolver/examples/TestClientUniResolver.java):

	ClientUniResolver uniResolver = new ClientUniResolver();
	uniResolver.setResolveUri("https://uniresolver.danubetech.com/1.0/identifiers/");
	
	DIDDocument didDocument1 = uniResolver.resolve("did:sov:WRfXPg8dantKVubE3HX8pw").getDidDocument();
	System.out.println(didDocument1.toJson());
	
	DIDDocument didDocument2 = uniResolver.resolve("did:btcr:xz35-jzv2-qqs2-9wjt").getDidDocument();
	System.out.println(didDocument2.toJson());
	
	DIDDocument didDocument3 = uniResolver.resolve("did:stack:v0:16EMaNw3pkn3v6f2BgnSSs53zAKH4Q8YJg-0").getDidDocument();
	System.out.println(didDocument3.toJson());

## Troubleshooting

If docker-compose complains about wrong versions than you probably have a too old docker-compose version.

On Ubuntu 16.04 remove docker-compose and install a new version e.g.
```
sudo apt-get remove docker-compose
curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```
You might want to adjust the version number 1.22.0 to the latest one. Please see: [Installing docker-compose](https://docs.docker.com/compose/install/#install-compose)

## About

Decentralized Identity Foundation - http://identity.foundation/
