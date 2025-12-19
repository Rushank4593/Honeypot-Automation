# Enhanced Honeypot Telemetry for Studying SSH Attacker Behavior

**Author:** Sai Rushank Ketha  
**Keywords:** Honeypots · Deception Systems · SSH Attacks · System Telemetry · Attacker Behavior · Security Observability

---

## 🌐 Research Motivation

Internet-facing services such as **SSH** are persistently targeted by both automated and human-driven attackers. While honeypots are widely deployed to collect attack data, comparatively less attention has been paid to **how attacker behavior can be abstracted from low-level interaction logs in a controlled, reproducible, and analyzable manner**.

This project investigates how **deception-based SSH telemetry**, collected via honeypots, can be leveraged to study attacker interaction patterns, credential usage strategies, and command-level behaviors. The long-term goal is to support **behavioral modeling**, **system observability**, and **systems-level security research**.

---

## 🔍 Research Questions

This work is guided by the following research questions:

1. **Behavioral Scope**  
   What types of attacker behaviors can be observed from SSH honeypot interactions beyond simple credential brute-forcing?

2. **Intent Inference**  
   Can command-level telemetry reveal distinct attacker intent patterns, such as reconnaissance, enumeration, or exploitation?

3. **Modeling Limits**  
   What are the inherent limitations of low-interaction honeypots in representing realistic attacker workflows?

---

## 🛡️ Threat Model

| Component | Description |
|---------|-------------|
| **Adversary** | Remote attackers attempting unauthorized SSH access |
| **Capabilities** | Credential guessing, command execution after login |
| **Out of Scope** | Kernel exploits, privilege escalation, lateral movement |
| **Defender Control** | Full control of honeypot environment and telemetry |

---

## ⚙️ Experimental Setup

- **Honeypot:** Cowrie (low-interaction SSH honeypot)
- **Environment:** Kali Linux Virtual Machine
- **Virtualization:** VirtualBox (Bridged Networking)
- **Service Exposure:** SSH on port `2222`
- **Authentication:** Controlled credential database (`userdb.txt`)
- **Primary Data Source:** `cowrie.json` structured event logs

All experiments were conducted within an **isolated virtual lab** to ensure both safety and reproducibility.

---

## 📊 Data Collection

Cowrie generates structured JSON logs capturing rich attacker telemetry, including:

- Connection attempts  
- Authentication attempts  
- Source IP addresses  
- Issued commands  
- Session-level metadata  

Data was collected continuously during both **simulated** and **opportunistic** SSH interactions.

📄 See `data_description.md` for a detailed schema overview.

---

## 🧠 Analysis Methodology

A Python-based analysis pipeline was developed to enable systematic and repeatable analysis:

1. Parsing structured Cowrie JSON logs  
2. Extracting relevant behavioral features  
3. Aggregating statistics across attacker sessions  
4. Visualizing distributions of:
   - Source IP addresses
   - Attempted usernames
   - Command execution frequencies

This methodology supports **empirical analysis of attacker interaction patterns** derived from deception-based telemetry.

---

## 📈 Visualization & Observability

An interactive **Streamlit dashboard** provides:

- Near real-time visibility into honeypot activity  
- Aggregated behavioral metrics  
- Interactive exploration of attacker telemetry  

This component demonstrates how deception-generated data can be integrated into **security observability workflows** and analyst-facing systems.

---

## ⚠️ Limitations

- Cowrie is a **low-interaction honeypot** and does not fully emulate real production systems  
- Attacker behavior may change upon detecting deception  
- Dataset size is constrained by deployment duration  
- Findings should not be over-generalized beyond SSH-based attack scenarios  

---

## ⚖️ Ethical Considerations

- No real user data was collected  
- No outbound attacks were launched  
- The honeypot was isolated from production systems  
- All collected data is used strictly for **academic research purposes**  

---

## 🚀 Future Research Directions

- Behavioral clustering of attacker sessions  
- Integration with **provenance-based system observability**  
- Comparative analysis with high-interaction honeypots  
- Application to **cloud**, **containerized**, and **distributed environments**  

---

## 🔁 Reproducibility

All scripts, configurations, and setup instructions required to reproduce the experimental environment and analysis pipeline are included in this repository.

---

### ✨ Final Note

This project is designed as a **research artifact**, not merely a deployment tutorial. It serves as a foundation for empirical research into attacker behavior, deception systems, and system-level security observability.
