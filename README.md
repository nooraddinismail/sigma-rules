# Sigma Rules Repository

This repository contains a collection of Sigma detection rules for security monitoring and threat detection. Sigma is a generic and open signature format that allows you to describe relevant log events in a straightforward manner.

## 📁 Repository Structure

```
sigma-rules/
├── rules/              # Sigma detection rules organized by category
│   ├── windows/        # Windows-specific detection rules
│   ├── linux/          # Linux-specific detection rules
│   ├── network/        # Network-based detection rules
│   ├── cloud/          # Cloud platform detection rules (AWS, Azure, GCP, etc.)
│   ├── application/    # Application-specific detection rules
│   └── web/            # Web application and web server detection rules
├── schema/             # Sigma rule schema for validation
│   ├── sigma-schema.json  # JSON Schema for rule validation
│   └── README.md       # Schema documentation
└── .github/
    └── workflows/      # GitHub Actions workflows
        ├── validate-sigma-rules.yml  # Validation workflow
        └── sigma-ci.yml              # CI/CD workflow
```

## 🚀 Features

- **Organized Structure**: Rules are categorized by platform and type for easy navigation
- **Automated Validation**: GitHub Actions automatically validate rules on push and pull requests
- **Schema Validation**: All rules are validated against the official Sigma schema
- **Quality Checks**: Automated checks for rule quality and best practices
- **Duplicate Detection**: Automatic detection of duplicate rule IDs
- **Rule Statistics**: Automated generation of rule statistics and indices

## 📝 Sigma Rule Format

Sigma rules are written in YAML format. Here's a basic example:

```yaml
title: Suspicious PowerShell Command
id: 12345678-1234-1234-1234-123456789abc
status: test
description: Detects suspicious PowerShell commands that may indicate malicious activity
references:
    - https://example.com/threat-analysis
author: Your Name
date: 2024/01/01
modified: 2024/01/01
tags:
    - attack.execution
    - attack.t1059.001
logsource:
    category: process_creation
    product: windows
detection:
    selection:
        Image|endswith: '\powershell.exe'
        CommandLine|contains:
            - '-enc'
            - '-nop'
            - 'bypass'
    condition: selection
falsepositives:
    - Legitimate administrative scripts
level: medium
```

## 🔧 Contributing

### Adding New Rules

1. Create your Sigma rule in the appropriate category directory
2. Ensure the rule includes all required fields:
   - `title`, `id`, `status`, `description`, `logsource`, `detection`
3. Follow the Sigma specification and naming conventions
4. Generate a unique UUID for the `id` field
5. Add appropriate metadata (author, date, tags, etc.)
6. Submit a pull request

### Rule Requirements

- **Valid YAML syntax**: Rules must be valid YAML files
- **Unique ID**: Each rule must have a unique UUID
- **Required fields**: title, id, status, description, logsource, detection
- **Proper categorization**: Place rules in the appropriate directory
- **Quality metadata**: Include author, date, level, falsepositives, and references

### Validation

All rules are automatically validated when you:
- Create a pull request
- Push to main/master branch
- Manually trigger the workflow

The validation workflow checks:
- ✅ YAML syntax
- ✅ Schema compliance
- ✅ Duplicate IDs
- ✅ Rule quality (metadata completeness)
- ✅ File naming conventions

## 🛠️ Local Development

### Prerequisites

```bash
# Install Python dependencies for validation
pip install pyyaml jsonschema yamllint
```

### Validating Rules Locally

```bash
# Validate YAML syntax
yamllint rules/

# Validate against schema (requires Python)
python -c "
import yaml
import json
import jsonschema
from pathlib import Path

with open('schema/sigma-schema.json', 'r') as f:
    schema = json.load(f)

for rule_file in Path('rules').rglob('*.yml'):
    with open(rule_file, 'r') as f:
        rule = yaml.safe_load(f)
    jsonschema.validate(instance=rule, schema=schema)
    print(f'✓ {rule_file}')
"
```

## 📚 Resources

- [Sigma Official Repository](https://github.com/SigmaHQ/sigma)
- [Sigma Specification](https://github.com/SigmaHQ/sigma-specification)
- [Sigma HQ Community](https://github.com/SigmaHQ)
- [MITRE ATT&CK Framework](https://attack.mitre.org/)

## 🔐 Rule Levels

Sigma rules use the following severity levels:

- **informational**: Informational events
- **low**: Low severity findings
- **medium**: Medium severity threats
- **high**: High severity threats
- **critical**: Critical threats requiring immediate attention

## 📊 Rule Status

- **stable**: Production-ready rules
- **test**: Rules in testing phase
- **experimental**: Experimental rules, may produce false positives
- **deprecated**: Deprecated rules, should not be used
- **unsupported**: Unsupported rules

## 📖 License

This repository is for storing and managing Sigma detection rules. Please ensure you have the right to use and distribute any rules you add.

## 🤝 Support

For questions or issues:
- Open an issue in this repository
- Refer to the [Sigma documentation](https://github.com/SigmaHQ/sigma)
- Join the Sigma community discussions
