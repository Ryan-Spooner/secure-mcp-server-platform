# Cloudflare's Ecosystem for Model Context Protocol (MCP) Servers: Products, Services, and Strategic Implications

## 1. Executive Summary

Cloudflare is strategically positioning itself as a pivotal enabler for the adoption and scaling of the Model Context Protocol (MCP), providing a comprehensive suite of tools and infrastructure. The company's approach is centered on simplifying the development lifecycle of MCP servers, ensuring robust security for AI-driven interactions, and leveraging its extensive global network for optimal performance.1 This strategy offers significant benefits to developers and organizations, including accelerated development of AI agent capabilities, fortified security for sensitive AI interactions, and highly scalable deployment mechanisms for MCP servers. These offerings empower entities to integrate AI more deeply and effectively with their existing services and Cloudflare's diverse platform capabilities.2

The implications of Cloudflare's initiatives extend beyond mere facilitation. By providing tools that abstract the inherent complexities of MCP and by championing the deployment of remote MCP servers, Cloudflare is effectively democratizing access to advanced AI agent integration. Historically, MCP server implementations were often confined to local environments, creating a barrier to broader adoption and more sophisticated use cases.3 Cloudflare's emphasis on remote servers, built using Cloudflare Workers and supported by tools like McpAgent and workers-oauth-provider, significantly lowers this barrier.7 This shift empowers a wider array of developers and organizations to construct and deploy powerful AI agents capable of interacting with real-world services, moving far beyond the capabilities of simple chatbots and fostering a new wave of AI-driven automation and interaction.

Furthermore, security is not merely an add-on but a foundational pillar of Cloudflare's MCP strategy. The inherent risks associated with AI agents interacting with sensitive data and critical systems are considerable.1 Cloudflare proactively addresses these challenges through a multi-layered security approach. The workers-oauth-provider library enables robust OAuth 2.1-based authorization for MCP servers, ensuring that agents act only with appropriate permissions.6 Cloudflare AI Gateway offers critical observability into AI interactions and incorporates "Guardrails" for content moderation, preventing the propagation of harmful or inappropriate content.12 Complementing this, Firewall for AI is designed to protect against Large Language Model (LLM)-specific attacks and prevent the leakage of Personally Identifiable Information (PII).1 This comprehensive suite of security tools, presented as integral components of the AI and MCP offerings, underscores Cloudflare's commitment to building a trustworthy ecosystem for the adoption and scaling of MCP-enabled AI agents.

## 2. Introduction to Model Context Protocol (MCP) and Cloudflare's Vision

The Model Context Protocol (MCP) is an emerging open standard designed to facilitate secure and structured interaction between AI agents or assistants—such as Anthropic's Claude or the Cursor IDE—and a diverse range of external tools, services, and APIs.10 Its core significance lies in its ability to empower AI systems to move beyond simple inference or Retrieval Augmented Generation (RAG). MCP provides a standardized communication layer that enables AI agents to discover available tools and take concrete actions in the digital or physical world, such as sending emails, deploying software updates, managing complex projects, or interacting with IoT devices.6 This allows AI to dynamically determine how to accomplish a given task by intelligently composing and invoking available tools, rather than merely following a predefined, hardcoded sequence of steps.10

Cloudflare's strategic approach to MCP is to establish its platform as the premier environment for building, deploying, and securing remote MCP servers. This vision aims to significantly accelerate the development and mainstream adoption of sophisticated AI agents.3 The company is focused on abstracting the complexities inherent in the MCP standard, including protocol handling, robust authentication and authorization mechanisms, secure data transport, and efficient state management.2 A key element of this strategy is Cloudflare's active effort to transition the MCP paradigm from predominantly local-only server setups to globally accessible, remote server deployments. This shift is crucial for unlocking the full potential of MCP and enabling broader usability across diverse applications and user environments.3

This focus on infrastructure, security, and developer tooling for MCP positions Cloudflare as a key enabler in the burgeoning field of AI agents, akin to providing the essential "picks and shovels" during a gold rush. The MCP standard allows AI agents to perform a wide array of actions and interact with diverse tools, signaling a new frontier for AI applications.6 However, building and securing these MCP servers independently can be a complex undertaking, involving intricate aspects of OAuth, transport protocols, and state persistence.2 Cloudflare addresses this by offering an "end-to-end toolkit" 3, which includes the McpAgent class, the workers-oauth-provider library, deployment templates, and foundational infrastructure like Cloudflare Workers and Durable Objects.4 This comprehensive support allows companies such as Asana, Atlassian, and Stripe to concentrate on their core services and innovate on how AI can leverage these services, rather than expending resources on the underlying MCP plumbing.2 By providing these fundamental building blocks, Cloudflare empowers a broad ecosystem of businesses to develop value-added AI services, effectively fueling the growth of AI agent capabilities.

Moreover, the concerted push towards remote MCP servers represents a critical evolutionary step for AI assistants to become truly ubiquitous. Traditional local MCP server deployments inherently limit the reach and utility of AI agents, often tying them to individual machines or isolated environments.3 Cloudflare is pioneering the widespread adoption of remote MCP servers, being the "first platform to support remote MCP server deployment" and streamlining the process to a "one-click" experience for certain configurations.16 These remote servers, typically built on Cloudflare Workers, enable AI agents to function seamlessly and consistently across web browsers, mobile applications, and shared collaborative environments, entirely obviating the need for local setup by the end-user.3 This enhanced accessibility is fundamental for AI assistants like Claude to interact with enterprise-grade applications (such as those from Stripe, PayPal, and Asana) on behalf of users, irrespective of the user's device or physical location.2 Consequently, Cloudflare's remote MCP initiative is not merely a technical enhancement but a foundational enabler for the broader vision of AI agents becoming pervasively and deeply integrated into our daily digital interactions and workflows.

## 3. Building and Deploying MCP Servers on Cloudflare

Cloudflare provides a robust set of tools, frameworks, and deployment models designed to streamline the creation and operation of MCP servers on its global network. These offerings cater to various needs, from simple, stateless servers to complex, stateful AI agents requiring persistent context and secure authentication.

### 3.1. Core Development Tools & Frameworks

At the heart of MCP server development on Cloudflare are specialized SDKs and packages that abstract protocol complexities and integrate seamlessly with the Cloudflare Workers serverless environment.

