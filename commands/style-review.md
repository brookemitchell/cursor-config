---
name: style-review
description: Review Go and SQL code style and formatting between current branch and target branch, or review a specific PR
arguments:
  - name: target
    description: The target branch name to compare against (e.g., main, develop), or a GitHub PR URL (e.g., https://github.com/IDEXX/vello-forms/pull/228)
    required: false
---

# Style Review Command

## What this command does

1. **List changed files** - Compares current branch against the target branch (e.g., main) to identify all modified Go (`.go`), SQL (`.sql`), and documentation (`.md`) files

   - Excludes any changes in the `.cursor` folder
   - Only reviews Go, SQL, and documentation files

2. **Create review log** - Generates a markdown file (`style-review-{timestamp}.md`) that records:

   - Target branch being compared against
   - List of all Go, SQL, and documentation files that have been updated
   - Review progress tracker with checkboxes

3. **Review all changed files** - Reviews all Go, SQL, and documentation style across changed files:
   - Provides detailed style review feedback for each file
   - Focuses on code style and documentation standards, not logic or functionality
   - **Reviews follow guidelines in `.cursor/rules/go.mdc` (VetSoft Go Style Guide), `.cursor/rules/docs.mdc` (Feature Documentation Rule), and `.github/copilot-instructions.md`**
   - **Marks reviewed files as done (✓) in the review log** after completing the review
   - The command automatically skips already reviewed files by checking the markdown log

## Style Review Focus Areas

### Go Code Style

#### Naming Conventions

- Package names (short, lowercase, single-word, no `util`/`common`/`helpers`)
- Variable and function names (MixedCaps, never snake_case except tests)
- Interface names (small, end in "-er" for single-method)
- No stuttering (e.g., `http.HTTPServer` → `http.Server`)
- Domain terminology (no `foo`, `bar`, `tmp`, `data`)
- Realistic veterinary domain names (`patientWeight`, `medicationDosage`, not generic terms)
- Constants explain role, not value (`MaxPatientWeight` not `TwoHundred`)

#### Documentation

- Godoc comments for ALL exported types/functions/methods
- Comments start with the item name
- Explain "why" not obvious "what"
- No temporal references ("recently", "improved", "new", "enhanced")
- No commented-out code (use git history)
- File header comments (2-line explanation)

#### Code Clarity

- Obvious intent over clever one-liners
- Named intermediate steps for complex logic
- Comments preserved unless actively false
- Simplicity over abstraction (least mechanism)

#### Function Design

- Single responsibility per function
- Context as parameter (NEVER in structs)
- Consistent receivers throughout a type (1-2 letter abbreviations, e.g., `p` for `Patient`)
- Proper parameter order: ctx first, domain objects, options, error last
- Early returns to reduce nesting
- Max 3 levels of nesting

#### Error Handling Style

- VetSoft pattern: `fmt.Errorf("context: %v", err)`
- Provide actionable context at each layer
- Check errors immediately
- Early returns for error cases

#### Imports

- Import organization: stdlib, project, third-party (alphabetical within each)
- No unnecessary imports

#### Consistency

- Match surrounding code style
- Follow established patterns in the package
- MixedCaps consistently throughout
- No unnecessary changes outside current task scope

#### Anti-Patterns to Flag

- Speculative abstractions (interfaces with one implementation)
- Context stored in structs
- Inconsistent receiver names
- snake_case naming (except test filenames)
- Missing godoc on exports
- Temporal or "new"/"improved" naming
- Commented-out code
- Generic package names (`utils`, `common`, `models`, `helpers`)
- Global state without justification

### SQL Code Style

#### SQL Naming Conventions

- Table names: lowercase, plural, snake_case (e.g., `patients`, `medical_records`)
- Column names: lowercase, snake_case (e.g., `patient_id`, `appointment_date`)
- Alias clarity: use meaningful aliases, not single letters
- Consistency with Go domain terms where applicable

#### Query Formatting

- Keywords uppercase: SELECT, FROM, WHERE, JOIN, GROUP BY, ORDER BY
- One clause per line for readability
- Proper indentation for nested queries
- Consistent whitespace

#### Comments

- SQL comments explain non-obvious logic or business rules
- Use `--` for single-line comments
- Use `/* */` for multi-line comments where appropriate

#### SQL Anti-Patterns to Flag

- SELECT \* (be explicit about columns)
- Inconsistent naming across tables/columns
- Poor query indentation
- Missing or unclear aliases in joins
- Unused columns in queries

### Documentation Style

#### Structure and Content

- Presence of all required sections: Overview, Getting Started, Examples & Tutorial, Best Practices, Architecture, Troubleshooting
- Clear section hierarchy and organization
- Consistent terminology throughout
- Problem statement clearly articulated
- Relationship to project goals explained

#### Clarity and Language

- Clear, concise language for veterinary software developers
- No unnecessary jargon
- Technical concepts explained adequately
- Consistent terminology across all sections
- Examples use realistic veterinary domain concepts

#### Examples and Code

- All code examples are tested and functional
- Examples progress from simple to complex
- Expected outputs are documented
- Configuration options are explained
- Code snippets are complete and self-contained

#### Links and References

- All internal links are valid
- External links are current and functional
- References to other documentation are accurate
- No broken or stale links

#### Diagrams and Visuals

- Mermaid diagrams render correctly
- Diagrams accurately represent architecture/flow
- Data flow is clearly illustrated
- Component relationships are evident
- Diagrams match current implementation

#### Metadata

- Date implemented is present: `**Date Implemented:** [YYYY-MM-DD]`
- Feature scope is briefly described
- File naming follows convention: `docs/[feature_name].md`

#### Documentation Anti-Patterns to Flag

- Missing required sections
- Incomplete examples
- Untested code snippets
- Broken or invalid links
- Broken Mermaid diagrams
- Generic or unclear language
- Inconsistent terminology
- Missing metadata (date, scope)
- Outdated information
- Examples that don't match current code

## Parameters

- `target` (optional): The target branch to compare against (e.g., `main`, `develop`), or a GitHub PR URL (e.g., `https://github.com/IDEXX/vello-forms/pull/228`)
- If no argument is provided, reviews against the current branch

## Example workflow

```bash
# Review all Go and SQL style changes against main branch
/style-review main

# Review style against develop branch
/style-review develop

# Review style on a specific PR (will post feedback as comments)
/style-review https://github.com/IDEXX/vello-forms/pull/228

# Review current branch
/style-review
```

## Notes

- Changes in `.cursor/` folder are automatically ignored
- If a GitHub PR URL is provided, feedback will be automatically posted as comments on the PR
- If a branch name is provided, review log file is saved in the workspace root with format `style-review-{timestamp}.md`
- If no argument is provided, review log file is saved using the current branch name
- Reviews ALL changed Go, SQL, and documentation files in a single pass (no line limits)
- Reviewed files are marked with `[x]` checkboxes in the review log (when using branch comparison)
- You can continue reviews across multiple sessions by running the command again
- The command reads the existing review log to skip already completed files (when using branch comparison)
- **Focus is on style only** - logic, functionality, and correctness are NOT reviewed here
- For full code review including logic, use `/code-review` instead
- **Reference `.cursor/rules/go.mdc` for detailed VetSoft Go Style Guide standards**
- **Reference `.cursor/rules/docs.mdc` for detailed Feature Documentation Rule standards**
