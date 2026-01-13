## Dataset and files for submission "Accountability Gaps in the Fake Follower Market"

This dataset will be published on Kaggle in unanonymised form. We uploaded this anonymised version for reviewers.

Our project investigates the online economy of Fake Activity Shops (FAS): commercial websites selling fake social-media engagement, such as followers, likes, comments, views, and other inauthentic interactions. This dataset captures websites that **sell fake social-media engagement** (“FakeActivity”). Each row represents one domain/snapshot. We identified **≈2000** Fake Activity Shops (FAS) across **24** European countries. Using LLM-based classifiers and extraction agents achieving **~86%** accuracy for platform/price parsing, the project collects detailed metrics on each shop’s WHOIS/DNS/hosting configuration, site structure, platform-specific price indicators, payment methods, technology stack, search-engine discoverability, and multiple security and reputation signals.

## Schema of FAS_data.csv

### Identification & time
- `domain` — domain name.  
- `scan_timestamp` — crawl time (ISO 8601).  
- `domain_registration_date`, `registration_date` — domain registration date.

### WHOIS / DNS / network
- `whois_registrar`, `whois_privacy` (Public/Private), `whois_country`.  
- `whois_name_servers`, `dns_a_records`, `dns_ns_records` — stored as list-like strings.  
- `ip`, `reverse_dns`, `reverse_dns_redacted` (True/False).  
- `asn`, `asn_description`, `org_name`, `org_country` — AS/hosting meta.

### Page content (meta)
- `html_title`, `html_meta_description` — from the home page.

### Visibility & geography
- `countries` — list of targeted countries (stringified list).  
- `serp` — JSON with per-country search ranks (`by_country &rarr; {google|bing|duckduckgo: [positions]}`).

### Size & links
- `num_pages` — estimated number of HTML pages.  
- `external_links_total`, `internal_links_total` — link counts.

### Site sections (presence/count)
- `section_pages_pricing`, `section_pages_services`, `section_pages_faq`,  
  `section_pages_reviews`, `section_pages_contact`, `section_pages_about`,  
  `section_pages_blog_news`, `section_pages_terms_privacy_refund`,  
  `section_pages_social_chat` — integers (0 = not found; >0 = count).

### Payments (mentions/pages)
- `payment_pages_paypal`, `payment_pages_stripe`, `payment_pages_yookassa`, `payment_pages_payeer`,  
- `payment_pages_skrill`, `payment_pages_wise`, `payment_pages_revolut`, `payment_pages_coinbase`,  
- `payment_pages_binance`, `payment_pages_usdt`, `payment_pages_cryptomus`, `payment_pages_coingate`,  
- `payment_pages_qiwi`, `payment_pages_interkassa`, `payment_pages_cards` — integers (page counts).

### AI / automation (mentions/pages)
- `ai_pages_ai`, `ai_pages_chatgpt`, `ai_pages_automation`, `ai_pages_ml_dl` — integers.

### Tech stack (mentions/pages)
- `framework_pages_wordpress`, `framework_pages_woocommerce`, `framework_pages_shopify`,  
- `framework_pages_squarespace`, `framework_pages_wix`, `framework_pages_bigcommerce`,  
- `framework_pages_nextjs`, `framework_pages_react`, `framework_pages_vue`,  
- `framework_pages_angular`, `framework_pages_tailwind`, `framework_pages_bootstrap` — integers.

### GDPR signal
- `gdpr_pages` — count of GDPR-related pages/mentions.

### Platforms, indicators, and prices
- `platforms` — JSON object:  
  `{ "<platform>": { "<indicator>": { "median": <float USD/unit>, "outlier": 0|1 }, ... }, ... }`  
  Examples of `<platform>`: `instagram`, `tiktok`, `youtube`, `facebook`, `twitter`/`x`, `telegram`, `spotify`, etc.  
  Examples of `<indicator>`: `like`, `views`, `subscribtion` *(spelling as in data)*, `comment`, `other`.  
  **Outlier flag semantics:** `outlier = 1` &rarr; **not** an outlier (kept in central trend); `outlier = 0` &rarr; flagged as an outlier.

### Security signals (aggregated)
- `urlscan_malicious` (True/False), `urlscan_tags`, `urlscan_related_domains`.  
- `cloudflare_protected` (True/False).  
- `virustotal_malicious` (True/False), `virustotal_malicious_count`, `virustotal_suspicious_count`.  
- `cookie_count`, `urlscan_suspicious_cookies_count`, `suspicious_cookie_flags`.  
- `tracker_domain_count`, `unique_domains_contacted`.  
- `payload_like_download` (True/False), `suspicious_tld` (True/False), `keyword_hits`, `lookyloo_risk_score` (0–10).  
- ZAP: `zap_has_high_risk` (True/False), `zap_high_risk_count`, `zap_medium_risk_count`, `zap_low_risk_count`,  
  `zap_high_risk_issues`, `zap_medium_risk_issues`, `zap_low_risk_issues` (list-like strings).  
- JS heuristics: `js_external_form_targets`, `js_eval_usage`, `js_function_constructor_usage`,  
  `js_settimeout_string_usage`, `js_keylogging_detected`, `js_redirection_detected`,  
  `js_content_injection_detected`, `js_cookie_access_detected`, `js_malicious_behavior_score_10`.

### JSON parsing hint
- Fields like `platforms`, `countries`, `serp`, and `*_issues` are stored as strings; parse with `json.loads`.
