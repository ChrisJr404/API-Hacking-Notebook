# Reverse Engineering API Documentation
This section provides a systematic approach to reverse engineer an API when documentation or specification files are unavailable. It introduces manual and automatic methods to build your collection of API requests, aiding in the discovery of potential vulnerabilities and attack surfaces.

# Manual Request Collection with Postman
## **Creating a Workspace**
- Create a workspace in Postman to organize your collections. 

## **Using the Postman Proxy**
1. Click the `Capture Requests` button at the bottom right of Postman.
2. In the "Capture requests" window, enable the proxy, ensuring the port matches the FoxyProxy setup.
3. Add your target URL in the "URL must contain" field and initiate the request capture by clicking `Start Capture`.
4. Use FoxyProxy to proxy the web browser's traffic through Postman and navigate through the web application's entire functionality:
    - Registration, login, visiting profiles, posting comments, etc.
    - Make sure to interact thoroughly, as missing functionalities could lead to blind spots in the API's attack surface.

## **Building the API Collection**
After capturing the necessary data:
1. Stop the proxy in Postman.
2. Create a new collection named "API Proxy Collection".
3. Organize requests by endpoints, grouping similar requests, and renaming them for better clarity.

# Automatic Documentation with mitmproxy2swagger
Another efficient way of reverse engineering an API is by capturing web traffic with mitmweb and then using mitmproxy2swagger to generate an Open API 3.0 specification.

## **Setting Up mitmweb**
1. Start mitmweb to listen on port `8080`.
```
mitmweb
```

## **Capturing and Saving Requests**
1. Use FoxyProxy to proxy the web browser's traffic through mitmweb and navigate through the web application's entire functionality:
    - Registration, login, visiting profiles, posting comments, etc.
    - Make sure to interact thoroughly, as missing functionalities could lead to blind spots in the API's attack surface.
2. Save the session to a file named `flows` for further processing.

## **Generating the Open API Documentation**

1. Convert the `flows` file into an Open API 3.0 YAML documentation.
```
$sudo mitmproxy2swagger -i /Downloads/flows -o spec.yml -p http://api.example.com -f flow
```
2. Edit the `spec.yml` file to ensure no endpoints are overlooked and update the file's title for clarity.
	- Remove `ignore:` from endpoints you wish to include using a text editor.
	- Validate and correct formatting by running the `mitmproxy2swagger` script again, adding `--examples` flag for enriched documentation.
	```
	sudo mitmproxy2swagger -i /Downloads/flows -o spec.yml -p http://api.example.com -f flow --examples
	```

## **Validation and Importation into Postman**
1. Validate the YAML file using the Swagger Editor(https://editor.swagger.io/). You can also interact with the API and read the collected documentation on the site.
2. Import the validated YAML file into Postman as a collection for penetration testing purposes.