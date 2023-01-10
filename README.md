##P5 - Create a block chain app for loyalty points with Hyperledger Fabric Ethereum Virtual Machine. Deploy Fabric locally with EVM and create a proxy for interacting with a smart contract through a Node.js web app

1 . Login as a root
	
	Install go 
	Install docker
	Install Docker compose
	Install node install npm
	
2 . Remove all your docker containers and images
	
	Docker stop $(docker ps -a -q)
	Docker rm $(docker ps -a -q)
	Docker rml $(docker images -q) -f
	
3 . set gopath to your go installation
	
	Export GOPATH=#HOME/go
	Cd $GOPATH

4 . Clone the fabric-chaincode-evm repo in your gopath directory
	
	Mkdir src
	Cd src
	Mkdir github.com
	Cd gihub.com
	Mkdir Hyperledger
	Cd Hyperledger
	Cd $GOPATH/src/github.com/Hyperledger/
	Git clone https://github.com/hyperledger/fabric-samples.git 

5 . Checkout release-0.1
	
	Cd fabric-samples
	Git checkout release1.4

6 . now navigate back to your fabric samples folder . Here we will use firs-network to launch the network
	
	Cd $GOPATH/src/github.com/Hyperledger/fabric- samples/first-netowrk

7 . update the docker-compose-cli.yml with the volume to include the fabric-chaincode-evm
	
	Chmod+x docker-compose-cli.yml
	
	Gedit docker-compose-cli.yml
	…. /…/fabric-chaincode-evm:/opt/gopath/src/github.com/Hyperledger/fabric-chaincode-evm

8 . Generate certificates and bring up the network
Cd bin/
	Cp crpytogen ../first-network/
	Cd ..
	Cd first-network/
	./byfn.sh generate
	./byfn.sh up

9 . navigate into the cli docker container
	Cd ..
	Docker exec -it cli bash
	
	Export 	CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/Hyperledger/fabric/peer/crypto/p	eerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
	
	Export CORE_PEER_LOCALMSPID=”org1MSP”
	
	Export 	CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/Hyperledger/fabric/peer/crypto	/peerOrganization/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
	Peer chaincode install -n evmcc -l golang -v 0 -p github.com/Hyperledger/fabric-	chaincode.evm/evmcc

10 . Initiate the chaincode
	
	Peer chaincode instantiate -n evmcc -v 0 -C mychannel -c ‘{“Args”:[]}’ -o ordered.example.com:7050 –tls –cafile /opt/gopath/src/github.com/Hyperledger/fabric/peer/crts/tlsca.example.com-cert.pem
And exit from terminal
	
	Exit

11 . Execute the following to set certain environment variables required for setting up fab3
	
	Export FABPROXY_CONFIG=${GOPATH}/src/github.com/Hyperledger/fabric-chaincode-evm/examples/first-network-sdk-config.yml
	Export FABPROXY_USER=User1
	Export FABPROXY_ORG=0rg1
	Export FABPROXY_CHANNEL=mychannel
	Export FABPROXY_CCID=evmcc
	Export PORT=5000

12 . Redirect to fabric-chaincode-evm directory
	
	Cd fabric-chaincode-evm/
	Cd $GOPATH/src/github.com/Hyperledger/fabric-chaincode-evm/

13  . Run the following to build the fab proxy
	
	Go mod init
	Go mod tidy -e
	Go mod vendor
	Go build -o fab3 ./fabproxy/cmd

14 . You can run the proxy
	./fab3
	
	
	
##P6 - Develop a voting application using Hyperledger and Ethereum. Build a decentralized app that combines Ethereum's Web3 and Solidity smart contracts with Hyperledger hosting Fabric and Chaincode EVM

Code:

pragma solidity 0.8.17; 
contract VotingForTopper{
 address owner; 
 string purpose; 
 struct Voter{
 bool authorized; 
 bool voted; 
 } 
uintTotalVotes; 
uintteamA; 
uintteamB; 
uintteamC; 
 mapping(address=>Voter) info; 
constructor(string memory _name) public{ 
 purpose = _name; 
 owner = msg.sender; 
 } 
 modifier ownerOn(){ 
 require(msg.sender==owner); 
 _; 
 } 
 function authorize(address _person) ownerOn public{
 info[_person].authorized = true; 
 } 
 function teamAF(address _address) public { 
 require(!info[_address].voted,"already voter person");  require(info[_address].authorized,"You have no right for Vote");  info[_address].voted = true; 
teamA++; 
TotalVotes++; 
 } 
 function teamBF(address _address) public { 
 require(!info[_address].voted,"already voter person");  require(info[_address].authorized,"You have no right for Vote");  info[_address].voted = true; 
teamB++; 
TotalVotes++; 
 } 
 function teamCF(address _address) public returns(string memory){  require(!info[_address].voted,"already voter person");  require(info[_address].authorized,"You have no right for Vote");  info[_address].voted = true; 
teamC++; 
TotalVotes++; 
return("Thanks for Voting"); 
 } 
 function TotalVotesF() public view returns(uint){  return TotalVotes; 
 } 
 function resultOfVoting() public view returns(string memory){  if(teamA>teamB){ 
 if(teamA>teamC){
 return "A is Winning"; 
 } 
 else if(teamC>teamA){
 return "C is Winiing"; 
 } 
 } 
 else if(teamB>teamC){
 return "B is Winiing"; 
 } 
 else if(teamC>teamB){
 return "C is Winiing"; 
 } 
 else if(teamA==teamB&&teamA==teamC || teamB==teamC){
 return "No One is Winiing"; 
 } 
 } 
}

1 . Open terminal
	
	Npm install -g truffle
	Git clone https://github.com/tko22/truffle-webpack-boilerplate
	Cd truffle-webpack-boilerplate
	Npm install
	Truffle develop
	Compile
	Npm run dev

	Start localhost:8080

