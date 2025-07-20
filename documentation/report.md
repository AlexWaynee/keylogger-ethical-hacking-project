📘 Title:
Keystroke Logging: Understanding Keyloggers Through Ethical Hacking

👨‍🎓 Abstract
This project demonstrates the working of a basic keylogger written in Python, built for educational purposes under a Cybersecurity and Ethical Hacking course. A keylogger is a software tool designed to record user keystrokes. While malicious in real-world scenarios, our implementation is a controlled and ethical demonstration, focusing on understanding potential threats, their mechanisms, and defensive strategies.

🎯 Objective
Develop a basic Python keylogger

Understand how keystroke logging works at a technical level

Explore ethical and legal implications of using keyloggers

Identify countermeasures and preventive techniques

🔍 Introduction
Keyloggers are among the most common tools used in cyberattacks to capture sensitive data like passwords, banking information, and personal messages. This project takes an ethical approach to build and test a keylogger for academic understanding. Inspired by the YouTube video "Keylogger using Python", we implemented a simple logger using the pynput library in Python that captures keyboard events and writes them to a log file.

⚙️ Tools & Technologies Used
Tool / Library	Purpose
Python 3.x	Programming Language
pynput		Captures keyboard events
logging module	Records keystrokes to a log file
Git & GitHub	Version control and collaboration
Notepad / VS Code	Code editing and debugging

🧠 Working Principle
The Python keylogger works as follows:

Keyboard Hooking: The pynput.keyboard.Listener is used to monitor and capture each key pressed.

Logging: Captured keystrokes are logged using Python’s logging module into a .txt file with timestamps.

Execution Loop: The program continuously listens and logs until manually stopped (Ctrl + C or force exit).

Output: All logs are saved to keylog.txt in the same directory as the script.

The keylogger does not run in the background, hide its process, or send logs remotely — this keeps it ethically sound and classroom-safe.

🔐 Code Overview
python
Copy code
from pynput import keyboard
import logging

logging.basicConfig(filename="keylog.txt", level=logging.DEBUG, format="%(asctime)s: %(message)s")

def on_press(key):
    try:
        logging.info(f"Key pressed: {key.char}")
    except AttributeError:
        logging.info(f"Special key pressed: {key}")

with keyboard.Listener(on_press=on_press) as listener:
    listener.join()
on_press() handles both character and special key logging

logging.basicConfig() defines the output format and file location

⚖️ Ethical & Legal Considerations
Keyloggers are illegal when used without consent.

Malicious keyloggers are often used by cybercriminals to steal passwords and personal information.

In corporate security, authorized keyloggers may be used for compliance and monitoring.

In this project, we emphasize ethics: our tool is transparent, visible, and used in a controlled environment.

🛡️ Prevention Techniques
Defense Measure	Explanation
Antivirus Software	Detects and blocks known keyloggers
Behavioral Monitoring	Tracks suspicious activities like keystroke logging
On-Screen Keyboards	Bypass physical keystrokes
Endpoint Security	Prevents unauthorized software installation
User Awareness	Critical for identifying phishing or fake downloads

🧪 Testing & Output
We tested the keylogger in a controlled environment:

Typed input: "Hello123!"

Log Output:

yaml
Copy code
2025-07-20 18:05:32,145: Key pressed: H
2025-07-20 18:05:32,278: Key pressed: e
2025-07-20 18:05:32,400: Key pressed: l
...
The keylog.txt file stored all keystrokes accurately.

📌 Limitations
Only logs keyboard inputs — no screen capture or clipboard monitoring

Works only while the script is running in the terminal

Easily detectable by modern antivirus software

Does not hide itself or auto-start

📚 References
YouTube Video: "Keylogger using Python"

Python Docs: pynput, logging

OWASP Top 10 Security Risks

CERT-In and GDPR for legality of data monitoring

✅ Conclusion
This project offered practical exposure to understanding how simple but dangerous tools like keyloggers operate. By implementing it in a legal, safe, and educational manner, we learned not only the technical aspects but also the serious ethical responsibilities of cybersecurity professionals.