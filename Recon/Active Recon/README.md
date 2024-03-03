# Objectives of Active Recon
Active reconnaissance in API penetration testing involves directly interacting with the target system to uncover as much information as possible regarding its API infrastructure. The essential objectives of active reconnaissance include:
- Identifying Target APIs: Actively seek out the target's APIs through hands-on interaction.
- Scanning Systems: Utilize scanning tools to identify live systems and their open ports.
- Enumerating Open Ports: Determine which ports are open and the types of services they are hosting, specifically those utilizing HTTP.
- Discovering Services Using HTTP: Since APIs typically communicate over HTTP/HTTPS, identifying such services is crucial.
- Uncovering API-Related Directories: By scanning web applications, researchers can discover directories related to APIs.
- Building Out the Target's API Attack Surface: The ultimate goal is to piecemeal together a comprehensive overview of the target's API ecosystem to identify potential vulnerabilities.

# Techniques and Tools for Active Reconnaissance
A variety of tools are employed during active reconnaissance, each serving specific functions, from scanning ports to enumerating services and discovering API endpoints.

## 1. Nmap
Nmap is a versatile tool for discovering services and vulnerabilities on a network.

### Nmap Commands
This command employs default scripts (`-sC`) and service enumeration (`-sV`) against the target, saving the output in three formats for later analysis.
```
nmap -sC -sV [target address or network range] -oA nameofoutput
```
Quickly checks all 65,535 TCP ports to identify any running services, their application versions, and the operating system.
```
nmap -p- [target address] -oA allportscan
```
Uses the NSE script `http-enum` to enumerate web services on standard HTTP and HTTPS ports as well as common alternative web ports.
```
nmap -sV --script=http-enum <target> -p 80,443,8000,8080
```

## 2. OWASP Amass
OWASP Amass gathers information about a target’s external network and services using Open Source Intelligence (OSINT).

### OWASP Amass Commands
Before using Amass, enhance its capabilities with API keys. This command fetches a sample `config.ini` file. Then, obtain and add API keys to this file to improve the tool’s scanning efficiency.
```
sudo curl https://raw.githubusercontent.com/OWASP/Amass/master/examples/config.ini > ~/.config/amass/config.ini
```
This scan will reveal API subdomains that could be of particular interest, like `legacy-api.target-name.com`, suggesting potential vulnerabilities.
```
amass enum -active -d target-name.com |grep api
```
Use the intel command to collect SSL certificates, search reverse Whois records, and find ASN IDs.
```
amass intel -addr [target IPs]
```
Domains can be passed to intel with the whois option to perform a reverse Whois lookup.
```
amass intel -d [target domain] –whois
```
Passively enumerated subdomains.
```
amass enum -passive -d [target domain]
```
Active enum scan does most scans as the passive one, however it will add domain name resolution, attempt DNS zone transfers, and SSL certificate information.
```
amass enum -active -d [target domain]
```
Add the -brute option to brute-force subdomains, -w to specify the API_superlist wordlist, and then the -dir option to send the output to the directory of your choice.
```
amass enum -active -brute -w /usr/share/wordlists/API_superlist -d [target domain] -dir [directory name]  
```

## 3. Gobuster
Gobuster is a command-line tool for brute-forcing URIs and DNS subdomains using wordlists.

### Gobuster Commands
This scan will discover API directories by brute-forcing paths from a wordlist.
```
gobuster dir -u target-name.com:8000 -w /home/hapihacker/api/wordlists/common_apis_160
```

## 4. Kiterunner
Kiterunner offers a novel approach to discovering API endpoints by simulating complex API request patterns beyond the customary HTTP GET requests.

### Kiterunner Commands
This command allows Kiterunner to attempt common API request types and paths, identifying potential API endpoints.
```
kr scan HTTP://127.0.0.1 -w ~/api/wordlists/data/kiterunner/routes-large.kite
```
Use a text wordlist rather than a .kite file.
```
kr brute <target> -w ~/wordlist.txt
```

### DevTools
DevTools in browsers is an invaluable tool for interactive exploration of web applications and APIs. By monitoring network traffic in DevTools, researchers can identify API calls, inspect and modify them, and even resend modified requests to observe behavior. This helps in understanding how the application interacts with its backend APIs.