- **Cloudflare Agents SDK and the McpAgent Class**:  
    The McpAgent class, a key component of the Cloudflare Agents SDK, serves as the primary building block for constructing MCP servers on the Cloudflare platform.8 Developers extend this class to create specialized server instances.  
    
    Its core functionalities include sophisticated state management, allowing MCP servers to remember previous tool invocations, user interactions, and cache the state of external API calls. This state is durably persisted using Cloudflare Durable Objects, which can leverage their own integrated SQL databases for structured data storage.8  
    A significant advantage of McpAgent is its automatic handling of remote transport configuration. It natively supports both Server-Sent Events (SSE) and the newer Streamable HTTP transport, ensuring broad client compatibility and future-proofing server implementations.17 
     
    Furthermore, McpAgent instances benefit from WebSockets Hibernation support. This innovative feature allows stateful MCP servers to enter an inactive state during periods of no client activity, thereby conserving compute resources. Crucially, hibernation preserves the server's complete state, including conversational context and history, enabling seamless resumption of interactions.8 This makes long-lived, stateful agent sessions economically viable.  
    
    For tool integration, McpAgent allows developers to easily define and add tools (functions exposed to AI clients) using standard libraries like the `@modelcontextprotocol/typescript-sdk` package.8
    
- The workers-mcp Package:  
    The workers-mcp package offers Command Line Interface (CLI) tooling and in-Worker logic designed to connect MCP clients, particularly local clients like Claude Desktop, to a Cloudflare Worker functioning as an MCP server.19 Its primary mechanism involves a build step that translates TypeScript methods defined within the Cloudflare Worker into discoverable MCP tools. A Node.js server, typically run on the client's local machine, then acts as a proxy. This local proxy handles the stdio transport expected by some clients and translates these interactions into remote calls to the appropriate methods of the Cloudflare Worker.19  
    
    The principal use case for workers-mcp is to expose functions within a Cloudflare Worker, APIs hosted on Workers, or other Cloudflare platform services (like R2, KV, D1) to LLMs embedded in coding agents or other MCP clients.19 The package's documentation suggests it can be employed to build a remote MCP server, and it often directs users to the more general guides for remote MCP server development.19  
    When comparing workers-mcp with the McpAgent approach, it appears that McpAgent (within the broader Agents SDK) represents a more current and comprehensive solution for building robust, feature-rich remote MCP servers directly on Cloudflare. McpAgent offers integrated state management, hibernation, and direct handling of modern remote transports.8 workers-mcp, while valuable, might be better suited for specific scenarios like proxying for older local clients or as an earlier, perhaps simpler, method for exposing Worker functions. Some documentation for workers-mcp also notes that remote MCP servers were "not supported yet" in the context of that specific guide, suggesting it might predate the more advanced capabilities of the Agents SDK.13 However, 13 also highlights workers-mcp for its ability to rapidly turn existing APIs into MCP servers with minimal setup, managing tool discovery, protocol handling, and request routing.
    
- Templates and CLI (Wrangler) for Streamlined Deployment:  
    To accelerate development, Cloudflare provides official templates for creating MCP servers. These templates cater to common patterns, such as servers with no authentication (e.g., cloudflare/ai/demos/remote-mcp-authless) and servers requiring authentication, often exemplified with GitHub as an OAuth provider (e.g., cloudflare/ai/demos/remote-mcp-github-oauth).7  
    Deployment of these MCP servers to the Cloudflare network is facilitated either through a "Deploy to Workers" button for projects hosted on GitHub or via Cloudflare's command-line interface, Wrangler, using commands like npx wrangler@latest deploy.7
    

### 3.2. Deployment Models

Cloudflare's platform is optimized for deploying remote MCP servers, which are accessible over the internet, contrasting with earlier local-only models.

- Building Remote MCP Servers:  
    Cloudflare strongly advocates for and provides extensive support for the deployment of remote MCP servers, which is seen as crucial for moving beyond the limitations of local-only instances and enabling wider AI agent integration.6
    

- Without Authentication: This model allows any MCP client to connect and utilize the server's tools without requiring user login. The setup typically involves using a predefined template and deploying the Worker project via Wrangler or integrated Git workflows.7
    
- With Authentication: This more secure model requires users to authenticate before they can access the MCP server's tools. This enables fine-grained control over tool access based on user identity and permissions. A common example involves using GitHub as an external OAuth provider. Setting this up requires creating GitHub OAuth applications for both local development and production environments and securely configuring the client ID and client secret as environment variables or secrets within the Cloudflare Worker.7
    

- Python Support for MCP Server Development:  
    Recognizing the popularity of Python in the AI/ML ecosystem, Cloudflare has extended its MCP server development capabilities to include Python, in addition to JavaScript and TypeScript.2 Developers can now build and deploy remote MCP servers written entirely in Python, leveraging Python Workers. The Python MCP SDK is provided for defining tools within these servers. For developers already using FastAPI, a popular Python web framework, the FastAPI-MCP library offers a convenient way to expose existing FastAPI endpoints as MCP tools with minimal code changes.18
    

### 3.3. Local Development and Testing

Effective local development and testing workflows are essential for building reliable MCP servers.

- MCP Inspector:  
    The @modelcontextprotocol/inspector package provides a visual testing tool specifically designed for MCP servers.20 Developers can run the inspector locally using npx @modelcontextprotocol/inspector. It allows connection to both locally running MCP servers (e.g., a Worker running via wrangler dev) and remote MCP servers deployed on Cloudflare. Once connected (and authenticated, if required), the inspector can list the tools exposed by the server, facilitating verification and debugging.20
    
- Local Server Execution:  
    Most Cloudflare Worker projects, including those for MCP servers, can be run locally for development and testing. Commands like npm start or wrangler dev typically start a local development server, often accessible at an address like http://localhost:8787/sse.7
    
- The mcp-remote Adapter:  
    The mcp-remote adapter is a critical utility that bridges the gap between older MCP clients designed for local (stdio) connections and modern remote MCP servers that use SSE or Streamable HTTP.3 Many existing MCP clients, such as earlier versions of Claude Desktop, Cursor, or Windsurf, may only support local stdio-based communication. mcp-remote acts as a local proxy: the client is configured to run mcp-remote with the URL of the remote Cloudflare-hosted MCP server. mcp-remote then handles the stdio communication locally and translates it to the appropriate remote protocol (SSE or Streamable HTTP) to interact with the server.20 This tool ensures that users of these clients can still benefit from the growing ecosystem of remote MCP servers built on Cloudflare.
    

The combination of McpAgent and Durable Objects represents a strategic direction for Cloudflare, aimed at enabling sophisticated, stateful AI agents. While stateless MCP servers have their utility, the capacity to maintain conversational context, remember user preferences, and cache external API states—all facilitated by McpAgent's integration with Durable Objects 8—unlocks far more complex, intelligent, and personalized agent behaviors. The addition of hibernation for these stateful McpAgent instances further enhances this model by making potentially long-lived agent sessions economically viable, as compute resources are only consumed when the agent is active.8 This focus indicates that Cloudflare is not merely supporting basic MCP tool invocation but is actively fostering the development of advanced AI agents capable of richer, more continuous interactions.

