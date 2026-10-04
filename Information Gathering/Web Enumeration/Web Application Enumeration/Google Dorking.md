
Search Engine Discovery (Google Dorking / OSINT) relies on leveraging advanced search operators to filter, isolate, and uncover publicly indexed data, sensitive administrative portals, exposed configuration files, database backups, and internal documentation.

## Search Operators

| **Operator**         | **Description**                                                   | **Usage Example**                               |
| -------------------- | ----------------------------------------------------------------- | ----------------------------------------------- |
| `   site:`           | Restricts results strictly to a specific target domain.           | `site:example.com`                              |
| `inurl:`             | Filters pages containing a specific keyword within the URL path.  | `inurl:login`                                   |
| `filetype:` / `ext:` | Filters specific file extensions (PDF, SQL, TXT, LOG, CONF).      | `filetype:pdf` / `ext:sql`                      |
| `intitle:`           | Searches for pages matching specific terms in the HTML title bar. | `intitle:"Index of /"`                          |
| `intext:`            | Searches for specific keywords within the visible page body text. | `intext:"password reset"`                       |
| `cache:`             | Displays Google's historic cached snapshot of a URL.              | `cache:example.com`                             |
| `NOT` / `-`          | Excludes specific terms or paths from search results.             | `site:example.com -inurl:login`                 |
| `OR` / `AND`         | Applies Boolean logic to broaden or narrow target conditions.     | `site:example.com (inurl:admin OR inurl:login)` |
| `""`                 | Enforces exact phrase matching.                                   | `"confidential report"`                         |

### Practical Google Dorking Patterns (pentester uses)

- **Finding Admin & Login Portals**
`site:target.com (inurl:login OR inurl:admin OR inurl:dashboard OR inurl:portal)`

-  Directory Listing & File Browsing
`site:target.com intitle:"Index of /"`

- Exposed Sensitive Documents
`site:target.com (filetype:pdf OR filetype:doc OR filetype:xls OR filetype:xlsx) "internal use only"`

- Leaked Configuration Files & Environment Keys
`site:target.com (ext:xml OR ext:conf OR ext:cnf OR ext:reg OR ext:inf OR ext:env) intext:db_password`

-  Database Dumps & Backups
`site:target.com (ext:sql OR ext:db OR ext:tar OR ext:zip OR ext:bak)`