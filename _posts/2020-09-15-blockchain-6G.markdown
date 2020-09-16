---
layout: post
title:  "Where does Blockchain fit in 6G?"
date:   2020-09-15 15:04:00 +09
categories: Blockchain 6G Wireless
---
#### Author: Steven Platt

With 5G network services reaching initial commercial deployments, work has begun to map how these networks will be evolved and matured for the eventual migration to 6th generation designs expected in 2030. The International Telecommunications Union (ITU-T) has begun formal planning with the establishment of the Focus Group on Technologies for Network 2030, while recent further research has been published providing early 6G performance targets, enabling technologies and use cases. In additional to terahertz spectrum, and visible light communication, blockchain is also identified as a technology core to enabling the advanced information society envisioned with 6G [1]. 

As a technology in early development, it remains difficult to place blockchain into context for wireless design. To date, early research has focused primarily on individual use cases in part to simplify the network context. This framing however does not fully acknowledge heterogeneity in wireless networks, or the rapidly expanding variety of blockchain designs - both of which are expected to increase in complexity as networks move to 6G. In this perspective, we propose positioning blockchain from a macro view. To understand blockchains’ fit with native network operations, we must first acknowledge its data structure, inherited behaviors, and position it relative to alternatives that also functionally serve as distributed ledgers. Viewing blockchain in this format provides a more durable understanding as to the long-term functional fit of the blockchain structure while additionally highlighting areas that may conflict with the format and behavior of networks in 6G. 

In the following sections, we provide a brief overview of enabling technologies and behaviors characterizing future 6G networks. Next, we discuss four distributed ledger data structures, examining their relative scalability, security, and matching them against network behaviors identified in 6G. After, three network function categories are reviewed, providing a starting reference for network operations uniquely enhanced by the inclusion of blockchain in 6G. Last, we narrow focus to the structure of blockchain again, identifying evolution potentials that further enhance its compatibility as networks evolve to 6G.

## TOWARDS A DISTRIBUTED LEDGER TAXONOMY

### Network Behavior in 6G

The information society envisioned by 6G is characterized by globally pervasive data and near unlimited wireless connectivity. This new level of connectivity is enabled by terahertz communications, holographic beamforming, spatially multiplexed MIMO, and additional technologies that combined, move peak data rates to 100 times those of the existing 5G roadmap [1]. Wireless networks in the 6th generation continue to evolve how connectivity is experienced with paradigms in tactile access becoming pervasive, and corresponding network behaviors evolving to support them in the form of new multi-tier and dynamic topology designs not natively supported in 5G.

Bio sensing, hyper high-speed rail, and sightseeing at sea and in space have been identified as use cases specific to the 6G evolution [1]. Pursuing a macro view of these new networks will allow pairing the new network behaviors with distributed ledger structure in a way that remains compatible as its use cases evolve and change. Table 1 shows the macro view of 6G network performance targets and emergent technologies expected to support them. 

| 6G Performance Targets | Enabling Technologies |
| --------------------------- | --------------------------- |
| Peak Data Rate (≥1 Tb/s) | THz Communications |
| User Data Rate (1Gb/s) | Spatially Multiplexed MIMO |
| Latency (10-100μs) | Large Intelligent Surfaces |
| Density (107 Devices/km2) | Holographic Beamforming |
| Traffic Capacity (1 Gb/s/m2) | Visible Light Communications |
| Mobility (≥1,000 km/h) | Quantum Computing |
| Energy Efficiency (100x) | Blockchain |
| Spectrum Efficiency (5-10x) | Artificial Intelligence |

*TABLE 1:* Defining 6G: performance targets and enabling technologies [1]. 
6G Performance Targets and Enabling Technologies are not pairwise andare listed here in random order.

### Finding Utility in Distributed Ledger Structure