The mcp-remote adapter plays a crucial, albeit transitional, role in the ecosystem's evolution. As the MCP landscape shifts from predominantly local to increasingly remote server architectures, mcp-remote ensures backward compatibility and facilitates wider adoption.17 It prevents existing local-only clients from becoming obsolete overnight and allows their users to immediately leverage the benefits of Cloudflare's remote MCP server offerings. This smooths the transition for the entire ecosystem, accelerates the uptake of remote MCP servers, and helps prevent fragmentation between older and newer client-server interaction models.

Observing the trajectory from tools like workers-mcp to the more integrated McpAgent class within the broader Agents SDK likely reflects a maturation in Cloudflare's MCP strategy. workers-mcp primarily focuses on translating TypeScript methods into MCP tools and often relies on a local Node.js proxy for communication between a local client and a remote Worker.19 In contrast, McpAgent offers a more holistic and powerful framework for building full-fledged remote MCP servers directly on Cloudflare's infrastructure. It provides built-in state management via Durable Objects, automatic hibernation, and native handling of remote transport protocols like SSE and Streamable HTTP from within the Worker itself.8 The Agents SDK, as a whole, represents a broader Cloudflare initiative for building various types of AI agents.1 This progression suggests a strategic shift from enabling basic connectivity (workers-mcp) to providing a comprehensive, feature-rich server development framework (McpAgent) as Cloudflare deepens its investment and refines its offerings in the AI and agent technology space.

## 4. Key Components of Cloudflare's MCP Implementation

Cloudflare's implementation of MCP is characterized by robust authorization mechanisms, flexible transport protocols, and a clear methodology for defining and utilizing tools within MCP servers. These components work in concert to provide a secure, efficient, and developer-friendly environment.

### 4.1. Authorization and Authentication

Security is paramount in MCP, as AI agents are often granted permissions to act on behalf of users or access sensitive data. Cloudflare addresses this through standardized protocols and dedicated libraries.

- **OAuth 2.1 for MCP**:  
    The Model Context Protocol leverages a subset of the OAuth 2.1 specification for authorization.9 This industry-standard framework allows users to grant limited access to their resources (e.g., data, services) to MCP clients (and thus, the AI agents they power) without needing to share their primary credentials like API keys or passwords. This principle of delegated authority is crucial for enabling AI agents to securely interact with various services on a user's behalf.
    
- **workers-oauth-provider Library**:  
    Cloudflare provides the workers-oauth-provider, a TypeScript library specifically designed for Cloudflare Workers, which implements the provider (server) side of the OAuth 2.1 protocol, including support for Proof Key for Code Exchange (PKCE) for enhanced security.9 As of March 2025, this library was noted as being in beta, with its API subject to potential changes.21  
    
    Key features of this library include acting as a protective wrapper around Worker code to add authorization layers to API endpoints, automatically managing the complexities of token issuance and validation, and remaining agnostic to the specific user management systems or UI frameworks employed by the application.21 A notable security feature is its storage mechanism: secrets like access tokens and client secrets are stored only as hashes, and any application-specific properties (props) associated with a grant are encrypted using the token itself as key material, making them indecipherable from storage alone.21  
    
    In terms of usage, an instance of the OAuthProvider class typically becomes the main entry point for the Cloudflare Worker, intercepting all incoming HTTP requests. It is configured with routes that are considered API endpoints; for these routes, it checks for valid access tokens before passing the request to the designated API handler. The library also implements standard OAuth endpoints, such as the token endpoint (/token) for exchanging authorization codes for access tokens, and can be configured with an authorization endpoint (/authorize), though the user interface for the authorization consent flow itself must be implemented by the application developer.21  
    The workers-oauth-provider library offers flexible integration options 9:
    

1. **MCP Server handles authorization and authentication itself**: The Cloudflare Worker, utilizing the workers-oauth-provider library, manages the entire OAuth 2.1 flow without external dependencies for the OAuth process itself, though it can still rely on an external service for user authentication (login).
    
2. **Integrate directly with a third-party OAuth provider**: The library can be configured to work with established third-party OAuth providers like GitHub or Google. In this model, the user authenticates with the third-party provider, which then issues an authorization code back to the MCP server. The MCP server (your Worker) exchanges this for a third-party access token and subsequently issues its own, potentially more restricted, MCP-specific token to the MCP client.
    
3. **Bring your own OAuth Provider**: If an organization already has an existing OAuth provider or uses an authorization-as-a-service platform (e.g., Stytch, Auth0, WorkOS), this can be integrated in a manner similar to third-party providers. This allows leveraging established authentication mechanisms (like email/password, social logins, SSO, MFA) and mapping existing scopes and permissions to the tools exposed by the MCP server. Cloudflare provides examples and documentation for integrating with services like Stytch, Auth0, and WorkOS, detailing how they handle user identity, consent flows, and the mapping of permissions to MCP tools, ensuring that agents operate within defined boundaries.9
    

- **Implementing Permission-Based Access for MCP Tools**:  
    A critical aspect of secure MCP server design is enforcing fine-grained access control for the tools it exposes. When a user authenticates via the workers-oauth-provider (or an integrated system), their identity information and any associated tokens or claims become available within the MCP server's execution context (e.g., through the props parameter when using McpAgent in conjunction with the OAuth provider library).9  
    This authentication context can then be used to implement permission checks before a tool's handler function is executed. This allows developers to define logic that verifies whether the authenticated user (or the agent acting on their behalf) has the necessary roles or specific permissions required to invoke a particular tool.9 This ensures that AI agents can only access and utilize tools for which they have explicit authorization, preventing unauthorized actions and data access.
    

### 4.2. Transport Mechanisms

MCP defines several transport mechanisms for communication between clients and servers. Cloudflare's platform is designed to support the most relevant methods for remote interactions.

- **Overview of stdio, Server-Sent Events (SSE), and Streamable HTTP 17**:
    

- **stdio** (Standard Input/Output): This mechanism is primarily designed for local MCP connections, where the client and server are running on the same machine and communicate via standard input and output streams.
    
- **Server-Sent Events** (SSE): SSE has been the predominant transport for remote MCP client-server communication. However, it typically requires two separate HTTP endpoints: one for the client to send requests to the server, and another (an event stream) for the server to send responses and updates back to the client. While widely supported, it is gradually expected to be superseded by Streamable HTTP.
    
- **Streamable HTTP**: Introduced to the MCP specification in March 2025, Streamable HTTP aims to simplify remote communication by using a single HTTP endpoint for bidirectional messaging between the client and server. It is gaining adoption and is anticipated to become the standard transport in the future. A key advantage of Streamable HTTP is its potential for enhanced features like resumability, allowing clients to seamlessly continue long-running operations if a connection drops and reconnects, without losing responses.18
    

