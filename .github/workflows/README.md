# GitHub Actions Workflows

This directory contains automated workflows for validating and managing Sigma rules.

## Workflows

### 1. validate-sigma-rules.yml

**Purpose**: Comprehensive validation of Sigma rules

**Triggers**:
- Push to main/master branch (for files in `rules/`, `schema/`, or workflow file)
- Pull requests to main/master branch
- Manual workflow dispatch

**Jobs**:

1. **validate-yaml**: Validates YAML syntax
   - Uses `yamllint` for linting
   - Uses `pyyaml` for parsing validation
   - Reports files with YAML errors

2. **validate-sigma-schema**: Validates rules against Sigma schema
   - Loads `schema/sigma-schema.json`
   - Validates each rule against the schema
   - Reports validation errors and warnings
   - Requires `validate-yaml` to pass first

3. **check-rule-quality**: Checks for best practices
   - Verifies presence of recommended fields (author, date, level, etc.)
   - Reports quality issues as warnings (non-blocking)
   - Runs in parallel with schema validation

**Artifacts**:
- `yaml-validation-log`: Results of YAML syntax validation
- `schema-validation-log`: Results of schema validation
- `quality-check-log`: Results of quality checks

### 2. sigma-ci.yml

**Purpose**: Continuous integration and rule management

**Triggers**:
- Push to main/master branch
- Pull requests to main/master branch
- Manual workflow dispatch

**Jobs**:

1. **count-rules**: Generates rule statistics
   - Counts rules by category
   - Adds summary to GitHub Actions summary page
   - Provides total rule count

2. **check-duplicates**: Detects duplicate rule IDs
   - Scans all rules for duplicate UUIDs
   - Fails if duplicates are found
   - Reports all duplicated IDs

3. **check-file-naming**: Validates file naming conventions
   - Checks for spaces in filenames
   - Checks for special characters
   - Reports naming issues as warnings (non-blocking)

4. **generate-index**: Creates rule index (main/master only)
   - Generates markdown index of all rules
   - Lists rules by category with metadata
   - Uploads as artifact
   - Only runs on push to main/master

**Artifacts**:
- `rules-index`: Generated markdown file with rule index

## Usage

### Automatic Validation

Workflows run automatically when you:
- Create a pull request
- Push changes to main/master
- Modify files in `rules/` directory

### Manual Trigger

You can manually trigger workflows:

1. Go to "Actions" tab in GitHub
2. Select the workflow you want to run
3. Click "Run workflow"
4. Select branch and click "Run workflow"

### Viewing Results

1. **In Pull Requests**: Check status checks at the bottom of the PR
2. **In Actions Tab**: View detailed logs and artifacts
3. **In Summary**: View rule statistics and summaries

### Downloading Artifacts

1. Go to the workflow run page
2. Scroll to "Artifacts" section
3. Download the artifact you want to review

## Requirements

Both workflows require:
- Python 3.11
- PyYAML
- jsonschema
- yamllint

These are automatically installed by the workflows.

## Customization

### Adding New Validation Checks

To add new validation checks:

1. Edit the appropriate workflow file
2. Add a new job or step
3. Use Python scripts for custom validation logic
4. Save results to artifacts if needed

### Modifying Validation Rules

To change validation behavior:

1. **YAML Linting**: Modify yamllint configuration in `validate-sigma-rules.yml`
2. **Schema Validation**: Update `schema/sigma-schema.json`
3. **Quality Checks**: Modify Python validation scripts in workflow files

## Troubleshooting

### Workflow Fails on YAML Syntax

- Check the validation log artifact
- Validate YAML locally with `yamllint`
- Ensure proper indentation and syntax

### Workflow Fails on Schema Validation

- Check the schema validation log artifact
- Ensure all required fields are present
- Verify field types match schema requirements
- Check UUID format (lowercase, proper format)
- Verify date format (YYYY/MM/DD)

### Workflow Fails on Duplicate IDs

- Review the error message for duplicate UUIDs
- Generate new UUIDs for conflicting rules
- Ensure each rule has a unique ID

## Best Practices

1. **Run Locally First**: Validate rules locally before pushing
2. **Check Logs**: Review workflow logs for detailed error messages
3. **Download Artifacts**: Download validation logs for offline review
4. **Iterative Fixes**: Fix issues one at a time and re-run validation
5. **Quality Checks**: Address quality warnings even though they're non-blocking

## Maintenance

### Updating Workflows

When updating workflows:

1. Test changes in a feature branch first
2. Ensure backwards compatibility with existing rules
3. Update this README if behavior changes
4. Consider impact on existing rules and PRs

### Updating Dependencies

Python dependencies are managed in the workflow files:

```yaml
- name: Install dependencies
  run: |
    python -m pip install --upgrade pip
    pip install pyyaml jsonschema yamllint
```

Update version pins if needed:

```yaml
pip install pyyaml==6.0 jsonschema==4.17.0 yamllint==1.30.0
```

## Support

For workflow issues:

1. Check the Actions tab for error logs
2. Review this documentation
3. Check existing issues in the repository
4. Open a new issue with workflow logs if needed
