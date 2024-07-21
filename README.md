# GeoBlocks: Trustless Geospatial Data Sharing with Accountability and Decentralized Access Control

This repository contains the implementation of `GeoBlocks` protocol introduced in the paper [GeoBlocks]() [BRAINS'24]
---

Note that the Paper has 2 major components implemented in different sub repositories of this repo. This includes 

-  The application gateway implementation
-  The smartcontracts implemenation on hyperledger fabric framework

## GeoServer setup

This guide assumes each participant already has a geoserver running locally to which the application gateway connects. If you don't have one, you can use a test docker setup as below

```
docker run -it -p 80:8080 docker.osgeo.org/geoserver:2.23.0
```

## Build Instructions: Application Gateway

Note: Tested on node v16.20.2 and npm 8.19.4

```
npm i
node org_contract.js
```


## Environment setup: Hyperledger Fabric

Note the codebase was tested on fabric release-2.2

The official installation docs can be found [here](https://hyperledger-fabric.readthedocs.io/en/release-2.2/install.html) 



## Build & Deployment Insructions: Smart Contracts

1. Package the chaincode
```
peer lifecycle chaincode package acl.tar.gz --path ../chaincode/acl/ --lang golang --label acl_1.0

peer lifecycle chaincode package drs.tar.gz --path ../chaincode/drs/ --lang golang --label drs_1.0
```
2. Create channel
```
bash network.sh up createChannel -c mychannel -ca
```

3. Deploy packaged chaincode

```
bash network.sh deployCC -ccn acl -ccp ../chaincode/acl/ -ccl go

bash network.sh deployCC -ccn drs -ccp ../chaincode/drs/ -ccl go
```

Note: Behind the scenes, this script uses the chaincode lifecycle to package, install, query installed chaincode, approve chaincode for both Org1 and Org2, and finally commit the chaincode. You can refer each of the step involved and how it works [here](https://hyperledger-fabric.readthedocs.io/en/release-2.2/deploy_chaincode.html)


## Testing

Run the application gateway after you Geoserver and Fabric setup are done.

This starts an express server, post which you can start using the APIs of the gateway to test the functionality of the application.