- **Cloudflare's Support and Recommendations**:  
    MCP servers built using the Cloudflare Agents SDK, particularly those extending the McpAgent class, are designed to support both primary remote transport methods—SSE and Streamable HTTP—concurrently.17 This dual support ensures maximum compatibility with existing MCP clients that may only support SSE, while also embracing the newer, more efficient Streamable HTTP standard for future-facing applications.  
    The McpAgent class automatically handles much of the transport configuration complexity.17 Cloudflare provides clear guidance and code examples on how to configure an McpAgent-based server to listen on distinct paths for each transport, typically /sse for Server-Sent Events and /mcp for Streamable HTTP. This includes configurations for MCP servers that also implement authentication using the workers-oauth-provider library, ensuring that secure transport and authentication work seamlessly together.17
    

### 4.3. Defining and Using Tools within MCP Servers

The core functionality of an MCP server is to expose a set of "tools" that AI agents can discover and invoke.

- **How Tools Are Defined and Exposed**:  
    Developers define these tools as functions within their MCP server code. These functions encapsulate specific actions or capabilities that the AI agent can utilize. Packages such as `@cloudflare/model-context-protocol` (provided by Cloudflare) or the more general `@modelcontextprotocol/typescript-sdk` are used to structure these tool definitions.8 When using the workers-mcp package, JSDoc annotations in TypeScript code can be used to automatically register class methods as discoverable MCP tools.13 The MCP server then makes metadata about these tools (name, description, input parameters, output format) available to connected MCP clients, enabling dynamic tool discovery and invocation.
    
- **Interaction with Cloudflare Services**:  
    A powerful application of MCP servers on Cloudflare is their ability to expose tools that interact directly with various Cloudflare platform services. Developers can build MCP servers that provide AI agents with capabilities to manage and query Cloudflare resources such as Workers (deployment, configuration), R2 object storage, KV namespaces, D1 serverless databases, DNS records, Web Application Firewall (WAF) rules, analytics data, and load balancing configurations.14 This allows users to interact with their Cloudflare infrastructure using natural language prompts directed at an AI agent, which then translates these requests into specific tool calls to the appropriate Cloudflare-integrated MCP server.
    

The workers-oauth-provider library is a linchpin for enabling secure, multi-tenant MCP deployments on the Cloudflare platform. By adeptly managing the complexities of the OAuth 2.1 protocol and offering versatile integration pathways with various identity systems, it empowers developers to construct MCP servers capable of securely serving numerous users and organizations, each with distinct and enforceable permission sets. MCP servers, by their nature, expose tools that can execute actions with potentially significant impact on behalf of users.9 Consequently, the imperative to securely authenticate users and meticulously authorize agent actions cannot be overstated. OAuth 2.1 stands as the accepted industry standard for this form of delegated authority 9, and Cloudflare's workers-oauth-provider effectively implements this standard within the Cloudflare Workers environment.21 Its inherent flexibility, allowing integration with existing Identity Providers (IdPs) such as GitHub, Google, or enterprise-focused solutions like Stytch, Auth0, and WorkOS 9, means that organizations are not compelled to re-engineer their established identity management infrastructures to accommodate AI agents. This facilitates the implementation of granular, role-based access control 9, an essential prerequisite for widespread enterprise adoption and for MCP servers that might provide access to shared, sensitive, or regulated resources. Therefore, this library is instrumental in fostering the development of trustworthy and enterprise-ready MCP solutions on Cloudflare.

Cloudflare's proactive embrace and promotion of Streamable HTTP underscore a commitment to future-proofing the Model Context Protocol and enhancing the developer experience. While Server-Sent Events (SSE) is the currently prevalent transport for remote MCP, it possesses certain limitations, such as the typical requirement for two distinct endpoints.17 Streamable HTTP, being a newer addition to the specification, offers a simpler, single-endpoint model and introduces potential benefits like connection resumability, which is particularly valuable for long-running agent tasks.17 Cloudflare's McpAgent is designed to support both SSE and Streamable HTTP concurrently, ensuring backward compatibility while paving the way for the adoption of the improved standard.17 The provision of "Deploy to Cloudflare" buttons for server templates that support both transport methods further encourages this transition.17 This forward-looking stance indicates that Cloudflare is not merely implementing MCP as it currently exists but is actively contributing to its evolution, usability, and long-term viability.

The capability for MCP tools to interact directly with Cloudflare's own extensive suite of platform services creates a potent and transformative feedback loop for AI-driven infrastructure management. This effectively turns the Cloudflare platform itself into an AI-manageable entity, holding the potential to revolutionize traditional DevOps practices and infrastructure operations. Cloudflare offers a multitude of critical infrastructure services, including DNS management, Web Application Firewall, serverless compute (Workers), object storage (R2), and more.14 Concurrently, Cloudflare provides a growing array of public MCP servers that expose tools specifically designed to manage these very services.14 This means that an AI agent, whether it's a general-purpose assistant like Claude or a custom-developed script, can be instructed using natural language to perform complex infrastructure tasks such as "deploy this new Worker function," "list all DNS records for example.com," or "configure a WAF rule to block traffic from a specific region".14 This paradigm shifts infrastructure management beyond manual dashboard interactions or intricate scripting towards AI-assisted, or even fully AI-automated, operations. This unique capability serves as a significant differentiator for Cloudflare and vividly showcases a future where managing complex cloud infrastructure could become a conversational or agent-driven process, dramatically increasing efficiency and accessibility.

## 5. Cloudflare's Publicly Available MCP Servers

Cloudflare not only provides tools for developers to build their own MCP servers but also offers a growing collection of publicly available remote MCP servers. These servers are designed to allow MCP clients—such as Anthropic's Claude, the Cloudflare AI Playground, or custom applications—to interact directly with various Cloudflare services and data sources using AI agents.2 This initiative enables users to manage their Cloudflare resources, retrieve analytics, debug applications, gain internet insights, and perform a host of other platform-specific tasks, all through natural language interactions mediated by an AI agent.25

These publicly accessible MCP servers represent a direct service offering from Cloudflare, leveraging the MCP standard to enhance user interaction with its platform. 

**Use Cases and Connection Methods:**

The use cases for these public MCP servers are diverse, spanning obtaining current Cloudflare documentation, managing various aspects of Cloudflare Workers (bindings, build processes), debugging applications through observability logs, accessing internet-wide insights via Radar, utilizing isolated development environments with the Container server, performing browser actions like web page rendering and screenshotting, analyzing Logpush job health, searching AI Gateway logs, interacting with AutoRAG document stores, querying audit logs for compliance, optimizing DNS through analytics, gaining insights from Digital Experience Monitoring (DEM), and performing security checks with the Cloudflare One CASB server.14

