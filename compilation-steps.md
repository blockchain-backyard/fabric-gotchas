1. If any of the gotools not found: 
	i. export PATH=$PATH:$GOPATH/src/github.com/hyperledger/fabric/build/docker/gotools/bin
		(adds the built gotools to your local path for them to be discovered
	ii. make gotools (installs gotools on your local GOPATH)
2. make native 
	-- creates the ccenv and javaenv docker images
	-- creates in build/bin images peer, orderer, configtxgen, cryptogen, configtxlator
	-- creates gotools/* in build/docker/gotools/bin
3. make docker 
	-- pulls third-party images fabric-kafka, fabric-couchdb, etc. from dockerhub 
	-- builds docker images: fabric-peer, fabric-orderer, fabric-buildenv, fabric-testenv, fabric-tools

4. exclude `release_notes`  from license checks
	in file release_notes/v1.1.0.txt
	```
	-  | grep -v .key$ | grep -v \\.gen.go$ | grep -v ^Gopkg.lock$ \
	+  | grep -v .key$ | grep -v \\.gen.go$ | grep -v ^Gopkg.lock$ | grep -v release_notes \
	```

5. make unit-test
	-- runs the unit-test within the docker-compose environment as specified in the script within unit-test/


#### Building build/bin/peer
To build bin/image/peer we need build/image/ccenv/ and build/image/javaenv. 
To build ccenv/javaenv we need 
	i.   proto generator for the respective languages:
		a. protoc-gen-go (for go) (available with gotools)
		b. protos.tar.bz2 (for java)
	ii.  chaincode shims for the respective languages
		a.   goshim.tar.bz2
		b. javashim.tar.bz2
	iii. the build environment for respective languages
		a. build/bin/chaintool 
		b. settings.gradle

#### sampleconfig
smapleconfig for each of the peer/orderer/testenv/tools are supplied as build/sampleconfig.tar.bz2 archive 
