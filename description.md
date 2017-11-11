# kallCoin
Decentralised real time voice and video network overcoming the problem of centralized networks and shortcomings of federated networks.

## Aims
* deal with centralisation and walled garden problem of networks (having effective monopolies due to “network effect”)
* deal with problems inherent in federated networks (ie. why WatsApp won over XMPP) [1]
* ability to call from any to any device (e.g. regular voice call from mobile phone network to a “logged in” web browser)
* focus on reachability, and quality of service (ie. a call should “just” work), over features
* ability to reward centralised network owners for access to their networks
* provide monetization ability in the decentralised network

## Glossary
* client - a destination and/or source of media
* human clients - enables humans to make/receive calls.
* machine clients - utilised to provide auxiliary services to the network. E.g. a “voicemail” client,  a voice enabled AI client etc
* service - infrastructure piece necessary to facilitate connection between two or more clients
* service types - predefined list of services and their protocols
* the ledger - decentralised consensus ledger necessary for the decentralised media network (e.g. can implement on top of Ethereum and/or Tezos networks)
* registrar - list of entries in the ledger specifying details for “adding” clients or services to the network
* client registrar - subset of registrar, where clients specify necessary details to contact them 
* service registrar - subset of registrar, where clients specify necessary details to utilise them
* network API version - version of full specification of the decentralised network 
* kallCoin - utilised to pay for provision of services, machine clients, and for calculating voting power in kallCoin network spec advancement votes
* invitation/initiation - clients and services can join each other by invitation or initiation. E.g. if Bob calls Alice, he joins the call by initiation, while Alice joins by invitation
* accounts - public key and kallCoin balances for services and clients
* statistics - a segment in ledger, where statistics about provided services is signed and stored by clients

## Client
For clients to be part of network, they must “register” to the network. “Registration” is addition of an entry in client registrar. Entry contains:
* public key identifying the “account”
* supported network API versions
* (optional) details for making a direct connection (e.g. ip of the client device)
* list of services providers. E.g. if direct connection is not possible, details of a TURN service (normal TURN server + signalling support) provider for the client. There can be a list of multiple service providers for a single service type, giving the connecting client or service option to choose the best provider. Service providers must digitally sign their agreement to act as provider. This signature must be included here.

Entries in the registrar can be updated only by signing the new registration message with corresponding private key.

Private keys have associated balances of kallCoins with them, which are primarily used for payment of services.

Clients can sign and add entry in statistics evaluating quality of provided service. This gives opportunity for service providers to build “reputation”. Clients are not motivated to lie, as quality of stats provided can be weighted by the kallCoins balance associated with the account. The bigger balance, the more reliable the stats can be considered to be, as bigger the ballance, the more invested in the network is the clients.

## Services
Services also must register to the registrar. Their entry consists of: 
* public key
* network API version
* service type
* cost of usage - A list of three integer tuples (kallCoins per 15 sec, network API version, how cost is split between inviter and invitee for using service). 
* (optionally) geographic location
* extra details for specific service type to enable its utilisation

Possible services include:
* stun service
* proxy services (aka turn server + signalling)
* gateway service (skype gateway, isdn)
* etc.

## Examples

### Direct call

For Bob to make a call to Alice, Alice must have an entry in registry. Bob fetches her entry (identified by public key) from the ledger. If it is possible to connect directly, Bob does so using a call flow protocol (eg. possibly SIP).

### Simple call though invitees proxy service

For Bob to make a call to Alice, Alice must have an entry in registry. Bob fetches her entry (identified by public key) from the ledger. Bob picks the best service provider (e.g. based on costs, and supported network API version, reputation) and initiates a call utilising the details using call flow protocol though the proxy server. The service provider gets remunerated for provision of service.

### Simple call though inviters proxy service
It is possible that Bobs client is quite simplistic. In this case, Bob requests his proxy service to initiate a call to Alice. The proxy service makes a call to Alice as a client could if it wished (directly or through some other service). The service provider gets remunerated for provision of service.

## Payments

Payments for services happens in incremental manner, by making standard token transfers from one account to another utilising ledger. Service can optionally demand from a client a “deposit”, to protect themselves from clients reneging on promise of payment. Clients in turn can evaluate service providers based on their reputation derivable from the statistics section of ledger if to agree to service demands.

## Network specification updates

Once per X blocks of ledger a time limited vote shall be held on next API specification. To participate in voting, accounts need to register interest for every vote. Account voting power is based on their kallCoin balances. Accounts with most voting power can move forwards maximum of five proposals (including a no change proposal). Accounts can delegate their voting power to other accounts. Proposal is represented with a hash of the new specification. Voting is done with transferable vote system. The new API index then represents the new hash. 

## References
[1] https://whispersystems.org/blog/the-ecosystem-is-moving/

[2] https://tools.ietf.org/html/draft-jennings-rtcweb-signaling-01
