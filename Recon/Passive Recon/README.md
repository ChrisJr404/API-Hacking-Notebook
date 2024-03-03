# Objectives of Passive Reconnaissance
- Identify API endpoints.
- Discover exposed credentials and sensitive information.
- Collect information on API versions and documentation.
- Understand the API’s purpose and potential business logic flaws.
- Find API keys, JSON Web Tokens (JWT), and other secrets.

# Techniques and Tools for Passive Reconnaissance
## 1. Google Dorking
Google Dorking leverages specific search queries to find information that is not easily visible through regular search attempts.

### Google Dorking Queries
Finds WordPress user directories.
'''
inurl:"/wp-json/wp/v2/users"
'''
Locates API key files.
'''
intitle:"index.of" intext:"api.txt"
'''
Unveils API directories.
'''
inurl:"/api/v1" intext:"index of /"
'''
Searches for sites with a potential XenAPI SQL injection vulnerability.
'''
ext:php inurl:"api.php?action="
'''
Lists potentially exposed API keys.
'''
intitle:"index of" api_key OR "api key" OR apiKey -pool
'''

## 2. Git Dorking
Searching GitHub and other repository hosting services for sensitive information disclosure.

### GitHub Search Techniques
- Search by filename (e.g., filename:swagger.json) or extension (e.g., extension:.json) for specific sensitive files.
- Look for secrets using keywords in the organization’s GitHub repositories such as "api key," "authorization: Bearer," "access_token," "secret," or "token."
- Investigate various GitHub repository tabs like Code, Issues, and Pull requests to discover potential weaknesses.

## 3. TruffleHog
A tool for automatically discovering exposed secrets in git repositories.

### Using TruffleHog
This command initiates a scan of the specified organization's GitHub repositories for secrets.
'''
sudo docker run -it -v "$PWD:/pwd" trufflesecurity/trufflehog:latest github --org=target-name
'''

## 4. API Directories
Platforms like ProgrammableWeb (https://www.programmableweb.com/apis/directory) provide extensive data on APIs.

### ProgrammableWeb API Directory
- Access a searchable database of over 23,000 APIs.
- Find information such as API endpoints, version details, business logic, and SDKs.

## 5. Shodan
Shodan is instrumental in discovering internet-facing APIs and devices.

### Useful Shodan Queries
Basic search for a target’s domain name.
'''
hostname:"targetname.com"
'''
Filters results to those responding with JSON or XML.
'''
"content-type: application/json" and "content-type: application/xml"
'''
Limits results to those with successful responses.
'''
"200 OK"
'''
Searches for web applications using the WordPress API.
'''
"wp-json"
'''

## 6. The Wayback Machine
The Wayback Machine is valuable for examining historical versions of a target’s API or web presence.

### Using The Wayback Machine
- Check for changes in the API documentation to find deprecated or "zombie" APIs.
- Analyze alterations to web pages where APIs were previously advertised.