In tandem with the development of blockchain, alternative ledger structures have been produced, or evolved to address known limitations, or provide specific focus for behaviors that were secondary or underdeveloped in blockchain. Research has identified a “trilemma” within these systems, noting that at best, they may perform well on only two of three axis: decentralization, scalability, and security [2]. This framing is important as it helps set expectations of system performance for ledger systems embedded in 6G design. In the context of this trilemma, we can deduce, for example, that a data structure adopted for high scalability and decentralization traits, makes tradeoffs in security, making it less suitable for network access control. Or conversely a ledger structure with high inherent security and high decentralization will do so at the expense of scalability - resulting in limited compatibility for network intelligence operations such as the handling of streaming data. The following section provides a representative sample of ledger data structures beyond blockchain, detailing the behaviors inherent to each corresponding data structure. The data structures include: Distributed Hash Table, Directed Acyclic Graph, and Block Lattice, and finally Blockchain.

#### Distributed Hash Table

![distributed hash table](/assets/img/dht.jpg)

Distributed hash tables (DHT) operate as a peer-to-peer network layer for storing data. DHT systems operate by spreading data among peers in a network, with a cryptographic function being used to either randomly select, or select in a set distribution, which peers store the data. Like a traditional database, DHT systems function as a key value store where a lookup is required to gain a list of nodes which store the file(s) being requested [3]. It is the oldest distributed ledger design presented here, having underpinned early internet file sharing applications including versions of BitTorrent. Because the selection of nodes which store data is distributed, data stored in distributed hash tables is highly decentralized with high throughput, making it well suited for 6G applications relying on hyper density, including data caching and edge compute. DHT systems may store multiple copies of a file or segment files to further enhance these traits. Devices participating in a distributed hash table system can join and leave the system at any time, allowing long term compatibility in 6G dynamic and multi-tiered topologies where network access may be on-demand or temporary. 

Negative areas of impact for DHT systems are in transparency and security. The selection process of storage nodes as participant numbers are changing makes it difficult to audit activity or trends in deployment. Security is reduced in the system overall, by a possibility that modified, malicious, or duplicate data records can be added and shared - although partial mitigation is possible using hashed message digests such as MD5 values to improve trust of records distributed. Fig. 1 shows a representation of the distributed hash table data structure.

#### Directed Acyclic Graph

![directed acyclic graph](/assets/img/dag.jpg)

Directed Acyclic Graph (DAG) structures such as IOTA [4] modify the linked hashing of data to use a graph structure that allows it to expand exponentially. IOTA achieves this by allowing data blocks to be added by validating only two edge blocks (tips) selected at random which forms its graph structure. Decentralization in this format is high due to the linear effort of block contribution tied to the forward hashing from two previous graph tips. This native behavior of high throughput allows DAG structures to easily handle streaming network data, of the kind required for high level network intelligence (AI) systems that power high levels of autonomy in 6G.

Having a low barrier of participation conversely creates circumstances that can result in restricted availability as increases in block volume also increases storage usage and the probability that nodes in a network cannot update local records and remain current as new blocks arrive. A second limitation inherited in DAG-based distributed ledgers is the finality of the data stored in its blocks. Because block truthfulness is weighted by the number of forward validations from a graph tip, the state of the data in DAG structures is probabilistic rather than deterministic as certainty increases with each forward block, giving the system moderate levels of security. A consensus mechanism that enforces determinism as seen in blockchain is not possible in graph structures, based on current research - making the systems a poor fit for rule-based operations, such as spectrum sharing policy in 6G which relies binary “allowed” or “not allowed” states. Fig. 2 shows a representation of the directed acyclic graph data structure.

#### Block Lattice

![block lattice](/assets/img/block_lattice.jpg)

At the time of writing, block lattice structures are early in development, with the earliest block lattice cryptographic construction being outlined by Miklos Ajit in 1996 [5]. Modern lattice structure, such as QLC Chain [6] focus on the reduction of storage requirements compared to blockchain, which stores an ever-expanding block history. Rather than a single monolithic history that is stored and maintained at each node, block lattice platforms require participants to only store their personal ledger, with each block of data having a corresponding block stored in the ledger of the device or system on the other end of the interaction. These records become immutable as independent nodes complete further interactions, effectively spreading or sharding dependencies and storage within the wider environment. The transactions tie together in mutual dependency, the ledgers of peer nodes which expands over time, creating its mesh or lattice structure.

