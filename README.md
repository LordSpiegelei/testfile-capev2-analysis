# testfile-capev2-analysis

This repository contains a small Windows script used to generate benign 
"malware like" behaviour for CAPEv2 sandbox tests. It was created as part of a
bachelor thesis to evaluate sandbox detection capabilities.

## Overview

Running `src/malware_simbox/main.py` executes several routines that imitate
malware techniques without performing any harmful action. The key activities
are:

- **File encryption** – a temporary text file in `System32` is written and then
  encrypted.
- **File creation and access** – random files are generated in the `%TEMP%`
  directory and `activity.log` is read to mimic monitoring.
- **Process activity** – Notepad is started and terminated, a PowerShell script
  is run, and a dummy application is installed and removed.
- **User creation and deletion** – local users are created, one of which is
  removed after logging.
- **Network activity** – an HTTP request to `www.google.com` is issued, a small
  TCP server is started locally, a connection is made to it and a short port
  scan of localhost is performed.
- **Registry modifications** – a value is written under
  `HKCU\Software\Microsoft\Windows\CurrentVersion\Run` and a temporary key is
  created and deleted.
- **Self replication** – the script copies itself into
  `%APPDATA%\MalwareSimulation`.
- **Memory dump** – `rundll32` is used with `comsvcs.dll` to create a dump of the
  current process.

All actions are orchestrated in `main.py` where each helper class is executed in
sequence.

## Build Pipeline

The GitHub Actions workflow in `.github/workflows/build.yml` builds a standalone
Windows executable using PyInstaller. The job sets up Python, installs
requirements and produces `CAPEv2Testsuite.exe` as an artifact.
