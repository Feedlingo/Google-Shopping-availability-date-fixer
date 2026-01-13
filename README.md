Feedlingo Availability-Date Fixer for Google Shopping / Merchant Center

Feedlingo Availability-Date Fixer is a lightweight, self-hosted PHP tool that automatically generates a Google Shopping supplemental feed for products that are not in stock yet.

Many shops don’t know the exact delivery date for preorder or backorder products, but Google Merchant Center requires an availability_date for these items.
This tool solves that problem by automatically creating a supplemental feed where:

availability_date = today + N days (UTC)

You keep full control over the offset (e.g. 5, 10, 30 days) while still providing a valid date to Google.


SCREENSHOT

The screenshot shows the web-based configuration UI, including source feed selection, supplemental feed target, availability date offset and cronjob setup.

Image path in repository:
docs/availability-date-fixer-backend.png


WHAT IT DOES

- Reads the main product feed (XML or CSV)
- Filters out all products with availability = in stock
- Keeps only products with any other availability (preorder, backorder, out of stock, etc.)
- Writes a separate Google RSS supplemental feed containing:
  - g:id
  - g:availability
  - g:availability_date (auto-calculated)
- Designed for cronjob-based automation
- Includes a manual test run via the UI


SUPPORTED INPUT FORMATS

XML (Google Shopping format)

The tool first attempts to parse standard Google Shopping XML feeds such as rss/channel/item or feed/entry.


CSV

If XML parsing fails, the tool automatically falls back to CSV parsing.

Requirements:
- Header row
- One column for product ID
- One column for availability

Accepted column names (case-insensitive):

ID:
id, g:id, item_id, item id, product_id, product id

Availability:
availability, g:availability, availability_status, stock_status

Both comma and semicolon delimiters are supported and auto-detected.


OUTPUT FORMAT (SUPPLEMENTAL FEED)

The tool generates a clean Google RSS supplemental feed containing product IDs, availability states and automatically calculated availability dates.

The file is added in Google Merchant Center as a supplemental feed and linked to the main feed via the ID field.


REQUIREMENTS

- PHP 5.6 or higher
- Extensions: DOM, SimpleXML, json
- Webserver (Apache or Nginx) or PHP built-in server

No database, no Composer, no framework, no external API.


INSTALLATION

1. Copy feedlingo_availability_date_fixer.php to a directory on your webserver.
2. Ensure the webserver user has write permissions in that directory.
3. Open the script in your browser.
4. Configure source feed, target file, availability date offset and shop base URL.
5. Save configuration.


TEST RUN (MANUAL)

The UI includes a Run test now button.
It immediately generates the supplemental feed and shows a result message.
Use this to verify feed parsing, column mapping and file permissions before enabling the cronjob.


CRONJOB USAGE

Once configured, the generator can be triggered via URL:

https://yourserver.com/feedlingo_availability_date_fixer.php?run=1&secret=YOUR_SECRET_TOKEN

Linux cron example:
0 5 * * * /usr/bin/wget -qO- "https://yourserver.com/feedlingo_availability_date_fixer.php?run=1&secret=YOUR_SECRET_TOKEN" > /var/log/feedlingo_availability.log 2>&1


SECURITY

- A random secret token is generated on first run.
- The cron endpoint returns HTTP 403 if the secret is missing or invalid.
- Secrets can be regenerated via the UI.

Optional hardening:
- HTTP authentication
- IP allow-listing
- Placement in a protected backend directory


USE CASE SUMMARY

This tool is ideal if:
- Your shop or ERP exports a basic feed (XML or CSV) without availability_date
- You manage many preorder or backorder products
- Manual date maintenance is not feasible
- You want a local, transparent, self-hosted solution
- You need Google Shopping compliance with minimal effort

---

## License

MIT License

Copyright (c) 2025 Feedlingo

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