Block lattice structures have further benefit in that they potentially simplify consensus mechanisms, as consensus on balance and block state is only required between peers participating in an interaction - presenting an attractive alternative to Proof-of-Work implementations relying on hash power, or BFT consensus which encounters higher bandwidth use under multiple rounds of peer confirmation. The lack of unified history and isolated nature of transactions makes lattice structure highly compatible with operations existing exclusively device-to-device, such as the payments and user data exchange in hyper dense environments at the expense of higher-level network infrastructure use. Fig. 3 shows a representation of the block lattice data structure.

#### Blockchain

![blockchain](/assets/img/blockchain.jpg)

Unlike previously mentioned ledger structures which gain flexibility through modification and fragmentation of the underlying hashed-linked storage, blockchain allows little manipulation of its base structure outside of consensus model and block size. All nodes participating in blockchain systems must agree on the state of each block added, and the sequence of added blocks - regardless of who authors or ultimate users of the data inside of blocks. This rigid chain structure guarantees a record that is always identical for all nodes in the system, making it especially well-suited to policy-based operations demanding high transparency and auditability, including software defined radio, roaming in hyper mobility, and expanded spectrum resource sharing in 6G. By modifying the algorithms for achieving consensus, blockchain systems can be manipulated to process higher transaction volumes, improve security or control resource usage by allowing nodes to keep full (full-node) or partial (light-node) states [7]. Deploying such modification however, shows consensus latency in blockchain under best case scenarios, are reduced to reach 1 second, confirming that blockchain structure remains incompatible with operations at µ-second scale, such as real-time radio resource control and dynamic accesses not set on a semi-permanent basis. Fig. 4 shows a representation of the blockchain data structure.

As summary, table 2 below shows the previously presented data structures as well as the corresponding network behaviors compatible with each.

| Data Structure | Network Behavior |
| --------------------------- | --------------------------- |
| Distribtued Hash Table | Dynamic Topology & Hyper Density | 
| Directed Acyclic Graph | Network Autonomy |
| Block Lattice | Hyper Density | 
| Blockchain | Network Softwarization |

*TABLE 2:* Mapping data structure to network behaviors. 
Placing  focus  on  network  behavior  allows  deployment  of  blockchainwithout binding it directly to specific enabling technologies.


## Blockchain Applications in 6G

With an awareness of the behavior and performance of blockchain structure, it is confirmed that blockchain works best for network policy and record keeping that is set on a semi-permanent basis. Deploying blockchain in this context allows full realization of its utility as a system of accounting and storage, that can be shared among peer networks which have a mutual incentive to coordinate - but are otherwise competitors or lack a formal trust. Extending from this, blockchain is most functional for secure accounting and expected to see natural adoption when it increases environmental awareness or ability to operate efficiently. Deployment scenarios are far less likely for applications that require network operators to agree on configuration or deploy matched or compatible hardware. Fig. 5 visualizes this relationship between distributed ledger data structure and network behavior. The following section extends this behavior relationship to identify three specific 6G use cases utilizing blockchain as secure, auditable, and decentralized storage in a way that enhances network operation even when deployed in a competing network operator context. These 6G uses cases are: software defined networking, spectrum sharing, and public utility services.

### Software Defined Networking

