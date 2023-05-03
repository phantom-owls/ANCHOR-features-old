<h2><b>Technical Report: Engineering Features for ANCHOR based on MAP/Diameter Signaling Traffic</b></h2>

This additional material should be seen as accompanying material to the paper "ANCHOR: Proactive Anomaly Detection for IoT
Connectivity in Global Roaming".
 
 <h3> Introduction </h3>

Support for “things” operating globally has become critical for Internet of Things (IoT) verticals, from connected cars to smart sensors to wearables. IoT verticals deploy their devices world-wide, while keeping operational simplicity in terms of managing the connectivity of their devices and customers. IoT managed connectivity providers (such as Twilio, EMnify, or Truphone) answer these needs by exploiting the roaming functions within the cellular ecosystem. As a result, the communications between IoT devices and applications depend on a complex ecosystem composed of multiple network domains, often operated by different organizations. Within this ecosystem, we separate the IoT logical layer (which deals with the IoT applications supported, and their connectivity requirements), and the infrastructure layer (which supports the service of the IoT provider).

On the IoT logical layer, IoT customers (e.g., car manufacturers, fleet tracking service providers, connected elevator manufacturers) contract managed connectivity from IoT providers. These platforms provide their customers connectivity via a set of SIMs that can be installed in IoT devices and used globally (also called global SIMs). They also provide management in a centralized manner and other added values services (e.g., security, APIs) for the SIMs provisioned to every customer.

IoT providers do not own the infrastructure layer, but rather contract the global SIMs from one or multiple Mobile Network Operators (MNOs). These MNOs then provide the SIMs’ unique identifier, the IMSI. The MNOs are therefore the home networks enabling connectivity via their mobile infrastructure to IoT devices operating world-wide. 

<h3> Feature Engineering </h3>

We do feature engineering to transform raw data into effective features for anomaly detection. Our raw dataset includes raw Diameter and MAP dialogues from a sample of approximately four million IoT devices operating world-wide. The dialogues follow the official protocol specifications. We employ the domain knowledge of the IoT Provider operations team to define the per-dialogue protocol fields that best serve the anomaly detection task. The fields we consider include for each SIM the dialogue start date and time, its duration, a format violation indicator (to help eliminate malformed dialogues), the dialogue request type (the operation code such as Send Authentication Information Request (SAIR), or Update Location (UL) request) and the result code (showing whether the request was successful, or it was rejected). We also check the identity of the home and visited operators, and (where available) the identity of the core network elements involved in the signaling dialogue.

In total, we include 95 features including overall statistics on i) signaling traffic volume, ii) message types and signaling patterns, iii) device activity, iv) mobility statistics, and v) longitudinal activity statistics. The features cover different levels and directions to describe devices as holistically as possible from the point of view of the signaling protocols we monitor. For all features, we consider the period of time of interest (one month, in our case, matching the billing period), extract the timeseries of all records for each device per day and compute the statistical features. Our intent is to characterize the overall device behavior, and avoid sudden changes and anomalies that may occur on short timescales.

Since most IoT verticals deploy devices world-wide (outside the SIMs’ home country), MNOs use their roaming agreements to guarantee connectivity to the global SIMs installed in the IoT devices. International telco carriers that offer the roaming hub service interconnect the home network with any other visited network, thus increasing the geographical footprint of the IoT provides outside the borders of their respective MNOs’ home countries.
As part of designing our method, we first address several system design questions before developing our detection engine. For instance, to meet the requirements of a scalable solution and ensure that significant resources are available, we design a central anomaly detection solution that runs in the analytics platform operated by the IoT provider. To tackle the challenge, we rely on a comprehensive, unique dataset that records the control signaling traffic of a variety of IoT devices operating worldwide (spanning 40 countries) for multiple IoT vertical applications.

<h3> Feature Description </h3>

Next, we introduce all the 95 features and provide additional information where needed. We mention that we generate these features on a daily basis, over a period of 30 consecutive days (corresponding to a full billing period). 

 <table>
  <tr>
    <th>Feature Name</th>
    <th>Description</th>
    <th>Relationship to other features</th>
  </tr>
  <tr>
    <td>n_operator_diam</td>
    <td>Number of Visited MNOs from Diameter dialogues (4G connectivity).</td>
    <td></td>
  </tr>
  <tr>
    <td>n_operator_map</td>
    <td>Number of Visited MNOs from MAP dialogues (2G/3G connectivity).</td>
    <td></td>
  </tr>
   <tr>
    <td>n_vlr_unique</td>
    <td>Number of unique Visited Location Registries (VLRs) involved in the MAP signalig dialogues that we capture per day.</td>
    <td></td>
  </tr>
    <tr>
    <td>n_sgsn_unique</td>
    <td>Total number of unique Serving GPRS Support Node (SGSNs) involved in the MAP signalig dialogues we capture in one day.</td>
    <td></td>
  </tr>
     <tr>
    <td>n_operator_changes</td>
    <td>Number of visited operator changes that we observe from the daily feed of MAP dialogues.</td>
    <td>We use this feature as a signal for device mobility; it should correlate with n_sgsn_unique and n_vlr_unique. </td>
  </tr>
      <tr>
    <td>n_vlr_changes</td>
    <td>Number of VLR changes that we observe from the daily feed of MAP dialogues.</td>
    <td>We use this feature as a signal for device mobility. </td>
  </tr>
       <tr>
    <td>n_sgsn_changes</td>
    <td>Number of SGSN changes that we observe from the daily feed of MAP dialogues.</td>
    <td>We use this feature as a signal for device mobility. </td>
  </tr>
 
</table> 
