# Contributing to Sigma Rules Repository

Thank you for your interest in contributing to our Sigma rules collection! This document provides guidelines for contributing detection rules.

## 📋 Prerequisites

Before contributing, please ensure you:

1. Have a basic understanding of [Sigma rule syntax](https://github.com/SigmaHQ/sigma-specification)
2. Understand the threat or attack technique your rule detects
3. Have tested your rule against sample data when possible
4. Are familiar with YAML syntax

## 🔧 Setting Up Your Development Environment

### 1. Fork and Clone

```bash
# Fork this repository via GitHub UI
# Clone your fork
git clone https://github.com/YOUR_USERNAME/sigma-rules.git
cd sigma-rules
```

### 2. Install Validation Tools

```bash
# Install Python dependencies
pip install pyyaml jsonschema yamllint

# Or use a virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install pyyaml jsonschema yamllint
```

## 📝 Creating a New Sigma Rule

### 1. Choose the Right Category

Place your rule in the appropriate directory:

- `rules/windows/` - Windows-specific rules
- `rules/linux/` - Linux-specific rules
- `rules/network/` - Network-based rules
- `rules/cloud/` - Cloud platform rules (AWS, Azure, GCP)
- `rules/application/` - Application-specific rules
- `rules/web/` - Web application rules

### 2. Generate a Unique ID

Each rule needs a unique UUID. Generate one using:

```bash
# Linux/Mac
uuidgen | tr '[:upper:]' '[:lower:]'

# Python
python -c "import uuid; print(str(uuid.uuid4()))"

# Online
# Visit https://www.uuidgenerator.net/
```

### 3. Use the Rule Template

```yaml
title: Descriptive Rule Title
id: your-unique-uuid-here
status: test  # Options: stable, test, experimental
description: Detailed description of what this rule detects and why it's important
references:
    - https://reference-to-threat-intel.com
    - https://attack.mitre.org/techniques/T1234/
author: Your Name or Organization
date: YYYY/MM/DD  # Creation date
modified: YYYY/MM/DD  # Last modification date
tags:
    - attack.technique_category
    - attack.t1234  # MITRE ATT&CK technique ID
logsource:
    category: category_name  # e.g., process_creation, network_connection
    product: product_name    # e.g., windows, linux, aws
    service: service_name    # Optional, e.g., sysmon, cloudtrail
detection:
    selection:
        field_name: value
        another_field|modifier: value
    condition: selection
falsepositives:
    - Known legitimate scenario 1
    - Known legitimate scenario 2
level: medium  # Options: informational, low, medium, high, critical
fields:  # Optional: fields to include in alert
    - field1
    - field2
```

### 4. Required Fields

Your rule MUST include:

- ✅ `title` - Clear, descriptive title
- ✅ `id` - Unique UUID (lowercase)
- ✅ `status` - Rule maturity level
- ✅ `description` - What the rule detects
- ✅ `logsource` - Where the logs come from
- ✅ `detection` - Detection logic with condition

### 5. Recommended Fields

Include these fields for quality rules:

- 📌 `author` - Your name or organization
- 📌 `date` - Creation date (YYYY/MM/DD format)
- 📌 `references` - Links to threat intelligence, documentation
- 📌 `tags` - MITRE ATT&CK tags and other categorizations
- 📌 `falsepositives` - Known false positive scenarios
- 📌 `level` - Severity level

## ✅ Validation

### Local Validation

Before submitting, validate your rule locally:

```bash
# Check YAML syntax
yamllint rules/your_category/your_rule.yml

# Validate against schema
python -c "
import yaml
import json
import jsonschema

with open('schema/sigma-schema.json', 'r') as f:
    schema = json.load(f)

with open('rules/your_category/your_rule.yml', 'r') as f:
    rule = yaml.safe_load(f)

jsonschema.validate(instance=rule, schema=schema)
print('✓ Rule is valid!')
"
```

### Automated Validation

When you create a pull request, GitHub Actions will automatically:

1. ✅ Validate YAML syntax
2. ✅ Check against Sigma schema
3. ✅ Check for duplicate IDs
4. ✅ Verify file naming conventions
5. ✅ Check rule quality (metadata completeness)

## 📤 Submitting Your Rule

### 1. Create a Branch

```bash
git checkout -b add-rule-suspicious-activity
```

### 2. Add Your Rule

```bash
git add rules/your_category/your_rule.yml
git commit -m "Add rule: Suspicious Activity Detection"
```

### 3. Push and Create PR

```bash
git push origin add-rule-suspicious-activity
# Create Pull Request via GitHub UI
```

### 4. PR Description Template

```markdown
## Rule Information

**Rule Name:** Descriptive Rule Title
**Category:** windows/linux/network/cloud/application/web
**Level:** informational/low/medium/high/critical
**Status:** stable/test/experimental

## Description

Brief description of what this rule detects and why it's important.

## Testing

- [ ] Rule validated against schema
- [ ] YAML syntax checked
- [ ] Tested against sample data (if applicable)
- [ ] False positives identified and documented

## References

- Link to threat intelligence
- Link to MITRE ATT&CK technique
- Any other relevant documentation

## Checklist

- [ ] Rule placed in correct category directory
- [ ] Unique UUID generated for `id` field
- [ ] All required fields included
- [ ] Date in YYYY/MM/DD format
- [ ] MITRE ATT&CK tags added (if applicable)
- [ ] False positives documented
- [ ] References included
- [ ] Rule tested locally
```

## 📏 Best Practices

### Rule Quality

1. **Be Specific**: Target specific malicious behavior, not just keywords
2. **Consider False Positives**: Document known false positive scenarios
3. **Use MITRE ATT&CK**: Tag rules with relevant ATT&CK techniques
4. **Add Context**: Include references to threat intelligence or analysis
5. **Test Thoroughly**: Test against both malicious and legitimate activity

### File Naming

- Use lowercase letters
- Use underscores or hyphens (no spaces)
- Use descriptive names: `suspicious_powershell_execution.yml`
- Use `.yml` or `.yaml` extension

### Detection Logic

```yaml
# Good: Specific and targeted
detection:
    selection:
        Image|endswith: '\cmd.exe'
        CommandLine|contains|all:
            - 'powershell'
            - '-encodedcommand'
    condition: selection

# Better: Include filters to reduce false positives
detection:
    selection:
        Image|endswith: '\cmd.exe'
        CommandLine|contains|all:
            - 'powershell'
            - '-encodedcommand'
    filter:
        User|startswith: 'SYSTEM'
    condition: selection and not filter
```

### Severity Levels

- **informational**: Interesting events for hunting
- **low**: Suspicious but low-risk activity
- **medium**: Suspicious activity requiring review
- **high**: Likely malicious activity
- **critical**: Confirmed malicious activity or critical threats

## 🐛 Reporting Issues

If you find issues with existing rules:

1. Open an issue describing the problem
2. Include the rule file name and path
3. Explain the issue (false positive, incorrect logic, etc.)
4. Provide suggestions for improvement if possible

## 💡 Rule Ideas

Looking for ideas? Consider creating rules for:

- Recent threat campaigns or APT activity
- Common attack techniques from MITRE ATT&CK
- Emerging threats from threat intelligence feeds
- Gaps in existing rule coverage
- Platform-specific threats (Windows, Linux, Cloud)

## 📚 Resources

- [Sigma Official Repository](https://github.com/SigmaHQ/sigma)
- [Sigma Specification](https://github.com/SigmaHQ/sigma-specification)
- [Sigma Rule Converter](https://github.com/SigmaHQ/sigma-cli)
- [MITRE ATT&CK](https://attack.mitre.org/)
- [Sigma Rule Examples](https://github.com/SigmaHQ/sigma/tree/master/rules)

## 📞 Getting Help

- Open an issue for questions
- Check existing rules for examples
- Refer to Sigma documentation
- Review the example rules in this repository

## 🙏 Thank You!

Your contributions help improve detection capabilities for the entire community. Thank you for taking the time to contribute quality Sigma rules!