6G networks will add additional network dimensions to support a new mix of infrastructures ranging of satellite constellations to marine infrastructures for support of new services deployed to space and sea. This adds further complexity and heterogeneity in both the network core and radio access layers. Beyond the increase in service heterogeneity, the use of terahertz frequency bands increases energy usage, requiring additional configurability to operate network hardware efficiently. Network softwarization built on network function virtualization, software defined radio, and general network slicing technique matured in 5G will be extended to support these additional network dimensions. As network infrastructure is increasingly abstracted and replaced with application-controlled architectures in 6G, use of blockchain allows more direct coordination for Mobile Network Operators (MNO) and subscribing Mobile Virtual Network Operators (MVNO) to share and coordinate configuration due to differences in services and customers on either side. Blockchain-powered policy orchestration also increases security and transparency in Distributed Antenna Array (DAS) systems deployed to regulated environments such as light rail tunnels, sport venues, high rise towers and other increasingly specialized shared infrastructures where deployment is restricted or multiple operators are otherwise required to be multi-tenant.

### Spectrum Sharing

In 2016 the Federal Communications Commission (FCC) in the United States, deployed a 3-tier sharing model for the 3.5Ghz citizens broadcast radio service (CBRS) bands in the country. The three tiers policy identifies incumbent rights for military applications, secondary rights for opportunistic access of approved parties, and a third tier for open access in a smaller subset of the airspace with medium access controls matching that of Wi-Fi [1]. In tandem with recent policy changes, research has begun formal investigation into blockchain technologies support for spectrum operations, following statements by the FCC acknowledging that existing rules and technologies for spectrum sharing had become antiquated, highlighting blockchain as a potential path towards this goal as networks move to 6G [2]. As a peer-to-peer system, blockchain provides a desirable method of accounting for spectrum sharing operations as it allows verifiable record keeping among unmanaged peer operators who lack a formal trust.

Deploying blockchain for spectrum sharing has a secondary benefit in that it allows networks to achieve a proactive awareness on a macro scale of networks operating in its coverage zone. The ability to share spectrum, while maintaining coverage and providing service level guarantees has been a barrier to deployment of commercial service on top of shared spectrum. In 6G, this proactive awareness, combined with software-controlled radio policy; network providers can dimension or tune 6G spatially multiplexed MIMO behaviors to maintain cell-less coverage, even as additional networks operate in the shared airspace - providing access consistency in a manner not possible today with cognitive radio alone.

### Public Utility Services

As device density increases, there is also an increasing of public infrastructures such as hyper high-speed rail and sensors that become network enabled in non-commercial contexts, such as for the deployment of safety services. It is difficult for any single provider to provide full coverage for all connectivity contexts planned in 6G deployment (inclusive of space and sea), additionally public infrastructure often span municipality, region, and country boundaries. For a number of countries, there are restrictions placed on deploying municipal cellular networks [8], and there remains no natural path of coverage for these systems outside of commercial subscriber agreements. 

With blockchain, it is possible in this scenario, to deploy a registry of public utility services, such as vehicles in road networks, that are allowed specific connectivity. To date this has been complicated by limitations of IP address mobility across network boundaries, but that complication will be resolved through the transitional period to IPv6 during the deployment of 5G. In 2016, version 2 of the IETF Host Identity Protocol (HIP) standard [9] abstracts the IPv6 address, by placing a “host identity” between the network and higher-level internet layers of the TCP/IP stack. Combining this evolution of identity with securely shared blockchain registries allows public utility wireless services that can take advantage of fluid access across networks as IPv6 addressing becomes native in 6G. Combining mobile network identity with blockchain decentralized secure storage allows these new classes of access to be regulated at the spectrum block, country, or region level in 6G - without tying them to individual carriers coverage who in isolation may not have market incentive to extend coverage, or otherwise allow access.

![6G Design](/assets/img/6G_design.jpg)

## Evolution Potentials of Blockchain

To the authors knowledge, the origins of blockchain began with cryptographers Haber and Stornetta who in 1991 published research titled “How to time stamp a digital document” in the Journal of Cryptography [10]. The problem being addressed in research was how to handle authenticity as increasing amounts of records were existent only in digital form and at the time, lacking verifiability. This early system lacked however the decentralized consensus methods later popularized with Bitcoin, an evolution that was made in order to allow the preexisting blockchain structure to function in a permissionless manner as digital currency. As with Bitcoin, the blockchain structure is expected to further evolve increasing its compatibility in network application. The following section outlines three areas where the blockchain structure is evolving to further its compatibility, these areas are: increased composability, de-emphasizing of incentive model, and independent code execution.

