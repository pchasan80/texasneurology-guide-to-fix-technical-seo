# Technical SEO Fixes for Texas Neurology

This document outlines the steps required to fix technical SEO issues identified in the Ahrefs audit for [https://texasneurology.com/index.asp](https://texasneurology.com/index.asp).

## Issue 1: HTTPS page has internal links to HTTP

**Problem:**
Some pages on the secure (HTTPS) version of the website contain links to non-secure (HTTP) pages within the same domain. This causes "Mixed Content" warnings in browsers and negatively impacts SEO and user trust.

**Solution:**
You need to update all internal links to use the `https://` protocol instead of `http://`.

### Steps to Fix:

1.  **Identify the Links:**
    *   Use the Ahrefs report to find the specific pages and the exact links that are using `http://`.
    *   Alternatively, you can search your codebase (HTML, ASP, ASPX, CS files) for `http://texasneurology.com` or `http://www.texasneurology.com`.

2.  **Update Code (Hardcoded Links):**
    *   Open the source files for the affected pages.
    *   Locate the `<a>` tags (links), `<img>` tags (images), `<script>` tags, or `<link>` tags (stylesheets) pointing to `http://`.
    *   Change `http://` to `https://`.
    *   *Better yet:* Use relative paths (e.g., `/about-us.asp`) instead of absolute paths (e.g., `http://texasneurology.com/about-us.asp`) if the link is internal. This automatically uses the current protocol.

3.  **Update Database (Dynamic Content):**
    *   If links are stored in a database (e.g., blog posts, news items), run a SQL query to update them.
    *   *Example SQL (Backup your database first!):*
        ```sql
        UPDATE YourTableName
        SET YourColumnName = REPLACE(YourColumnName, 'http://texasneurology.com', 'https://texasneurology.com')
        WHERE YourColumnName LIKE '%http://texasneurology.com%';
        ```

4.  **Force HTTPS (Global Fix):**
    *   Ensure your `web.config` file is set up to redirect all HTTP traffic to HTTPS. This ensures that even if an old link exists, the user is redirected to the secure version.
    *   Add this rule to the `<system.webServer>` section of your `web.config`:

    ```xml
    <rewrite>
      <rules>
        <rule name="HTTP to HTTPS redirect" stopProcessing="true">
          <match url="(.*)" />
          <conditions>
            <add input="{HTTPS}" pattern="off" ignoreCase="true" />
          </conditions>
          <action type="Redirect" redirectType="Permanent" url="https://{HTTP_HOST}/{R:1}" />
        </rule>
      </rules>
    </rewrite>
    ```
    *(Note: This requires the IIS URL Rewrite module to be installed on the server).*
