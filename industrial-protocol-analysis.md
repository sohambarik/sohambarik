# Vulnerability Analysis: Load Assessment and Traffic Dissection of Modbus TCP & MQTT Environments
### Author: Soham Barik | Industrial IoT Security Researcher

## 1. Executive Summary
Operational Technology (OT) security demands high structural resilience against malicious traffic saturation, fuzzing, and data interception. This research highlights the practical workflow used to evaluate reference industrial communication modules under intensive load conditions while tracking protocol vulnerabilities using Wireshark packet capture analysis.

## 2. Threat Modeling: Modbus TCP vs. MQTT
Industrial gateway deployments rely heavily on legacy and modern message distribution vectors:
- **Modbus TCP:** Operates over standard TCP Port 502. It lacks native encryption and authentication boundaries, making it highly vulnerable to middleman packet manipulation.
- **MQTT (Message Queuing Telemetry Transport):** Lightweight network broker setup. If deployed over cleartext without TLS/SSL configurations, it leaks critical telemetry assets to anyone listening on the wire.

## 3. Practical Simulation Workflows
Under an independent, reference configuration testing lifecycle, the industrial communication interfaces were subjected to persistent packet processing cycles:
1. **Modbus Master Execution:** Polled continuous command structures (Function Code 3: Read Holding Registers / Function Code 6: Write Single Register) to query data outputs from reference target modules and digital energy meters.
2. **MQTT Broker Pipeline:** Generated automated traffic spikes targeting reference broker implementations to evaluate system stability and packet processing boundaries under high load scenarios.

## 4. Traffic Dissection via Wireshark
Using captured network log files (`.pcap`), deep structural analysis was carried out to audit protocol compliance parameters:

### Step 1: Isolating the Telemetry Stream
To filter out background system noise inside the capture panel, specific display filters were applied to isolate the target communication streams:
```text
mqtt or (tcp.port == 502)
```

### Step 2: Cleartext Extraction and TCP Stream Recovery
By using the **"Follow TCP Stream"** option inside the analyzer suite, fragmented hex data bytes were successfully parsed into human-readable text blocks.

### Security Flaw Analysis:
- **Credential & Data Interception:** The packet log analysis revealed that baseline device data distributions, device identifiers, and transmission messages were traveling across the data wire completely unencrypted in reference setups.
- **Replay Attack Surface:** Because the baseline reference implementation lacked cryptographic transaction challenges, an attacker on the same local network layer could record a valid command string and replay it onto the wire to simulate unauthorized register manipulation.

## 5. Defensive Remediations
1. **Enforce MQTTS:** Transition all telemetry message streams to run over TLS/SSL frameworks to completely neutralize cleartext visibility.
2. **Network Isolation (Segmentation):** Place legacy Modbus TCP links within isolated VLAN segments backed by strict firewall layouts to mitigate lateral traversal risks.

## 6. Conclusion
Evaluating industrial gateways under stress helps identify critical performance boundaries and hidden data leaks before deployment. Securing the modern factory floor requires shifting legacy cleartext communications to encrypted, authenticated alternatives to prevent targeted physical infrastructure disruption.
