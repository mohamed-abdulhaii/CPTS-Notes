
# robots.txt 

A **robots.txt** file is a text document placed in a website's root directory that tells web crawlers and search engine bots which pages or folders they are allowed to visit. The `robots.txt` file acts as an access guide for web crawlers, following the Robots Exclusion Standard. Located in the root directory of a web server (e.g., `[https://example.com/robots.txt](https://example.com/robots.txt)`), it communicates which paths or directories web spiders are allowed or forbidden to crawl and index.

### File Structure & Core Directives

- **User-agent**: Specifies the targeted crawler (e.g., `User-agent: *` for all bots, or `User-agent: Googlebot` specifically for Google's crawler)
- **Disallow**: Specifies directories, paths, or file extensions off-limits to spiders.
- **Allow**: Explicitly permits access to a specific subpath within a broader `Disallow` rule.
- **Crawl-delay**: Imposes a time buffer (in seconds) between requests to prevent server overloading.
- **Sitemap**: Directly points crawlers to the XML sitemap file location for index optimization.

security professionals and attackers treat `Disallow` entries as high-priority target maps: 
- **Uncovering Sensitive Endpoints**: (`/admin/`, `/dashboard/, /private/ , /backup/ `)
- **Structural Blueprinting** :helps construct an accurate diagram of the web application's structure.
- **Honeypot / Crawler Trap Identification**
--------------------------------------------------------------------------

# Well-Known URIs

The `.well-known` standard (defined in RFC 8615) specifies a reserved path prefix—`/.well-known/`—located at the root domain of a web server. It centralizes site-wide metadata, service discovery configurations, protocol definitions, and security policies in a predictable, standardized format.

Think of **`/.well-known/`** as a master filing cabinet or a front-desk directory located right at the front door of a website (`[example.com/.well-known/](https://example.com/.well-known/)`). Instead of hiding important configuration files all over the server, web standards say they should all be put in this one predictable folder so apps and tools can find them automatically.

### Important Files Found Inside It

-  **`security.txt`:** A quick instruction guide telling ethical hackers how to safely report security bugs to the company.
- **`openid-configuration`:** A blueprint file that explains how the website handles user logins and security tokens (OAuth/OIDC)
- **`change-password`:** A direct link that leads users straight to the password change page.
- **`assetlinks.json`:** A file that proves a mobile app (like an Android app) officially belongs to this website.
-  **`mta-sts.txt`:** Security rules to protect and encrypt the website's emails.

`openid-configuration` Analysis : When you open that configuration file, it dumps a JSON sheet showing the backend plumbing of the website's login system. It literally lists URLs for

- Where to send users to log in (`authorization_endpoint`)
- Where to trade passwords for security tokens (`token_endpoint`)
- Where to fetch user profile details (`userinfo_endpoint`)

#### Recon Value for Well-Known folder : 

- **Uncovering Hidden Login Routes:** It maps out the exact backend APIs used for user authentication that might not be visible on the main website.
- **Exposing Encryption Keys (`jwks_uri`):** It reveals public keys (`jwks_uri`) used to sign security tokens (JWTs). Hackers look at these to see if they can trick the system into accepting fake, forged tokens.
- **Spotting Weak Security Settings:** It shows which cryptographic algorithms the server allows, helping testers hunt for weak or outdated settings (like an algorithm set to "none").