### Increased Composability

![blockchain composability](/assets/img/blockchain_composability.jpg)

The size and resource usage of modern blockchains preclude them from full participation among low power or resource constrained network systems. This is shown in network research such as [11], which require a separation of network operation as a result of using proxy devices able to run intensive proof-of-work calculations, or which having enough storage to retain the entirety of a monolithic ledgers’ history. A natural progression in this scenario is to implement an alternative consensus model, or compression scheme to tune chain operation for the environment, rather than modifying the network structure to suit the chain. For example, if a universal record such as currency balance is not being mandated, it is further possible to form and retire chains for individual network operations as the shared data reaches the end of its useful life. Composability of this type is not possible for the most popular blockchain systems, such as Ethereum, which are delivered as an all-or-nothing design which may bundle behaviors that are superfluous or an active detriment in network environments. This awareness bring pressure to the necessity of tuning or composing the functions of the blockchain which impact its performance in the desired application, these include but are not limited to, abilities to modify block size, consensus model, and block timing (fig. 6). 

Research into the scalability of blockchain shows block throughput can be increased to four orders of magnitude higher than Bitcoin, and reduce latency to as low as 1 second through modification of classic blockchain design [2], To the authors knowledge, Hyperledger Fabric [7] remains the only blockchain projects allowing composability of blockchain component parts, with an early prototype dating to 2016.

### De-Emphasizing of Incentive Model

As a system demonstrated to support contract execution, several experiments have investigated the use of blockchain to incentivize behavior desired in network environment, such as resource sharing. Maksymyuk et al for example, propose a spectrum sharing solution which identifies spectrum owners, infrastructure owners, ISP, and end users as independent participants in a dynamic market driven by the nash equilibrium game theory [12]. In the Maksymyuk model, end users make digital currency payments to infrastructure (base station) operators, who in turn, pay for dynamic spectrum access to incumbents and regulators, while also paying for ISP backhaul services to carry traffic to the wider internet. The nash equilibrium model behaves similarly to other existing blockchain models built atop currency incentive; in network context however, it assumes a level of parity in the distribution of infrastructure, demand, and incentive that is uncommon in production markets. As deployed in network context, current blockchain incentive models are functionally similar to existing research into “neutral carrier” models, which abstract individual carrier businesses and to date have not gained wide market adoption [8]. 

To best fit existing incentives of network environment, blockchain can be evolved to be inclusive of competing operational incentives such as resource management, network investment levels, and demand growth which may be uneven among equivalent providers. Early research by Haber and Stornetta offers a possible path forward to highlight blockchain predating digital currency, in this format, the focus is on the latent utility of verifiable shared data instead of currency and contract incentive. As sharing of limited resources such as spectrum increase in the evolution to 6G, it possible that the higher utility is served through the sharing of context and data, such as tower location and channel occupation - which are required for functional operation for everyone - decoupling any awareness of economic model from the chain. 

### Independant Code Execution

Unlike ledger structures such as distributed hash table, blockchain is a rigidly time ordered structure by nature of its linear forward hashing. The general speed of code execution tied to the contribution of blocks will be inversely proportional and dependent on block consensus time. This means that a blockchain deployment seeing an increase in block contributions, will also see a corresponding decrease in how quickly those blocks, and corresponding information can be processed; all else remaining unchanged. 

