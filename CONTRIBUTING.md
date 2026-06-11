# 🤝 Contributing Guidelines

**Author:** Bilel Azaiez — Microsoft CSA, with AI-assisted development

Thank you for contributing to the Lumen framework. This guide explains how to add new content and maintain quality standards.

> **Tip:** You can use the [Prompt Generator Agent](agents/prompt-generator-agent.md) to generate new prompts from scratch, and the [Quality Reviewer Agent](agents/quality-reviewer-agent.md) to validate them before submitting.

---

## Adding a New Use Case

1. Copy the template from `templates/use-case-template.md`
2. Save it to `use-cases/` directory with a descriptive filename (kebab-case)
3. Complete all required fields:
   - Title and description
   - Role and persona
   - Key objectives
   - Prerequisites and data sources
   - Step-by-step workflow
   - Expected outcomes
   - Limitations and workarounds
   - Related prompts and playbooks
4. Validate that referenced prompts exist
5. Submit a PR with a clear description

**Example filename:** `investigate-sensitive-data-exposure.md`

---

## Adding a New Prompt

1. Copy the template from `templates/prompt-template.md`
2. Save it to the appropriate module folder:
   - `prompts/dlp/` for Data Loss Prevention
   - `prompts/irm/` for Information Rights Management
   - `prompts/dspm/` for Data Security Posture Management
3. Complete all required fields:
   - Unique prompt ID
   - Title and description
   - Objective
   - Persona (who should use this)
   - Context and prerequisites
   - Actual prompt text (the user message to Security Copilot)
   - Expected output type
   - Example output (if available)
   - Variations and advanced usage
   - Limitations
   - Related prompts
4. Test the prompt against real Security Copilot if possible
5. Document any SC quirks or workarounds discovered
6. Submit a PR

**Naming convention:** `[category]-[number].md` (e.g., `incident-response-001.md`)

---

## Adding a Playbook

1. Copy the template from `templates/playbook-template.md`
2. Save it to `playbooks/` with a clear name
3. Structure the playbook:
   - Title and scenario description
   - Role and assumed audience
   - Prerequisites and data access
   - Step-by-step instructions (reference specific prompts by ID)
   - Decision trees for common branches
   - Expected outcomes and success criteria
   - Escalation paths
   - Related use cases and prompts
   - Known issues and workarounds
4. Validate that all referenced prompts exist and are documented
5. Test the playbook end-to-end (ideally with real SC)
6. Include a "time estimate" for completion
7. Submit a PR

**Example filename:** `investigate-dlp-incident-playbook.md`

---

## Naming Conventions

- **Directories:** lowercase, hyphens, plural (e.g., `prompts/`, `playbooks/`, `use-cases/`)
- **Files:** lowercase with hyphens (e.g., `investigate-data-exfiltration.md`)
- **Prompt IDs:** Module-category-sequence (e.g., `dlp-incident-response-001`)
- **Branches:** feature/brief-description (e.g., `feature/add-dspm-prompts`)

---

## Quality Standards

### Tested > Theoretical
- Prompts should be tested against real Security Copilot when possible
- If untested, clearly label as "theoretical" and explain why
- Document actual SC behavior and quirks discovered during testing

### Honest About Limitations
- Every prompt and playbook should include a "Limitations" section
- Be clear about what SC cannot do or what requires manual steps
- Document known plugin gaps and workarounds

### Completeness
- All required template fields must be filled
- Don't leave "TODO" or placeholder text in submissions
- Include examples when available
- Cross-reference related content

### Clarity
- Use clear, concise language
- Assume the reader is familiar with Purview but may be new to Security Copilot
- Include screenshots or examples where helpful
- Define acronyms on first use

---

## Review Process

1. **Self-Review:** Check against this guide and quality standards
2. **Automated Checks:**
   - File naming conventions enforced
   - Required template fields verified
   - Cross-references validated
3. **Peer Review:**
   - At least one subject matter expert review
   - Focus on accuracy and utility
   - Feedback provided within 3 business days
4. **Merge:** Approved by repository maintainer

---

## Branch and PR Conventions

### Branch Naming
- `feature/add-[content-type]-[brief-description]`
- `fix/[issue-description]`
- `docs/[what-is-improved]`

Example: `feature/add-dspm-prompts-for-data-classification`

### PR Title Format
- Clear, concise description of what's being added
- Reference issue number if applicable

Example: `Add 8 DLP incident response prompts (#12)`

### PR Description Template
```
## What's being added
Brief summary of the content.

## Content checklist
- [ ] All template fields completed
- [ ] Tested against real SC (if applicable)
- [ ] Limitations documented
- [ ] Cross-references validated
- [ ] Follows naming conventions

## Related issues
Closes #[issue-number] (if applicable)

## Notes for reviewers
Any context reviewers should know.
```

---

## Maintenance and Updates

- Prompts and playbooks should be reviewed quarterly for SC product changes
- Update version numbers and dates when content changes
- Add a "Change log" section to track iterations
- Report breaks or limitations as GitHub issues

---

## Questions or Feedback?

- Open a GitHub issue for questions or suggestions
- Use issue template for bug reports or content gaps
- Tag appropriate maintainers for review
