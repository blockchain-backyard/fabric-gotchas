# fabric-gotchas
Catching errors that Fabric failed to catch.

### v1.1.0: last commit 523f644
#### Compilation Issues
1. We need gotools to be installed prior to running `make all`. The easiest fix is to run 
 ```
 make dist-clean gotools all
 ```
