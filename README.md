# 🔒 WordPress Plugin Security Audit Viewer

![Last Commit](https://img.shields.io/github/last-commit/Siphon880gh/wp-plugin-security-audit/main)
<a target="_blank" href="https://github.com/Siphon880gh" rel="nofollow"><img src="https://img.shields.io/badge/GitHub--blue?style=social&logo=GitHub" alt="Github" data-canonical-src="https://img.shields.io/badge/GitHub--blue?style=social&logo=GitHub" style="max-width:8.5ch;"></a>
<a target="_blank" href="https://www.linkedin.com/in/weng-fung/" rel="nofollow"><img src="https://img.shields.io/badge/LinkedIn-blue?style=flat&logo=linkedin&labelColor=blue" alt="Linked-In" data-canonical-src="https://img.shields.io/badge/LinkedIn-blue?style=flat&amp;logo=linkedin&amp;labelColor=blue" style="max-width:10ch;"></a>
<a target="_blank" href="https://www.youtube.com/@WayneTeachesCode/" rel="nofollow"><img src="https://img.shields.io/badge/Youtube-red?style=flat&logo=youtube&labelColor=red" alt="Youtube" data-canonical-src="https://img.shields.io/badge/Youtube-red?style=flat&amp;logo=youtube&amp;labelColor=red" style="max-width:10ch;"></a>

By Weng Fei Fung (Weng). A lightweight, browser-based tool for visualizing security audit results of WordPress plugins. This tool helps identify vulnerabilities, backdoors, and signs of nulled/pirated plugins.

## Features

- **Security Audit Visualization** - Display security findings in an interactive, sortable table
- **Piracy Detection** - Calculate and display piracy/nulled scores with indicators
- **Filtering & Sorting** - Filter by severity, type, title, or file path
- **Export Reports** - Save audit results as standalone HTML files
- **AI Integration** - Includes prompts for Cursor AI & ChatGPT to perform security audits

## Usage

### Running the Viewer

1. Open `index.html` in any modern web browser
2. No build process or server required - it's a standalone HTML file

### Performing a Security Audit

**Step 1: Copy and use this prompt for ChatGPT or Cursor AI:**

> **You are a security auditor for WordPress plugins.** I have provided a plugin codebase. Your job is to find security vulnerabilities, insecure coding patterns, backdoors, signs the plugin has been nulled/pirated, and any suspicious/obfuscated code that may hide malicious functionality.
> 
> **Goals:**
> 1. Identify security vulnerabilities (XSS, SQLi, CSRF, RCE, file inclusion, insecure deserialization, privilege escalation, insecure use of WP APIs, hardcoded secrets).
> 2. Identify backdoors or persistence mechanisms (hidden admin creation, eval/base64/gzinflate chains, remote update or command endpoints, scheduled tasks that call remote URLs).
> 3. Detect signs that the plugin is nulled/pirated (removed license checks, replaced update URLs, modified plugin headers, presence of "crack" comments, replaced licensing files).
> 4. Provide clear remediation steps and code fixes, and mark files that need urgent removal/cleanup.
> 
> **Instructions:**
> - Scan all PHP, JS, and assets. Focus on patterns: `eval()`, `create_function()`, `base64_decode()`, `gzinflate()`, `preg_replace("/.*/e")`, `system/exec/passthru/shell_exec`, `curl/wp_remote_post` to suspicious domains, `file_put_contents` to theme/plugin directories, and dynamic includes.
> - Check for missing capability/nonce checks on actions (AJAX endpoints, admin_post, REST API endpoints), and note where `current_user_can`, `check_admin_referer`, `wp_verify_nonce`, or `$wpdb->prepare` are missing or misused.
> - For each finding, return JSON array `findings[]` where each finding includes: `title`, `severity` (critical/high/medium/low), `type`, `file_path`, `line_numbers`, `explanation`, and `recommended_fix`.
> - Also return a short `nulled/piracy_score` (0–100) with a short rationale and list of matching indicators.
> - If you are unsure of a runtime behavior, mark as "requires dynamic verification" and give a short dynamic test to run.
> - **Output only valid JSON. No other prose.**

2. Provide your WordPress plugin codebase to Cursor AI or ChatGPT
3. The AI will analyze the code and return JSON with findings
4. Load the JSON into this viewer to visualize results

### Loading Audit Data

**Option 1: Browse for JSON file**
- Click "Browse JSON File" button
- Select your audit JSON file

**Option 2: Paste JSON directly**
- Copy your audit JSON
- Paste into the textarea
- Click "Process & Display Results"


## Audit JSON Format

The tool expects JSON in the following format:

```json
{
  "nulled_piracy_score": 85,
  "piracy_rationale": "Multiple indicators of nulled plugin detected",
  "piracy_indicators": [
    "License check bypassed in main plugin file",
    "Update URL replaced with suspicious domain"
  ],
  "urgent_actions": [
    "Remove backdoor in admin-ajax.php immediately",
    "Delete obfuscated file malware.php"
  ],
  "findings": [
    {
      "title": "SQL Injection Vulnerability",
      "severity": "critical",
      "type": "SQLi",
      "file_path": "includes/database.php",
      "line_numbers": "45-52",
      "explanation": "User input directly concatenated into SQL query",
      "recommended_fix": "Use $wpdb->prepare() to sanitize inputs"
    }
  ],
  "requires_dynamic_verification": [
    {
      "test": "Check if remote code execution endpoint is accessible",
      "method": "POST",
      "endpoint": "/wp-admin/admin-ajax.php?action=eval_code"
    }
  ]
}
```

## Severity Levels

- **Critical** - Remote code execution, SQL injection with data exposure, authentication bypass
- **High** - XSS, CSRF on sensitive actions, privilege escalation
- **Medium** - Information disclosure, insecure defaults, missing nonce checks
- **Low** - Code quality issues, minor information leaks

## Security Patterns Detected

The AI audit checks for:

- **Vulnerabilities**: XSS, SQLi, CSRF, RCE, file inclusion, insecure deserialization
- **Backdoors**: Hidden admin creation, eval/base64 chains, remote command endpoints
- **Piracy Indicators**: Removed license checks, replaced update URLs, crack comments
- **Dangerous Functions**: `eval()`, `create_function()`, `base64_decode()`, `gzinflate()`, `system()`, `exec()`
- **Missing Security**: Capability checks, nonce verification, input sanitization

## Export Reports

Click the "Save as HTML" button to export a standalone report that includes:
- All audit findings
- Piracy score and indicators
- Filtering and sorting capabilities
- Timestamp of generation

## Browser Compatibility

Works in all modern browsers:
- Chrome/Edge 90+
- Firefox 88+
- Safari 14+

## License

This tool is provided as-is for security research and auditing purposes.

## Contributing

Feel free to submit issues or pull requests to improve the viewer or audit prompts.

