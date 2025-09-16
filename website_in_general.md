# Table of contents

- [The general of web](#the-general-of-web)

# How the web works

<img src="img/part1/download.webp">

1. The URL gets resolved

2. A Request is sent to the server of the website

3. The response of the server is parsed

4. The page is rendered and displayed

## 1. The URL gets resolved

When a user enters a domain (e.g., `example.com`) in a browser, the browser must resolve it to an `IP address` since the website’s source code is hosted on a server identified by its IP. A domain `example.com` and its optional path `example.com/learn` together form a `URL (Uniform Resource Locator)`.

The `translation` from `domain` to `IP address` is performed by `DNS (Domain Name System)` servers, which act as distributed directories `mapping domain names to IP addresses`. Browsers are preconfigured with DNS server addresses, allowing them to `send a query`, `retrieve the correct IP`, and then `send a request` to the corresponding web server.

Technically, `www` is just a `subdomain` (e.g., `www.example.com`
), though most websites redirect it to the root domain. IP addresses can be IPv4 (e.g., `172.56.180.5`) or `IPv6`.

## 2. Request is sent

Once the IP address is resolved, the browser sends an `HTTP request` to the server. A request is a structured data package containing the URL, request type (e.g., `GET, POST`), and optional metadata.

Requests are transmitted via the `HyperText Transfer Protocol (HTTP)` or its secure variant `HTTPS`, which defines the standardized format for communication between clients and servers.

<img src="img/part1/sendarequestwebp">

The server `processes the request` and `returns a response`, which also includes `data and metadata`. For web pages, the response typically contains `HTML code`, though it may also include other resources such as `files`, `images`, or `dynamically generated content` depending on how the server is programmed.

<img src="img/part1/response.webp">

## 3. Response is parsed

Once the server sends a response, the browser `parses` it according to the standardized `HTTP response format`. The response includes both `data and metadata` (e.g., Content-Type). Based on this, the browser determines how to handle the data:

- For `text/html`, the response body is interpreted as `HTML`, the markup language that defines the structure of a webpage.

- For other content types (e.g., `PDF`, `images`), the browser invokes the appropriate `rendering mechanism`.

HTML itself is not a programming language but a markup language that uses standardized tags to describe document structure and semantics.

<img src="img/part1/parsedResponse.webp">

## 4. Page is displayed

The browser processes the parsed HTML to construct the `Document Object Model (DOM)` and display the page. However, HTML only defines structure, not presentation.

<img src="img/part1/displayed1.webp">

`CSS (Cascading Style Sheets)` provides styling rules, typically loaded via separate .css files. The HTML response may contain <link> tags instructing the browser to fetch these resources, resulting in additional requests.

JavaScript adds interactivity and dynamic behavior by executing code directly in the browser, enabling features such as tabs, overlays, and live updates.

Thus, modern webpages are rendered through the `combination` of `HTML` (structure), `CSS` (style), and `JavaScript` (behavior), often requiring multiple request–response cycles beyond the initial HTML file.

<img src="img/part1/displayed2.webp">

## 5. Server-side vs Client-side

Web development is divided into two primary contexts:

### Server-side (Backend):

- Runs on `servers` (ordinary computers configured to serve requests).

- Uses server-side programming languages such as `Node.js`, `PHP`, or `Python`.

- Responsible for `handling requests`, `business logic`, `data storage`, and `generating responses`.

### Client-side (Frontend):

- Runs in the `browser` (the client environment).

- Relies on three mandatory technologies:

  - HTML → defines page structure.

  - CSS → defines presentation and styling.

  - JavaScript → enables interactivity and dynamic behavior.

- Unlike the server-side, these are not interchangeable; all three are required to build functional websites.

### HTTP Request - Response (Server and client communication):

<img src="img/part1/ImageOfHTTPRequestResponse-660x374.png">

#### HTTP Request:

<img src="img/part1/08715e70-5e65-480d-bcb1-274e62c7636c.webp">

##### **Request Line**

- Example: `GET /products/dvd.htm HTTP/1.1`
- Contains three parts:

  1. **Method** (`GET`) → Type of operation (e.g., `GET`, `POST`, `PUT`, `DELETE`).
  2. **Request Target / Path** (`/products/dvd.htm`) → The resource being requested.
  3. **HTTP Version** (`HTTP/1.1`) → Protocol version used.

##### **General Headers**

- Apply to both requests and responses, not resource-specific.
- Examples:

  - `Host: www.videoequip.com` → Specifies the domain of the target server.
  - `Cache-Control: no-cache` → Instructs intermediaries/browsers not to use cached content.
  - `Connection: Keep-Alive` → Keeps the TCP connection open for multiple requests.

##### **Request Headers**

- Provide additional information about the client or request.
- Examples:

  - `Content-Length: 133` → Size of the request body (in bytes).
  - `Accept-Language: en-us` → Preferred language(s) for the response.

##### **Entity Headers**

- Describe metadata about the body of the request.
- Examples:

  - `Content-Length: 133` (again, but specific to the body).
  - `Content-Language: en` → Language of the request body content.

##### **Body**

- Optional, contains the actual data being sent (e.g., form data in a `POST` request, JSON payload, file upload).
- Empty in simple `GET` requests.

#### HTTP Response:

<img src="img/part1/579962dd-5265-426d-8340-ccf52a3fdab8.webp">

##### **Status Line**

- Example: `HTTP/1.1 200 OK`
- Contains three parts:

  1. **HTTP Version** (`HTTP/1.1`).
  2. **Status Code** (`200`) → Numeric code indicating result (e.g., 200=OK, 404=Not Found, 500=Server Error).
  3. **Reason Phrase** (`OK`) → Human-readable description of the status code.

##### **Response Headers**

- Provide metadata about the server or response.
- Examples:

  - `Date: Sun, 08 Feb xxxx 01:11:12 GMT` → Timestamp of response.
  - `Server: Apache/1.3.29 (Win32)` → Web server software.
  - `Last-Modified: Sat, 07 Feb xxxx` → Last modification date of the resource.
  - `ETag: "0-23-4024c3a5"` → Unique identifier for caching validation.
  - `Accept-Ranges: bytes` → Supports partial content requests (range requests).
  - `Content-Length: 35` → Size of the response body in bytes.
  - `Connection: close` → Informs that the connection will be closed after this response.
  - `Content-Type: text/html` → Type of content returned (HTML in this case).

##### **Blank Line**

- Separates headers from the body.
- Mandatory in HTTP messages.

##### **Response Body**

- Contains the actual content requested by the client.
- Example in diagram:

  ```html
  <h1>My Home page</h1>
  ```

- Could also be JSON, XML, images, PDFs, etc., depending on `Content-Type

## 6. Beyond Websites – General Internet Communication

The request–response model underpins all internet communication, but not every request/response involves websites.

Content negotiation: Requests and responses include metadata (headers) that specify the type of data expected (e.g., HTML, JSON, PDF). If a server does not support the requested type, it cannot return that data.

`APIs (Application Programming Interfaces)`: Many servers expose endpoints designed to return structured data instead of full web pages.

Example: A mobile app sends `HTTP requests` to an `API` to `retrieve` or `store data` (e.g., Twitter fetching tweets).

Example: A web page might send AJAX or fetch() requests behind the scenes to update content dynamically without reloading the page (e.g., newsletter signup).

Thus, the internet supports both website delivery and data exchange services, all powered by the same underlying HTTP request–response mechanism.
