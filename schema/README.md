# Sigma Schema

This directory contains the JSON schema for validating Sigma detection rules.

## sigma-schema.json

The `sigma-schema.json` file is a JSON Schema (draft-07) that defines the structure and validation rules for Sigma detection rules. This schema is used by the GitHub Actions workflow to automatically validate all Sigma rules in the repository.

## Schema Validation

The schema validates:

- **Required fields**: title, id, status, description, logsource, detection
- **UUID format**: Ensures the `id` field is a valid UUID
- **Status values**: Validates status is one of: stable, test, experimental, deprecated, unsupported
- **Date format**: Ensures dates follow YYYY/MM/DD format
- **Level values**: Validates severity level is one of: informational, low, medium, high, critical
- **Detection logic**: Ensures a condition is present in the detection object

## Usage

The schema is automatically used by the GitHub Actions workflow during pull requests and commits to validate all Sigma rules in the `rules/` directory.

You can also validate rules manually using tools like `check-jsonschema` or `ajv-cli`:

```bash
# Using check-jsonschema
pip install check-jsonschema
check-jsonschema --schemafile schema/sigma-schema.json rules/**/*.yml

# Using ajv-cli (requires converting YAML to JSON first)
npm install -g ajv-cli
ajv validate -s schema/sigma-schema.json -d rule.json
```

## References

- [Sigma Official Repository](https://github.com/SigmaHQ/sigma)
- [Sigma Specification](https://github.com/SigmaHQ/sigma-specification)
- [JSON Schema Documentation](https://json-schema.org/)
