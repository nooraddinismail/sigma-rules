# Quick Start Guide

Get started with the Sigma Rules repository in minutes!

## 🚀 Quick Setup

### 1. Clone the Repository

```bash
git clone https://github.com/nooraddinismail/sigma-rules.git
cd sigma-rules
```

### 2. Install Validation Tools

```bash
pip install pyyaml jsonschema yamllint
```

## 📝 Create Your First Rule

### Step 1: Generate a UUID

```bash
# On Linux/Mac
uuidgen | tr '[:upper:]' '[:lower:]'

# Using Python
python -c "import uuid; print(str(uuid.uuid4()))"
```

### Step 2: Create the Rule File

Choose the appropriate directory and create your rule:

```bash
# For Windows rules
touch rules/windows/my_rule.yml

# For Linux rules
touch rules/linux/my_rule.yml

# For Cloud rules
touch rules/cloud/my_rule.yml
```

### Step 3: Write Your Rule

Use this template:

```yaml
title: My Detection Rule
id: your-uuid-here
status: test
description: Description of what this rule detects
references:
    - https://example.com
author: Your Name
date: 2024/12/18
modified: 2024/12/18
tags:
    - attack.technique
logsource:
    category: process_creation
    product: windows
detection:
    selection:
        FieldName: value
    condition: selection
falsepositives:
    - Known false positive
level: medium
```

### Step 4: Validate Locally

```bash
# Check YAML syntax
yamllint rules/windows/my_rule.yml

# Validate against schema
python -c "
import yaml, json, jsonschema
with open('schema/sigma-schema.json') as f:
    schema = json.load(f)
with open('rules/windows/my_rule.yml') as f:
    rule = yaml.safe_load(f)
jsonschema.validate(instance=rule, schema=schema)
print('✓ Valid!')
"
```

### Step 5: Commit and Push

```bash
git checkout -b add-my-rule
git add rules/windows/my_rule.yml
git commit -m "Add: My Detection Rule"
git push origin add-my-rule
```

### Step 6: Create Pull Request

1. Go to GitHub repository
2. Click "Pull requests"
3. Click "New pull request"
4. Select your branch
5. Fill in the PR template
6. Submit for review

## ✅ Validation Checklist

Before submitting, ensure your rule has:

- [ ] Unique UUID (lowercase)
- [ ] Valid YAML syntax
- [ ] All required fields (title, id, status, description, logsource, detection)
- [ ] Author field
- [ ] Date field (YYYY/MM/DD format)
- [ ] MITRE ATT&CK tags (if applicable)
- [ ] False positives documented
- [ ] References included
- [ ] Appropriate severity level

## 🔍 Common Issues

### Invalid UUID Format

**Problem**: UUID must be lowercase and follow pattern: `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`

**Solution**:
```bash
python -c "import uuid; print(str(uuid.uuid4()))"
```

### YAML Syntax Error

**Problem**: Indentation or syntax issues

**Solution**:
- Use 4 spaces for indentation (not tabs)
- Check for missing colons
- Validate with `yamllint`

### Missing Required Fields

**Problem**: Schema validation fails

**Solution**: Ensure these fields are present:
- title
- id
- status
- description
- logsource
- detection (with condition)

### Date Format

**Problem**: Date must be YYYY/MM/DD

**Solution**:
```yaml
date: 2024/12/18
modified: 2024/12/18
```

## 📚 Next Steps

1. Read the [full README](../README.md)
2. Review [contribution guidelines](../CONTRIBUTING.md)
3. Check [example rules](../rules/windows/)
4. Explore [workflow documentation](../.github/workflows/README.md)

## 💡 Tips

- Start with the example rule as a template
- Use descriptive titles
- Tag with MITRE ATT&CK techniques
- Document false positives
- Include references to threat intelligence
- Test against sample data when possible

## 🆘 Getting Help

- Check the [CONTRIBUTING.md](../CONTRIBUTING.md) guide
- Review existing rules for examples
- Open an issue for questions
- Refer to [Sigma documentation](https://github.com/SigmaHQ/sigma)

## 🎯 Rule Quality Tips

### Good Rule Example

```yaml
title: Suspicious PowerShell Execution with Encoded Command
id: a1b2c3d4-e5f6-4a7b-8c9d-0e1f2a3b4c5d
status: test
description: >
    Detects PowerShell execution with encoded commands that may indicate malicious activity
    or attempt to evade detection
references:
    - https://attack.mitre.org/techniques/T1059/001/
author: Security Team
date: 2024/12/18
modified: 2024/12/18
tags:
    - attack.execution
    - attack.t1059.001
    - attack.defense_evasion
logsource:
    category: process_creation
    product: windows
detection:
    selection:
        Image|endswith: '\powershell.exe'
        CommandLine|contains:
            - '-encodedcommand'
            - '-enc'
    filter_legitimate:
        ParentImage|endswith: '\legitimate_tool.exe'
    condition: selection and not filter_legitimate
falsepositives:
    - Legitimate scripts from trusted sources
    - System management tools
level: high
fields:
    - CommandLine
    - User
    - ParentImage
    - ParentCommandLine
```

### What Makes This Good?

1. ✅ Clear, descriptive title
2. ✅ Unique UUID
3. ✅ Detailed description
4. ✅ References to threat intelligence
5. ✅ MITRE ATT&CK tags
6. ✅ Filters to reduce false positives
7. ✅ Documented false positive scenarios
8. ✅ Appropriate severity level
9. ✅ Relevant fields for analysis

Happy rule writing! 🎉
