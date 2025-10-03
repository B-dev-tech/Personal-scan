Personal Scan

Project by B dev (2025)

Personal Scan is a CLI Python tool to check whether your email appears in public data breaches using legal and trusted sources like Have I Been Pwned (HIBP) API.
No dark web scanning or illegal methods are used — this project is fully defensive and legal.


---

Main Files

scan.py — main Python script

pscan — Linux/Mac shell script to run scan.py

pscan.bat — Windows batch file to run scan.py

LICENSE — MIT License

README.md — this file



---

Installation

1. Clone the repository



git clone https://github.com/B-dev-tech/Personal-scan.git

2. Go to project folder



cd Personal-scan

3. Install dependencies



python -m pip install requests rich

4. Make shell script executable (Linux/Mac)



chmod +x pscan


---

Optional: Run pscan from anywhere

You can run pscan start --rich without ./ by:

Option 1: Move to system PATH (Linux/Mac)

sudo mv pscan /usr/local/bin/
sudo chmod +x /usr/local/bin/pscan

Option 2: Create alias

alias pscan="/full/path/to/Personal-scan/pscan"
source ~/.bashrc  # or ~/.zshrc


---

How to Use

Linux / Mac

pscan start --rich

Windows

pscan.bat start --rich

Example output:

Welcome to personal scan
By B dev

Type your email
Example: example@gmail.com
> user@example.com

Starting scan.....

Scan successfully
Email : user@example.com
Security : Good / Bad
> You don't have any leaked data
> Or you have leaked data at the following sites:
  - Example breach (example.com)


---

scan.py

#!/usr/bin/env python3
import os
import argparse
import requests
import time

try:
    from rich import print as rprint
    from rich.prompt import Prompt
    from rich.console import Console
    from rich.panel import Panel
    RICH_AVAILABLE = True
    console = Console()
except Exception:
    RICH_AVAILABLE = False
    console = None

HIBP_API_KEY = os.getenv("HIBP_API_KEY")
HIBP_BREACHEDACCOUNT_URL = "https://haveibeenpwned.com/api/v3/breachedaccount/{account}"
USER_AGENT = "PersonalScanByBDev/1.0"

def banner(use_rich=False):
    txt = "Welcome to personal scan\nBy B dev"
    if use_rich and RICH_AVAILABLE:
        console.print(Panel(txt, subtitle="Personal Scan", expand=False))
    else:
        print(txt)

def ask(prompt_text, use_rich=False):
    if use_rich and RICH_AVAILABLE:
        return Prompt.ask(prompt_text)
    else:
        return input(prompt_text + "\n> ")

def check_hibp_email(email):
    if not HIBP_API_KEY:
        raise RuntimeError("No HIBP API key set. Set HIBP_API_KEY environment variable to use HIBP.")
    headers = {"hibp-api-key": HIBP_API_KEY, "user-agent": USER_AGENT, "Accept": "application/json"}
    url = HIBP_BREACHEDACCOUNT_URL.format(account=email)
    params = {"truncateResponse": "false"}
    resp = requests.get(url, headers=headers, params=params, timeout=15)
    if resp.status_code == 200:
        return resp.json()
    elif resp.status_code == 404:
        return []
    else:
        resp.raise_for_status()

def simulate_search_feedback(duration=2, use_rich=False):
    if use_rich and RICH_AVAILABLE:
        with console.status("Starting scan.....", spinner="dots"):
            time.sleep(duration)
    else:
        print("Starting scan.....")
        time.sleep(duration)

def display_result(email, breaches, use_rich=False):
    status = "Good" if not breaches else "Bad"
    msg = f"Scan successfully\nEmail : {email}\nSecurity : {status}"
    if use_rich and RICH_AVAILABLE:
        console.print(Panel(msg, style="green" if status=="Good" else "red"))
    else:
        print(msg)

    if not breaches:
        print("> You don't have any leaked data")
    else:
        print("> You have leaked data at the following sites:")
        for b in breaches:
            title = b.get("Title") or b.get("Name")
            domain = b.get("Domain") or "-"
            print(f"  - {title} ({domain})")

def main():
    parser = argparse.ArgumentParser(description="Personal Scan CLI")
    sub = parser.add_subparsers(dest="cmd")
    start = sub.add_parser("start", help="Start scan")
    start.add_argument("--rich", action="store_true", help="Use rich for nicer output")

    args = parser.parse_args()
    use_rich = getattr(args, "rich", False) and RICH_AVAILABLE

    banner(use_rich)

    if args.cmd == "start":
        email = ask("Type your email\nExample: example@gmail.com", use_rich).strip()
        simulate_search_feedback(use_rich=use_rich)
        breaches = []
        if HIBP_API_KEY:
            try:
                breaches = check_hibp_email(email)
            except Exception as e:
                print(f"Error when contacting HIBP: {e}")
        display_result(email, breaches, use_rich)
    else:
        parser.print_help()

if __name__ == "__main__":
    main()


---

pscan (Linux/Mac)

#!/bin/bash
python3 scan.py "$@"

pscan.bat (Windows)

@echo off
python scan.py %*


---

LICENSE (MIT)

MIT License

Copyright (c) 2025 B dev

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.


