We are assuming that we are in the top fabric directory, i.e., `$GOPATH/src/github.com/hyperledger/fabric`.

#### Steps for Compilation
1. If any of the gotools not found: 
	* `export PATH=$PATH:$GOPATH/src/github.com/hyperledger/fabric/build/docker/gotools/bin`
		(adds the built gotools to your local path for them to be discovered)
	* `make gotools` (installs gotools on your local GOPATH)
2. `make native` 
	* creates the ccenv and javaenv docker images
	* creates in `build/bin` images peer, orderer, configtxgen, cryptogen, configtxlator
	* creates `gotools/*` in `build/docker/gotools/bin`
3. `make docker` 
	* pulls third-party images `hyperledger/fabric-kafka`, `hyperledger/fabric-couchdb`, etc. from dockerhub 
	* builds docker images: `hyperledger/fabric-peer`, `hyperledger/fabric-orderer`, `hyperledger/fabric-buildenv`, `hyperledger/fabric-testenv`, `hyperledger/fabric-tools`.

4. exclude `release_notes`  from license checks
	in file `release_notes/v1.1.0.txt`
	```
	-  | grep -v .key$ | grep -v \\.gen.go$ | grep -v ^Gopkg.lock$ \
	+  | grep -v .key$ | grep -v \\.gen.go$ | grep -v ^Gopkg.lock$ | grep -v release_notes \
	```

5. make unit-test
	* runs the unit-test within the docker-compose environment as specified in the script within `unit-test/`


#### Building build/bin/peer
To build `bin/image/peer` we need `build/image/ccenv/` and `build/image/javaenv`. 
To build `ccenv`/`javaenv` we need 
	
	1. proto generator for the respective languages:
		1. protoc-gen-go (for go) (available with gotools)
		2. protos.tar.bz2 (for java)
	2. chaincode shims for the respective languages
		1.   goshim.tar.bz2
		2. javashim.tar.bz2
	3. the build environment for respective languages
		1. build/bin/chaintool 
		2. settings.gradle

#### sampleconfig
smapleconfig for each of the peer/orderer/testenv/tools are supplied as `build/sampleconfig.tar.bz2` archive. 