In systems such as Ethereum, where end-to-end operations occur within its own virtual machine - Impacts of block additions can be partially controlled by charging a digital currency fee proportional with delay imputed on the system. This type control however is difficult to replicate in network environments, where creating an API or software representation for the unbounded number of machine operations in heterogeneous networks is impractical. Within networks of an identical generation; configuration for antenna geometry, sectoring, deployment density, backhaul capacity, and algorithms therein can differ, conflict, or be upgraded over time. Pointing again to a general benefit of limiting blockchain deployment, to decentralization of data. Unbinding blockchain data storage from code execution such as smart contracts can partially mitigate the original risk and related overhead tied to block delay. 

## Conclusion

Blockchain has gained an increased amount of attention for its potential application in wireless networks. There remains however, a lack of consensus for its natural fit with increasingly heterogeneous network hardware, and in what use cases blockchain can improve on existing network deployments.  With 5th generation network deployment already underway, this article provides a forward-looking view to examine blockchain in a 6G context by examining the underlying structure of blockchain compared to other distributed ledger formats and examining its deployment in the three use cases of software defined networks, spectrum sharing, and public utility service Finally, the article outlines areas of where blockchain is evolving or must change to provide the highest fit in 6G design. 

### References

[1] Zhang, Z. et al. (2019). 6G Wireless Networks: Vision, Requirements, Architecture, and Key Technologies. IEEE Vehicular Technology Magazine, 14(3), 28–41. https://doi.org/10.1109/mvt.2019.2921208.

[2] Xie, J., Yu, F. R., Huang, T., Xie, R., Liu, J., \& Liu, Y. (2019). A Survey on the Scalability of Blockchain Systems. IEEE Networks Magazine, 166–173. DOI: 10.1109/MNET.001.1800290.

[3] Chalaemwongwan, N., \& Kurutach, W. (2018). State of the art and challenges facing consensus protocols on blockchain. International Conference on Information Networking, 2018-Janua, 957–962. https://doi.org/10.1109/ICOIN.2018.8343266.

[4] Popov, S. (2018). The Tangle v1.4.3, 1–28. [Online] Available: \underline{https://www.iota.org/research/academic-papers}. Accessed on: Oct 2, 2019.

[5] M. Ajtai. 1996. Generating hard instances of lattice problems (extended abstract). In Proceedings of the twenty-eighth annual ACM symposium on Theory of Computing (STOC '96). ACM, New York, NY, USA, 99-108. DOI: https://doi.org/10.1145/237814.237838.

[6] Li, C. A., \& Zhao, C. (n.d.). A Multidimensional Block Lattice Public Chain with Smart Contract Support for Network-as-a-Service. [Online] Available: \underline{https://whitepaper.io/document/238/qlcchain-yellowpaper}. Accessed on: Nov 2, 2019

[7] Vukoli, M. (2017). Rethinking Permissioned Blockchains [Extended Abstract]. IBM Research, 3–7. https://doi.org/10.1145/3055518.3055526.

[8] Infante, J., Oliver, M., \& Macián, C. (n.d.). “Wi-Fi Neutral Operator: Promoting cooperation for network and service growth”. ITS Conference on Regional Economic Development, Pontevedra, Spain, 07/2005.

[9] Internet Engineering Task Force (IETF). (2015). Host Identity Protocol Version 2 (HIPv2). [Online] Available: \underline{https://tools.ietf.org/html/rfc7401}. Accessed on: Nov 19, 2019.

[10] Stornetta, W. S., \& Haber, S. (1991). How to Time-Stamp a Digital Document. Journal of Cryptology, 3(2), 99–111. https://doi.org/10.1002/pssb.201300062.

[11] Novo, O. (2018). Blockchain Meets IoT: An Architecture for Scalable Access Management in IoT. IEEE Internet of Things Journal, 5(2), 1184–1195. https://doi.org/10.1109/JIOT.2018.2812239.

[12] Maksymyuk, T., Gazda, J., Han, L., \& Jo, M. (2019). Blockchain-based intelligent network management for 5g and beyond. 2019 3rd International Conference on Advanced Information and Communications Technologies, AICT 2019 - Proceedings, 36–39. https://doi.org/10.1109/AIACT.2019.8847762.
