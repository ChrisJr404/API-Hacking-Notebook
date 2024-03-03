# API Recon Intro
API Recon is a crucial initial step in API penetration testing aimed at uncovering the API attack surface of a target. The process is divided into passive and active techniques, focusing on identifying how an API can be found and utilized based on its intended use—ranging from public, partner, to private APIs.

# Types of APIs Based on Accessibility
*** 1. Public APIs
Accessibility: Meant to be easily found and used by end-users.
Authentication: May not require authentication if only public information is handled. Otherwise, authentication is typically needed.
Documentation: Providers share documentation that is user-friendly and easily accessible.
# 2. Partner APIs
Accessibility: Intended for use exclusively by partners of the provider and might be harder to locate without partnership status.
Documentation: Documented, though often limited to partners.
# 3. Private APIs
Accessibility: Used within an organization, generally not intended for external use.
Documentation: Less documented than partner APIs, if documented at all.

# API Recon Techniques
# Passive Recon
Objective: Discover APIs and their documentation without engaging the target system directly.
# Active Recon
Involves direct interaction with the target to discover APIs.

# Web API Indicators
Identifying APIs can be straightforward by looking for specific indicators, such as:
- URL Naming Schemes: e.g., https://api.target-name.com/v1, https://target-name.com/docs
- Directory Names: /api, /v1, /graphql, /swagger.json
- Subdomains: e.g., api.target-name.com, dev.target-name.com
- HTTP Headers: Content-Type indicating JSON or XML usage
- HTTP Responses: Messages like {"message": "Missing Authorization token"}
- Third-Party Sources for Discovering APIs:
	- GitHub: https://github.com/
	- Postman: https://www.postman.com/explore/apis
	- ProgrammableWeb: https://www.programmableweb.com/apis/directory
	- APIs Guru: https://apis.guru/
	- RapidAPI Hub: https://rapidapi.com/search/
