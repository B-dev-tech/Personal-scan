🔒 Personal Scan

Project by B dev (2025)

Personal Scan is a CLI Python tool to check whether your email appears in public data breaches using legal and trusted sources like Have I Been Pwned (HIBP) API.
No dark web scanning or illegal methods are used — this project is fully defensive and legal. ✅


---

📂 Main Files

scan.py — main Python script 🐍

pscan — Linux/Mac shell script to run scan.py 💻

pscan.bat — Windows batch file to run scan.py 🖥️

README.md — this file 📖



---

⚙️ Installation

1. Clone the repository



git clone https://github.com/B-dev-tech/Personal-scan.git

2. Go to project folder



cd Personal-scan

3. Install dependencies



python -m pip install requests rich

4. Make shell script executable (Linux/Mac)



chmod +x pscan


---

🚀 Optional: Run pscan from anywhere

You can run pscan start --rich without ./ by:

Option 1: Move to system PATH (Linux/Mac)

sudo mv pscan /usr/local/bin/
sudo chmod +x /usr/local/bin/pscan

Option 2: Create alias

alias pscan="/full/path/to/Personal-scan/pscan"
source ~/.bashrc  # or ~/.zshrc


---

🖥️ How to Use

In easy way you can type

python scan.py start --rich

Linux / Mac

pscan start --rich

Windows

pscan.bat start --rich

Example output:

🔹 Welcome to personal scan
By B dev

Type your email
Example: example@gmail.com
> user@example.com

⏳ Starting scan.....

✅ Scan successfully
Email : user@example.com
Security : Good / Bad
> You don't have any leaked data
> Or you have leaked data at the following sites:
  - Example breach (example.com)


---



