# Fullstack Interview Questions

- What is OAuth?
  - OAuth is an open authorization framework that allows a third-party application to obtain limited access to a user's account on another service without exposing credentials. Instead of sharing a password, the user grants permission and the third-party receives an access token scoped to specific resources (e.g., "Sign in with Google").

- What is a REST API?
  - REST (Representational State Transfer) is an architectural style for building APIs over HTTP. A REST API exposes resources via URLs and uses standard HTTP methods (GET, POST, PUT, DELETE) to perform operations on them. It is stateless, meaning each request must contain all the information needed to process it.

- What is the difference between frontend and backend?
  - The frontend is everything the user sees and interacts with in the browser — HTML, CSS, and JavaScript that render the UI. The backend runs on a server and handles business logic, database access, authentication, and exposes data to the frontend via APIs. Together they form a fullstack application.

- What are some recent trends in fullstack?
  - Recent trends include server-side rendering (SSR) and static site generation with frameworks like Next.js, edge computing that runs logic closer to the user, serverless/function-as-a-service architectures, the rise of TypeScript across the stack, and AI-assisted development tooling integrated into developer workflows.

- How can the load time of a website be optimized?
  - Key techniques include minifying and bundling JS/CSS, lazy-loading images and code-splitting, serving assets via a CDN, enabling browser caching and HTTP/2, using efficient image formats (WebP/AVIF), reducing the number of HTTP requests, and server-side rendering or pre-rendering critical pages so content is delivered faster.

- What is caching? On which levels can it be applied?
  - Caching stores the result of an expensive operation so future requests can be served faster. It can be applied at multiple levels: browser cache (HTTP cache-control headers), CDN/edge cache (caching static or dynamic responses closer to the user), application-level cache (in-memory stores like Redis), database query cache, and DNS cache.

- What are websockets?
  - WebSockets provide a persistent, full-duplex communication channel over a single TCP connection between client and server. Unlike HTTP, which is request-response, either side can push data at any time. They are ideal for real-time use cases like chat, live notifications, collaborative editors, and financial tickers.

- What is SSR?
  - Server-Side Rendering (SSR) means the HTML for a page is generated on the server on each request and sent to the browser, rather than being assembled in the browser from JavaScript. This improves initial page load time and SEO because the browser receives fully formed HTML immediately, without waiting for JS to execute.

- What are cookies?
  - Cookies are small key-value data strings that a server sends to the browser, which stores them and sends them back with every subsequent request to the same domain. They are commonly used for session management, authentication tokens, and user preferences. Important attributes include `HttpOnly` (not accessible via JS), `Secure` (HTTPS only), and `SameSite` (CSRF protection).

- What is JWT?
  - JSON Web Token (JWT) is a compact, self-contained token used for transmitting claims between parties. It consists of three Base64-encoded parts — header, payload, and signature — separated by dots. The server signs the token with a secret or private key; the client stores it and sends it with requests so the server can verify identity without querying a database.

- What is the difference between an SPA and a PWA?
  - A Single-Page Application (SPA) loads a single HTML page and dynamically updates content in the browser using JavaScript, avoiding full page reloads. A Progressive Web App (PWA) is a set of enhancements (service workers, web manifest, HTTPS) that make a web app installable and capable of working offline. An SPA can be a PWA, but they address different concerns.

- What are CDNs?
  - A Content Delivery Network (CDN) is a geographically distributed network of servers that caches and delivers static assets (images, JS, CSS, video) from a location close to the end user. This reduces latency, offloads traffic from the origin server, and improves availability and resilience.

- How would you ensure the scalability of a webapp?
  - Scalability strategies include horizontal scaling (adding more server instances behind a load balancer), using stateless services so any instance can handle any request, offloading heavy work to background job queues, caching frequently accessed data, using a CDN for static assets, optimizing database queries and adding read replicas, and designing services that can be deployed and scaled independently (microservices or serverless functions).

- What is CORS?
  - Cross-Origin Resource Sharing (CORS) is a browser security mechanism that restricts web pages from making HTTP requests to a different origin than the one that served the page. The server signals which origins are allowed by including `Access-Control-Allow-Origin` headers. Without correct CORS headers, the browser blocks the response. CORS applies only to browser-initiated requests, not server-to-server calls.

- What is callback hell?
  - Callback hell describes deeply nested callback functions that arise when handling multiple asynchronous operations sequentially in JavaScript. Each async step is placed inside the previous step's callback, resulting in code that is hard to read, debug, and maintain. It is commonly resolved by using Promises (`.then()` chaining) or the `async/await` syntax.

- How can you prevent a bot from scraping your API?
  - Common countermeasures include rate limiting and throttling requests per IP or API key, requiring authentication tokens (API keys, OAuth) so anonymous access is blocked, using CAPTCHAs on sensitive endpoints, detecting anomalous request patterns with WAF rules, returning honeypot fields to identify scrapers, and employing bot-detection services (e.g., Cloudflare Bot Management).

- What is the difference between MVC and MVP?
  - In MVC (Model-View-Controller), the Controller handles user input, updates the Model, and selects the View; the View can also directly query the Model. In MVP (Model-View-Presenter), the Presenter sits between Model and View and all communication goes through it — the View is passive and only displays what the Presenter tells it. MVP makes the View easier to unit test because it has no direct dependency on the Model.
