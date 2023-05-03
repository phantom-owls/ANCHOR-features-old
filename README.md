<h2><b>Technical Report: Engineering Features for ANCHOR based on MAP/Diameter Signaling Traffic</b></h2>

This additional material should be seen as accompanying material to the paper "ANCHOR: Proactive Anomaly Detection for IoT
Connectivity in Global Roaming".
 
 <h3> Introduction </h3>

Support for “things” operating globally has become critical for Internet of Things (IoT) verticals, from connected cars to smart sensors to wearables. IoT verticals deploy their devices world-wide, while keeping operational simplicity in terms of managing the connectivity of their devices and customers. IoT managed connectivity providers (such as Twilio, EMnify, or Truphone) answer these needs by exploiting the roaming functions within the cellular ecosystem. As a result, the communications between IoT devices and applications depend on a complex ecosystem composed of multiple network domains, often operated by different organizations. Within this ecosystem, we separate the IoT logical layer (which deals with the IoT applications supported, and their connectivity requirements), and the infrastructure layer (which supports the service of the IoT provider).

On the IoT logical layer, IoT customers (e.g., car manufacturers, fleet tracking service providers, connected elevator manufacturers) contract managed connectivity from IoT providers. These platforms provide their customers connectivity via a set of SIMs that can be installed in IoT devices and used globally (also called global SIMs). They also provide management in a centralized manner and other added values services (e.g., security, APIs) for the SIMs provisioned to every customer.

IoT providers do not own the infrastructure layer, but rather contract the global SIMs from one or multiple Mobile Network Operators (MNOs). These MNOs then provide the SIMs’ unique identifier, the IMSI. The MNOs are therefore the home networks enabling connectivity via their mobile infrastructure to IoT devices operating world-wide. 

<h3> Feature Engineering </h3>

We do feature engineering to transform raw data into effective features for anomaly detection. Our raw dataset includes raw Diameter and MAP dialogues from a sample of approximately four million IoT devices operating world-wide. The dialogues follow the official protocol specifications. We em- ploy the domain knowledge of the IoT Provider operations team to define the per-dialogue protocol fields that best serve the anomaly detection task. The fields we consider include for each SIM the dialogue start date and time, its duration, a format violation indicator (to help eliminate malformed dialogues), the dialogue request type (the operation code such as Send Authentication Information Request (SAIR), or Update Location (UL) request) and the result code (showing whether the request was successful, or it was rejected). We also check the identity of the home and visited operators, and (where available) the identity of the core network elements involved in the signaling dialogue.

In total, we include 95 features including overall statistics on i) signaling traffic volume, ii) message types and signaling patterns, iii) device activity, iv) mobility statistics, and v) longitudinal activity statistics. The features cover different levels and directions to describe devices as holistically as possible from the point of view of the signaling protocols we monitor. For all features, we consider the period of time of interest (one month, in our case, matching the billing period), extract the timeseries of all records for each device per day and compute the statistical features. Our intent is to characterize the overall device behavior, and avoid sudden changes and anomalies that may occur on short timescales.