Connection to these servers can be established from any MCP client that supports remote server connections. If the client offers first-class support for remote MCP servers, such as the Cloudflare AI Playground 25, the user typically enters the server's URL directly into the client's interface. For clients that primarily support local connections, the mcp-remote adapter can be configured within the client's settings to proxy communication to the remote Cloudflare MCP server URL.25 For instance, users of claude.ai can integrate these servers by navigating to their settings, adding a new "Integration" by providing the server URL, and then authenticating with their Cloudflare account to authorize access.27

The provision of these public MCP servers serves as a practical demonstration and an instance of Cloudflare "dogfooding" its own MCP technology. By building, maintaining, and offering these servers, Cloudflare not only delivers tangible value to its users but also showcases the capabilities, robustness, and utility of its MCP development tools and underlying infrastructure (like Workers, McpAgent, and workers-oauth-provider). This approach builds credibility within the developer community, drives adoption of MCP principles, and provides invaluable real-world feedback that can be channeled into improving Cloudflare's developer offerings. It effectively serves as a large-scale, ongoing test case and a testament to the viability of their MCP platform strategy.

Furthermore, these public MCP servers are instrumental in transforming the Cloudflare platform into an AI-native environment. Users are no longer restricted to traditional interaction methods like dashboards or direct REST API calls; they can now interact with and manage their Cloudflare services through conversational AI agents. This significantly lowers the barrier to entry for performing complex tasks and unlocks new paradigms for automation and operational workflows. Instead of navigating multiple UI screens or writing intricate scripts, a user can instruct an AI agent like Claude to "deploy my latest Worker code" or "show me the DNS analytics for my primary domain," and the agent, by leveraging these MCP servers, can perform the requested actions or retrieve the necessary data.14 This fundamental shift in user experience makes Cloudflare's powerful, yet sometimes complex, suite of services more accessible, manageable, and automatable through the intuitive medium of natural language and the intelligent capabilities of AI-driven agents.

## 6. Complimentary Services for MCP Servers

Beyond the core tools for building and deploying MCP servers, Cloudflare offers a wide array of complimentary services that enhance their security, operational robustness, and performance. These services are integral to creating a comprehensive and resilient environment for AI agent interactions.

### 6.1. Security Services

Cloudflare's security offerings are multi-faceted, addressing various threats and vulnerabilities pertinent to AI applications and MCP servers.

- **Cloudflare for AI Suite: Overall Security Strategy:**  
    The "Cloudflare for AI" suite represents a holistic strategy to provide comprehensive visibility, robust security, and granular control for all types of AI applications, including those leveraging MCP servers.1 This initiative underscores Cloudflare's commitment to enabling businesses to adopt, deploy, and secure AI technologies with confidence, ensuring that security is a foundational element from the outset.
    
- **AI Gateway:**  
    Cloudflare AI Gateway acts as an intelligent proxy, providing deep observability into AI application usage patterns, including metrics like cost, latency, and the content of prompts and responses. Critically, it enhances security through its "Guardrails" feature.1  
    Guardrails with Llama Guard: This feature integrates Meta's Llama Guard 3 8B model, hosted on Cloudflare Workers AI for low-latency inference, to offer sophisticated content moderation capabilities. Guardrails can inspect both user prompts sent to AI models (potentially via an MCP server) and the responses generated by these models. It can detect and then flag or block harmful, inappropriate, or sensitive content based on customizable hazard categories.12 This is highly relevant for MCP servers that broker interactions with LLMs, ensuring a layer of safety and compliance.
    
- **Firewall for AI:**  
    Firewall for AI is seamlessly integrated with Cloudflare's existing Web Application Firewall (WAF). It is designed to automatically discover and protect LLM-powered applications, including those accessed through MCP servers, from various forms of abuse and data leakage.1 Key capabilities include the detection of Personally Identifiable Information (PII) in prompts or responses, and protection against common LLM-specific attacks such as prompt injection. Being model-agnostic and operating as a reverse proxy, Firewall for AI allows security teams to define custom rules for enforcement. For example, rules can be created to block requests containing certain types of PII or to rate-limit or block excessively long prompts based on token counting, which can help manage costs and prevent abuse.11
    
- **DDoS Protection for MCP Server Infrastructure:**  
    MCP servers deployed on Cloudflare Workers inherently benefit from Cloudflare's industry-leading DDoS protection services. This infrastructure-level security automatically mitigates most distributed denial-of-service attacks within seconds, ensuring the high availability and resilience of the MCP servers against volumetric and sophisticated attacks.34 This is a crucial baseline protection for any internet-facing service.
    
- **Leveraging Cloudflare Zero Trust Principles for Secure Access:**  
    While specific documentation on applying Cloudflare Zero Trust services directly to MCP server access patterns was not extensively detailed in the provided materials 22, the underlying principles are highly relevant. Cloudflare's Zero Trust Network Access (ZTNA) solutions can be employed to ensure that only authorized employees or systems can access internal AI applications or the management interfaces related to MCP servers.1 More directly, the robust authentication and authorization mechanisms provided by the workers-oauth-provider library and its integrations with established Identity Providers (like Stytch, Auth0, and WorkOS) align closely with Zero Trust tenets. These tools enforce strong authentication for users and enable granular, auditable permissions for AI agents accessing MCP tools, ensuring that access is explicitly verified and granted on a least-privilege basis.9
    

### 6.2. Deployment and Operational Resources

Cloudflare provides foundational infrastructure and storage solutions that are essential for deploying and operating scalable and efficient MCP servers.

- **Cloudflare Workers:**  
    This is the serverless compute environment where MCP servers are typically deployed. Workers run on Cloudflare's extensive global network, enabling code execution close to end-users, which minimizes latency and enhances performance. The serverless architecture also provides automatic scalability, adjusting resources based on demand without manual intervention.4
    
- **Durable Objects:**  
    Durable Objects offer a unique solution for adding stateful capabilities to serverless applications like MCP servers and the AI agents they support. They provide strongly consistent storage and coordination primitives, enabling the persistence of conversational context, user session data, long-term memory for agents, and caching of frequently accessed information.4 The recent introduction of a free tier for Durable Objects significantly democratizes access to these powerful stateful features, encouraging more developers to build sophisticated, context-aware AI agents.4
    
- **Cloudflare R2, KV, and D1:**  
    These storage solutions cater to different data needs of MCP applications:
    

- **R2**: An S3-compatible object storage service, ideal for storing large datasets such as AI training data, model artifacts, or any substantial binary data that MCP tools might need to access or generate. A key advantage is the absence of data egress fees, which can be a significant cost factor with other cloud storage providers.1
    
