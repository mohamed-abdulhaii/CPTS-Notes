
```
python3 ReconSpider.py http://<target>.com

cat results.json | jq '.<section>'

```


The best tool for crawling the web depends heavily on your technical skill and project goals, with **[Scrapy](https://scrapy.org/)** serving as the gold standard for custom code-first developer projects. 

The relationship is that **`ReconSpider.py` depends on `scrapy` to function.**
Here is how they work together:
- **`scrapy` is the engine:** It is a powerful, open-source Python framework specifically designed for web crawling, spidering, and extracting data from websites.
- **`ReconSpider` is the vehicle:** It is a reconnaissance and OSINT tool built _on top_ of Scrapy. It uses Scrapy's core crawling engine to automatically spider target websites, follow links, and pull information during a security assessment.

The tool saves the output in results.json and categorized into sections  : 

- `emails`: Discovered internal/external email addresses.
- `links`: Internal and external linked URLs.
- `external_files`: Uploaded documents (PDFs, DOCX, ZIPs).
- `js_files`: Paths to JavaScript assets (ideal for finding hidden API routes).
- `comments`: HTML inline developer comments (frequently leaks sensitive notes or internal paths).

