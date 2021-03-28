# SMPP SMSC Simulator
smpp-smsc-simulator

This code was used as part of the Melrose Labs <a href="https://melroselabs.com/services/smsc-simulator/">SMPP SMSC Simulator</a> service.  The SMSC simulator (or SMPP simulator) is used to test an SMS application's support for the <a href="https://smpp.org">SMPP</a> protocol.

Build
=====

```
g++ -std=c++11 smscsimulator.cpp -o smpp-smsc-simulator
```

Run
===

```
./smpp-smsc-simulator
```

The SMSC will listen for connections on port 2775.

If you wish to use TLS with the SMSC simulator, then you should put an AWS load balancer in front of the server on which you run the simulator or use openssl to bridge to the SMSC simulator.


The simulator supports a number of command line parameters:

```
❯ ./smpp_smsc_simulator --help
./smpp_smsc_simulator build time: Mar 28 2021 11:35:17
SMPP Smsc Simulator
	--help		Print this help and exit.
	--enquireLinks	Print enquire link and enquire link responses on console.
	--quiet		Do not print smpp commands on console.
	--port		Smpp port to listen to. Default if not specified 2775.


Useful hints:
	- if ESME submits dest.addr == 111, then simulator introduces a 10 seconds delay before sending a reply for submit_sm
	- if ESME submits dest.addr == 222, then simulator does not send a reply for submit_sm
	- if ESME submits dest.addr == 333, then simulator replies with ESME_RSUBMITFAIL
	- if ESME submits dest.addr == 444, then simulator replies with ESME_RTHROTTLED
	- if ESME submits dest.addr == 555, then simulator replies with ESME_RMSGQFUL
	- if ESME submits dest.addr == 666, then simulator replies with ESME_ROK, but generates a delivery report with status undelivered
	- if ESME submits dest.addr.len < 8, then simulator replies with ESME_INVDSTADR
	- if ESME submits system id in dest.addr, then simulator replies with a MO deliver_sm
```

Docker
===

Alternatively you can build and run the SMSC simulator in docker

```
docker-compose up
```

This will build and run the SMSC simulator in Docker on port 2775.

Canned replies
==========

The SMSC can simmulate different replies depending on some conditions specified by ESME:

* if dest.addr == 111 -> introduce 10s delay before successful submit_sm_resp
* if dest.addr == 222 -> do not send a reply for a SUBMIT_SM
* if dest.addr == 333 -> reply with submit_sm_resp with status ESME_RSUBMITFAIL
* if dest.addr == 444 -> reply with submit_sm_resp with status ESME_RTHROTTLED
* if dest.addr == 555 -> reply with submit_sm_resp with status ESME_RMSGQFUL
* if dest.addr == 666 -> reply with submit_sm_resp with status ESME_ROK, but generate a delivery report with status UNDELIVERED
* if dest.addr.len < 8 -> reply with submit_sm_resp with status ESME_INVDSTADR
* if system_id is in destination address -> reply with submit_sm_resp with status ESME_ROK, but also generate a deliver_sm mobile originated message


References
==========

Melrose Labs SMPP SMSC Simulator - https://melroselabs.com/services/smsc-simulator/ (service running newer version of code)

SMPP protocol - https://smpp.org
