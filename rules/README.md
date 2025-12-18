# Sigma Rules

This directory contains Sigma detection rules organized by category.

## Directory Structure

- **windows/** - Detection rules for Windows systems
- **linux/** - Detection rules for Linux systems
- **network/** - Network-based detection rules
- **cloud/** - Cloud platform detection rules (AWS, Azure, GCP, etc.)
- **application/** - Application-specific detection rules
- **web/** - Web application and web server detection rules

## Sigma Rule Format

Sigma rules are written in YAML format and follow the official Sigma specification.

### Example Rule Structure

```yaml
title: Rule Title
id: unique-uuid-here
status: test
description: Description of what this rule detects
references:
    - https://reference-url.com
author: Your Name
date: 2024/01/01
modified: 2024/01/01
tags:
    - attack.technique_id
logsource:
    category: category_name
    product: product_name
detection:
    selection:
        field_name: value
    condition: selection
falsepositives:
    - Known false positive scenario
level: medium
```

## Contributing

When adding new Sigma rules:

1. Place the rule in the appropriate category directory
2. Ensure the rule follows the Sigma specification
3. Include proper metadata (title, id, author, date, etc.)
4. Test the rule before committing
5. Run the validation workflow to ensure compliance
