Web Security Header Analyzer is a Python web app that checks how well a website’s HTTP response headers are configured for security. You enter a URL, and it fetches only the headers, evaluates them against simple rules, and shows a score with recommendations.

What it checks

Content-Security-Policy (flags unsafe-inline, unsafe-eval and wildcards)
Strict-Transport-Security (looks for a max-age of at least six months)
X-Content-Type-Options (expects nosniff)
X-Frame-Options (expects DENY or SAMEORIGIN)
Referrer-Policy (flags unsafe-url and no-referrer-when-downgrade)

How it works

app.py: the Streamlit interface, with a dark-themed page showing the score out of 100, the number of headers present, the findings count, and the HTTP status. It also shows a results table, expandable recommendations, and the raw headers.
analyzer.py: the core logic. It validates and normalizes the URL, follows up to 5 redirects manually, evaluates each header as Good, Review, Missing or N/A, and calculates the score (20 points per header).
reports.py: builds downloadable HTML and JSON reports.

Safety features

Only passive checks: it never downloads page content.
Blocks localhost, private and non-public IPs, and URLs with embedded usernames or passwords.
Re-validates every redirect.
Connects to the same IP it validated, which prevents DNS rebinding.

Tech stack: Python, Streamlit, Requests, Pandas.

Limitations: it’s a header-only check, so it isn’t a full security audit. A header being present doesn’t prove it’s configured well, and it covers only five headers. It should only be used on sites you own or are authorized to test.

Possible next steps: add more headers (Permissions-Policy, Cross-Origin policies), add cookie flag checks, and add unit tests for _evaluate.