- **KV**: A globally distributed key-value store designed for high-availability, low-latency read access to small pieces of data, such as configuration settings, feature flags, or small, frequently accessed state information for MCP tools.25
    
- **D1**: A serverless SQL database built on SQLite, providing relational data storage capabilities for MCP applications that require structured data management at the edge.25
    

- **Observability Tools for Monitoring and Debugging:**  
    Effective monitoring and debugging are critical for maintaining the health and performance of MCP servers. Cloudflare Workers Logs, which can be accessed and analyzed using tools like the public Workers Observability MCP server, allow developers to browse invocation logs, identify errors, and compute statistics across their MCP server deployments running on Workers.27 Additionally, Cloudflare AI Gateway provides its own detailed logs specifically for AI interactions, offering insights into prompts, responses, and the actions taken by security features like Guardrails.12
    

The tight integration of compute (Workers), state (Durable Objects), advanced security (AI Gateway, Firewall for AI, OAuth via workers-oauth-provider), and comprehensive observability tools effectively creates a holistic "Secure AI Enclave" on Cloudflare. MCP servers operate as a key component within this enclave. This is not merely a collection of disparate services but an interconnected environment specifically tailored to support the unique operational and security demands of AI agents. For instance, AI Gateway leverages Workers AI to run Llama Guard for its Guardrails feature, and Firewall for AI integrates deeply with the broader Cloudflare WAF capabilities.11 This synergy forms a specialized and protected ecosystem where AI agents, communicating via MCP servers, can perform their tasks with enhanced security, granular control, and clear visibility.

The "Guardrails" feature within AI Gateway marks a significant advancement towards promoting responsible AI by operationalizing content safety directly at the infrastructure level. AI models, by their nature, can sometimes produce harmful, biased, or inappropriate content.12 Different models also come with varying degrees of built-in safety features, leading to inconsistency. AI Gateway's Guardrails, powered by Llama Guard, addresses this by providing a uniform and model-agnostic moderation layer that can be applied to both user prompts and model responses.12 This consistent safety net can be applied to any text-based AI model accessed through AI Gateway, which would naturally include models exposed via MCP servers that are routed through it. By offering this infrastructure-level safety mechanism, Cloudflare simplifies the implementation of responsible AI practices for developers, enabling them to build AI applications, including those using MCP, with greater confidence in their safety and compliance.

Furthermore, the strategic decision to offer a free tier for Durable Objects significantly lowers the barrier to entry for developing sophisticated, stateful MCP agents.4 Stateful interactions are fundamental for enabling more advanced AI capabilities, such as maintaining long-term memory, learning from past interactions, and providing highly personalized experiences.8 Durable Objects are Cloudflare's primary mechanism for providing this statefulness in a serverless context.4 By making this powerful feature accessible without upfront costs, Cloudflare encourages broader experimentation and innovation in the realm of stateful AI agents. This is likely to spur an increase in the development of more advanced and capable AI agents utilizing MCP on the Cloudflare platform, benefiting everyone from hobbyists and academic researchers to startups and larger enterprises.

## 7. Scaling, Performance, and Best Practices

Ensuring that MCP servers can scale to meet demand and perform with low latency is crucial for the effectiveness of AI agents. Cloudflare's global infrastructure and serverless architecture provide a strong foundation for achieving these goals, complemented by sound architectural design principles.

### 7.1. Leveraging Cloudflare's Infrastructure for Scalable and Performant MCP Deployments

- **Cloudflare Workers**: MCP servers deployed on Cloudflare Workers inherently benefit from the platform's global network. As of March 2025, Cloudflare Workers AI utilized GPUs in over 190 cities worldwide, and the general Workers compute presence is even more extensive.1 This distributed architecture allows MCP server logic to run physically close to end-users, significantly reducing network latency, which is critical for responsive AI interactions. The serverless nature of Workers also means that deployments automatically scale based on incoming request volume, eliminating the need for manual capacity planning and management.
    
- **Cloudflare CDN**: While many MCP interactions are dynamic and involve real-time processing, any static assets served by the MCP application (e.g., UI components for an associated web interface) or cachable API responses from tools can be effectively cached by Cloudflare's Content Delivery Network (CDN). Caching these assets closer to users further reduces latency and offloads origin server requests, improving overall application performance.15
    
- **Cloudflare Load Balancing**: For more complex MCP server architectures that might involve multiple instances or backend origins (though less common with typical Worker deployments), Cloudflare's Load Balancing services can distribute incoming traffic intelligently. This ensures high availability, resilience against single points of failure, and optimal resource utilization across the deployment.14
    
- **Intelligent Traffic Routing**: The Cloudflare network employs sophisticated algorithms to dynamically route traffic along the most optimal network paths. This continuous optimization helps to minimize latency and improve the reliability of connections to MCP servers from clients located anywhere in the world.35
    

### 7.2. Architectural Considerations for Robust MCP Server Design

Building robust and efficient MCP servers involves several key architectural considerations:

- **State Management**: For AI agents that require memory, contextual awareness, or coordination across multiple interactions, leveraging Cloudflare Durable Objects is the recommended approach. Durable Objects provide strongly consistent storage and compute capabilities, ideal for managing stateful MCP server instances.4
    
- **Authentication & Authorization**: Implementing robust OAuth 2.1 authentication and authorization flows is critical. Utilizing the workers-oauth-provider library simplifies this process and allows for the definition of granular permissions for each tool exposed by the MCP server, ensuring agents operate within defined boundaries.9
    
- **Transport Choice**: To ensure broad client compatibility and prepare for future protocol advancements, MCP servers should ideally support both Server-Sent Events (SSE) and Streamable HTTP transports concurrently. The McpAgent class facilitates this dual support.17
    
- **Error Handling and Observability**: Implementing comprehensive error handling within tool definitions and across the server logic is essential for reliability. Leveraging Cloudflare's observability tools, such as Workers Logs and AI Gateway logs, provides crucial insights for monitoring server health, debugging issues, and understanding usage patterns.12
    

Cloudflare's edge compute platform, Cloudflare Workers, serves as a fundamental enabler for achieving the low-latency interactions critical for real-time AI agent responsiveness. For conversational AI systems or agents tasked with performing time-sensitive operations, minimizing the delay between the agent's request, the MCP server's processing, and the execution of underlying tools is paramount. Deploying MCP servers on Workers inherently places the server logic at the network edge, geographically closer to users and potentially closer to the services with which the MCP tools interact.4 This architectural advantage significantly reduces round-trip times compared to traditional centralized cloud deployments, making it a key competitive differentiator for hosting performant MCP servers designed for interactive and immediate AI experiences.

