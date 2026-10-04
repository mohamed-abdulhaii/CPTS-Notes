# Overview

Crawling (also called **spidering**) in web penetration testing is the automated process of exploring a web application by following links, clicking buttons, and submitting forms to map out its structure and discover hidden pages, files, and API endpoints. Unlike fuzzing or brute-forcing—which rely on guessing names—crawling works by systematically parsing web pages, extracting valid links (`href`, `src`, form actions), and following them iteratively across the target.

عملية الـ Web Crawling (أو الـ Spidering) هي تتبع ورو زحف تلقائي للموقع؛ البوت يبدأ من صفحة البداية (Seed URL)، يسحب كل اللينكات الموجودة فيها، يدخل على كل لينك فيهم ويسحب اللينكات اللي جواه وهكذا عشان يرسم خريطة كاملة للموقع والصفحات المربوطة ببعض.


### Crawling Strategies

1. **Breadth-First Crawling**
Explores the website level by level. It visits all direct links on the current page before moving deeper down the hierarchy. Useful for gaining a high-level overview of the application structure.

![[Pasted image 20260920092151.png]]


2. **Depth-First Crawling**
Follows a single link chain as deep as possible until reaching the end before backtracking to explore alternate branches. Ideal for discovering deeply nested resources.

![[Pasted image 20260920092324.png]]

--------------------------------------------------------------------------

Think of a crawler as a digital detective that automatically reads every single page of a website, clicks every link, and notes down everything it finds.

### Key Data the Crawler Collects (The Loot)

- **Links (Internal & External):** Maps out the whole layout of the website, showing hidden pages, navigation routes, and connections to outside services.
- **Code Comments:** Developer notes left inside the website's source code. Sometimes coders accidentally leave behind passwords, old instructions, or hints about hidden APIs.
- **Metadata:** Information about how the site was built, such as software versions, framework tags, and author names.
- **Sensitive Files:** Accidental leaks of things that should be hidden—like backup archives (`.zip`, `.bak`), configuration files (`.env`), or open folders where you can browse all files directly.

But finding a single piece of information is nice, but the real power comes when you **connect the clues like a puzzle**:

- **Example 1:** Finding a link to a folder like `/files/` is normal. But if that folder has _Directory Listing_ turned on (meaning anyone can view its contents), you suddenly have open access to download private backup files.
    
- **Example 2:** Reading a developer comment that mentions an internal server (like _"TODO: update storage.local"_), combined with finding an exposed configuration file, gives you a clear blueprint of how the system works and where to look next.