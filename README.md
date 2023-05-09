# Ping and Status with TLS

### Most used commands
All at once
* mvn -P test install

Process resources and run integration tests
* mvn -P test process-resources apigee-config:exportAppKeys frontend:npm@integration -Dskip.clean=true -Dapigee.config.dir=target/resources/edge -Dapigee.config.exportDir=target/test/integration -Dapi.testtag=@get-ping

Just run integration tests from target
* mvn -P test frontend:npm@integration -Dskip.clean=true -Dapigee.config.dir=target/resources/edge 
* npm run integration

### Commands
* mvn jshint:lint
* mvn -P test frontend:npm@unit
* mvn -P test install
* mvn -P test install -Dapi.testtag=@get-ping -DskipTests=true
* mvn -P test process-resources frontend:npm@integration -Dapi.testtag=@get-ping
* mvn -P test install -Dapigee.config.options=sync -Dapi.testtag=@get-ping
* mvn -P test clean process-resources jmeter:jmeter jmeter-analysis:analyze
* mvn -P test clean process-resources frontend:npm@integration -Dapi.testtag=@get-status
* mvn -P test apigee-config:developers apigee-config:apiproducts apigee-config:developerapps -Dapigee.config.options=update
* mvn -P test apigee-config:exportAppKeys -Dapigee.config.exportDir=./appkeys
* mvn -P test install -Dapi.testtag=@get-ping -DskipTests=true
* mvn -P test clean process-resources frontend:npm@integration -Dapi.testtag=@get-ping

# Just update the keystores, aliases and references - discretely
export PROFILE=test
export PROFILE=prod
mvn -P $PROFILE resources:copy-resources@copy-resources replacer:replace@replace -DConfigCertSuffix=2023-03-15
mvn -P $PROFILE apigee-config:keystores -Dapigee.config.options=update
mvn -P $PROFILE apigee-config:aliases -Dapigee.config.options=update
mvn -P $PROFILE apigee-config:references -Dapigee.config.options=update

# Test
mvn -P $PROFILE apigee-config:exportAppKeys -Dskip.clean=true -Dapigee.config.dir=target/resources/edge -Dapigee.config.exportDir=target/test/integration
mvn -P $PROFILE frontend:npm@integration

# Update certs and target servers
mvn -P test process-resources -Dapigee.config.options=update apigee-config:keystores apigee-config:aliases apigee-config:references -DConfigCertSuffix=2023-03-15

* mvn -P test apigee-config:targetservers -Dapigee.config.options=update

* mvn -P test apigee-config:developerapps -Dapigee.config.options=update
* mvn -P test apigee-config:apiproducts -Dapigee.config.options=update
* mvn -P test apigee-config:kvms -Dapigee.config.options=update

Install proxy no integration or jmeter tests
* mvn -P test install -Dapi.testtag=@NONE -DskipTests=true

Install proxy and update all configs, no integration or jmeter tests
* mvn -P test install -Dapigee.config.options=update -Dapi.testtag=@NONE -DskipTests=true

Update Devs, Products, Apps
mvn -P test clean process-resources apigee-config:developers apigee-config:apiproducts apigee-config:developerapps -Dapigee.config.options=update

Export App keys
* mvn -P test apigee-config:exportAppKeys -Dapigee.config.exportDir=target/test/integration