The combination of serverless scaling, inherent in Cloudflare Workers, with the sophisticated stateful capabilities offered by Durable Objects (particularly when coupled with McpAgent's hibernation feature) presents a uniquely cost-effective and efficient operational model. This model is especially well-suited for deploying potentially numerous, long-lived AI agent sessions via MCP. AI agent applications may involve many concurrent user sessions, each potentially requiring its own isolated, stateful MCP server instance or context to maintain conversation history, user preferences, or ongoing task status. Cloudflare Workers scale automatically to handle fluctuating demand without manual intervention.33 Durable Objects provide the necessary per-instance state persistence.8 The McpAgent's hibernation capability then allows these stateful instances to "sleep" during periods of inactivity, ceasing compute consumption while preserving their complete state.8 This synergistic combination enables the support of a large number of persistent agent interactions without incurring the continuous cost of dedicated, always-on resources for each session, thereby offering a highly efficient and economically attractive model for scalable AI agent deployment.

## 8. Conclusion and Future Outlook

Cloudflare has rapidly established a comprehensive and robust ecosystem for the development, deployment, security, and management of Model Context Protocol (MCP) servers. This ecosystem is built upon the synergistic integration of core Cloudflare services: the serverless compute of Cloudflare Workers, the stateful capabilities of Durable Objects, the secure authentication framework provided by workers-oauth-provider, and the advanced security and observability layers offered by AI Gateway and Firewall for AI. This integrated approach empowers developers to build sophisticated AI agents that can securely and efficiently interact with a multitude of tools and services.

The strategic emphasis on remote MCP servers, facilitated by tools like McpAgent and mcp-remote, along with support for modern transport protocols like Streamable HTTP, positions Cloudflare at the forefront of the evolving AI agent landscape. The provision of public MCP servers for interacting with Cloudflare's own platform services not only delivers immediate utility but also serves as a powerful demonstration of the technology's potential.

Looking ahead, several potential developments can be anticipated. Enhancements to the Cloudflare Agents SDK, the McpAgent class, and the workers-oauth-provider library are likely to continue, further refining the developer experience and adding new capabilities. The expansion of language support beyond JavaScript/TypeScript and Python for MCP server development could attract an even wider developer base. We may also see an increase in the number and variety of pre-built public MCP servers offered by Cloudflare, simplifying interaction with more aspects of its platform. Furthermore, deeper integrations of AI-driven security analytics and automated observability features will likely become standard, providing more intelligent ways to manage and protect MCP deployments. As MCP matures, it is poised to become a ubiquitous standard for all forms of service-to-agent communication, and Cloudflare appears strategically positioned to play a central role in enabling and shaping this interconnected ecosystem.2

Cloudflare's current trajectory suggests it is not merely facilitating the adoption of MCP but is actively shaping the future architecture of AI agents. By providing a scalable, secure, and developer-centric platform optimized for remote, stateful MCP servers, Cloudflare is influencing how next-generation AI applications will be conceived, built, and deployed. The company's investment in tools and services specifically designed for AI agents and MCP 1, coupled with solutions like remote MCP deployment, stateful agents via Durable Objects, and deeply integrated security, directly addresses the core challenges in making AI agents practical, trustworthy, and scalable. As a major infrastructure provider with a global network, Cloudflare's robust offerings are likely to attract a significant portion of AI agent development, thereby establishing de facto standards and architectural patterns for these emerging intelligent systems.

The success and continued expansion of Cloudflare's MCP strategy could catalyze the growth of a new ecosystem of "AI-ready" services. As more businesses leverage Cloudflare's platform to expose their tools and APIs via MCP—a trend already visible with companies like Asana, Stripe, and Atlassian 2—it will become progressively easier for AI agents to discover and utilize a vast and diverse array of capabilities. This enrichment of the tool landscape available to AI agents will, in turn, make the agents themselves more powerful, versatile, and valuable. Such a development could incentivize an even broader range of service providers to make their offerings MCP-compatible, potentially using Cloudflare's accessible and secure platform to do so. This creates a virtuous cycle: more tools lead to more capable agents, which drives demand for more tools, fostering innovation and leading to a more interconnected, intelligent, and automated digital landscape, with Cloudflare serving as a key infrastructural and innovation hub.

#### Works cited

1. Cloudflare for AI: supporting AI adoption at scale with a security-first ..., accessed May 17, 2025, [https://blog.cloudflare.com/cloudflare-for-ai-supporting-ai-adoption-at-scale-with-a-security-first-approach/](https://blog.cloudflare.com/cloudflare-for-ai-supporting-ai-adoption-at-scale-with-a-security-first-approach/)
    
2. MCP Demo Day: How 10 leading AI companies built MCP servers on Cloudflare, accessed May 17, 2025, [https://blog.cloudflare.com/mcp-demo-day/](https://blog.cloudflare.com/mcp-demo-day/)
    
3. Cloudflare Powers Enterprise Access to Claude AI with MCP Server ..., accessed May 17, 2025, [https://www.channele2e.com/news/cloudflare-powers-enterprise-access-to-claude-ai-with-mcp-server-toolkit](https://www.channele2e.com/news/cloudflare-powers-enterprise-access-to-claude-ai-with-mcp-server-toolkit)
    
4. Cloudflare Accelerates AI Agent Development With The Industry's First Remote MCP Server, accessed May 17, 2025, [https://www.cloudflare.com/zh-cn/press-releases/2025/cloudflare-accelerates-ai-agent-development-remote-mcp/](https://www.cloudflare.com/zh-cn/press-releases/2025/cloudflare-accelerates-ai-agent-development-remote-mcp/)
    
5. MCP - The Cloudflare Blog, accessed May 17, 2025, [https://blog.cloudflare.com/tag/mcp/](https://blog.cloudflare.com/tag/mcp/)
    
6. Build and deploy Remote Model Context Protocol (MCP) servers to Cloudflare, accessed May 17, 2025, [https://blog.cloudflare.com/remote-model-context-protocol-servers-mcp/](https://blog.cloudflare.com/remote-model-context-protocol-servers-mcp/)
    
7. Build a Remote MCP server · Cloudflare Agents docs, accessed May 17, 2025, [https://developers.cloudflare.com/agents/guides/remote-mcp-server/](https://developers.cloudflare.com/agents/guides/remote-mcp-server/)
    
8. McpAgent — API Reference · Cloudflare Agents docs, accessed May 17, 2025, [https://developers.cloudflare.com/agents/model-context-protocol/mcp-agent-api/](https://developers.cloudflare.com/agents/model-context-protocol/mcp-agent-api/)
    
9. Authorization · Cloudflare Agents docs, accessed May 17, 2025, [https://developers.cloudflare.com/agents/model-context-protocol/authorization/](https://developers.cloudflare.com/agents/model-context-protocol/authorization/)
    
10. WorkOS + Cloudflare MCP: Plug and Play Auth for Agentic AI Builders, accessed May 17, 2025, [https://workos.com/blog/workos-cloudflare-mcp-auth-for-agentic-ai](https://workos.com/blog/workos-cloudflare-mcp-auth-for-agentic-ai)
    
11. Take control of public AI application security with Cloudflare's ..., accessed May 17, 2025, [https://blog.cloudflare.com/take-control-of-public-ai-application-security-with-cloudflare-firewall-for-ai/](https://blog.cloudflare.com/take-control-of-public-ai-application-security-with-cloudflare-firewall-for-ai/)
    
12. Keep AI interactions secure and risk-free with Guardrails in AI Gateway, accessed May 17, 2025, [https://blog.cloudflare.com/guardrails-in-ai-gateway/](https://blog.cloudflare.com/guardrails-in-ai-gateway/)
    
13. Build an MCP Server · Cloudflare Agents docs, accessed May 17, 2025, [https://developers.cloudflare.com/agents/examples/build-mcp-server/](https://developers.cloudflare.com/agents/examples/build-mcp-server/)
    
14. Control Cloudflare Infrastructure Using AI + MCP (with Python ..., accessed May 17, 2025, [https://dev.to/extinctsion/control-cloudflare-infrastructure-using-ai-mcp-with-python-example-1bak](https://dev.to/extinctsion/control-cloudflare-infrastructure-using-ai-mcp-with-python-example-1bak)
    
15. What Is Cloudflare MCP? A Look at the Model Context Protocol and AI Integration - Guru, accessed May 17, 2025, [https://www.getguru.com/reference/cloudflare-mcp](https://www.getguru.com/reference/cloudflare-mcp)
    
16. Introducing one-click remote MCP servers with Cloudflare - Workers AI, accessed May 17, 2025, [https://community.cloudflare.com/t/introducing-one-click-remote-mcp-servers-with-cloudflare/795791](https://community.cloudflare.com/t/introducing-one-click-remote-mcp-servers-with-cloudflare/795791)
    
17. Transport · Cloudflare Agents docs, accessed May 17, 2025, [https://developers.cloudflare.com/agents/model-context-protocol/transport/](https://developers.cloudflare.com/agents/model-context-protocol/transport/)
    
18. Bringing streamable HTTP transport and Python language support to MCP servers, accessed May 17, 2025, [https://blog.cloudflare.com:2096/ru-ru/streamable-http-mcp-servers-python/](https://blog.cloudflare.com:2096/ru-ru/streamable-http-mcp-servers-python/)
    
19. cloudflare/workers-mcp: Talk to a Cloudflare Worker from ... - GitHub, accessed May 17, 2025, [https://github.com/cloudflare/workers-mcp](https://github.com/cloudflare/workers-mcp)
    
20. Test a Remote MCP Server · Cloudflare Agents docs, accessed May 17, 2025, [https://developers.cloudflare.com/agents/guides/test-remote-mcp-server/](https://developers.cloudflare.com/agents/guides/test-remote-mcp-server/)
    
21. OAuth provider library for Cloudflare Workers - GitHub, accessed May 17, 2025, [https://github.com/cloudflare/workers-oauth-provider](https://github.com/cloudflare/workers-oauth-provider)
    
22. Piecing together the Agent puzzle: MCP, authentication ..., accessed May 17, 2025, [https://blog.cloudflare.com/building-ai-agents-with-mcp-authn-authz-and-durable-objects/](https://blog.cloudflare.com/building-ai-agents-with-mcp-authn-authz-and-durable-objects/)
    
23. Building an MCP server with OAuth and Cloudflare Workers - Stytch, accessed May 17, 2025, [https://stytch.com/blog/building-an-mcp-server-oauth-cloudflare-workers/](https://stytch.com/blog/building-an-mcp-server-oauth-cloudflare-workers/)
    
24. Tools · Cloudflare Agents docs, accessed May 17, 2025, [https://developers.cloudflare.com/agents/model-context-protocol/tools/](https://developers.cloudflare.com/agents/model-context-protocol/tools/)
    
25. mcp-server-cloudflare - Glama, accessed May 17, 2025, [https://glama.ai/mcp/servers/@cloudflare/mcp-server-cloudflare](https://glama.ai/mcp/servers/@cloudflare/mcp-server-cloudflare)
    
26. Cloudflare MCP Server - GitHub, accessed May 17, 2025, [https://github.com/cloudflare/mcp-server-cloudflare](https://github.com/cloudflare/mcp-server-cloudflare)
    
27. Thirteen new MCP servers from Cloudflare you can use today, accessed May 17, 2025, [https://blog.cloudflare.com/thirteen-new-mcp-servers-from-cloudflare/](https://blog.cloudflare.com/thirteen-new-mcp-servers-from-cloudflare/)
    
28. Cloudflare R2 MCP Server | Pipedream, accessed May 17, 2025, [https://mcp.pipedream.com/app/cloudflare_r2](https://mcp.pipedream.com/app/cloudflare_r2)
    
29. Cloudflare - MCP Server - Cursor Directory, accessed May 17, 2025, [https://cursor.directory/mcp/cloudflare](https://cursor.directory/mcp/cloudflare)
    
30. Cloudflare - MCP Server - Magic Slides, accessed May 17, 2025, [https://www.magicslides.app/mcps/gutmutcode-cloudflare](https://www.magicslides.app/mcps/gutmutcode-cloudflare)
    
31. Cloudflare API - MCP Server - Magic Slides, accessed May 17, 2025, [https://www.magicslides.app/mcps/amxv-cloudflare-api](https://www.magicslides.app/mcps/amxv-cloudflare-api)
    
32. Workers AI LLM Playground, accessed May 17, 2025, [https://playground.ai.cloudflare.com/](https://playground.ai.cloudflare.com/)
    
33. Cloudflare Introduces Cloudflare for AI, a Comprehensive Suite of ..., accessed May 17, 2025, [https://www.cloudflare.com/press-releases/2025/cloudflare-introduces-cloudflare-for-ai/](https://www.cloudflare.com/press-releases/2025/cloudflare-introduces-cloudflare-for-ai/)
    
34. Cloudflare DDoS Web Protection Demo - SecuritySenses, accessed May 17, 2025, [https://securitysenses.com/videos/cloudflare-ddos-web-protection-demo](https://securitysenses.com/videos/cloudflare-ddos-web-protection-demo)
    
35. MCP Global Deployment: Guide to Cloudflare & AI Integration - BytePlus, accessed May 17, 2025, [https://www.byteplus.com/en/topic/542010](https://www.byteplus.com/en/topic/542010)