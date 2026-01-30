---
name: 'learn'
description: 'Professional knowledge synthesis and continuous learning for Compound stage. Loads context intelligently, synthesizes experience into reusable skills, and maintains clean domain knowledge without pollution.'
agent: 'agent'
argument-hint: 'What did you learn? (patterns, bugs, solutions, workflows)'
tools: ['read', 'edit', 'search', 'todo']
---

# Professional Knowledge Synthesis & Continuous Learning

Transform project experience into structured, reusable knowledge assets. This workflow implements professional software engineering best practices for knowledge management, context engineering, and skill creation during the Compound stage.

## Mission

Systematically capture lessons learned, bug patterns, workflow improvements, and technical insights, then synthesize them into well-organized domain knowledge and reusable skills. Prevent knowledge loss and redundancy while building a compound learning system that makes the team smarter with each project.

## Core Principles of Effective Context Engineering

### 1. **Precision Over Volume**
- Load ONLY what's relevant to the current learning synthesis
- Read INDEX files first, then selectively load specific documents
- Avoid dumping entire directories into context
- Use semantic search to find relevant prior knowledge

### 2. **Incremental Updates**
- Update core sections only, never rewrite entire documents
- Append new learnings to existing sections
- Merge similar patterns before adding new ones
- Maintain document coherence without pollution

### 3. **Domain Separation**
- Keep technical knowledge separate from experience knowledge
- Organize by domain (testing, build, architecture, etc.)
- Create new domain files when existing ones don't fit
- Cross-reference related domains without duplication

### 4. **Verification Before Storage**
- Validate that the learning is repeatable and useful
- Ensure it's not already documented elsewhere
- Confirm it provides actionable guidance
- Test that examples are correct and complete

## Scope & Preconditions

### Prerequisites
- **Workspace exists**: ${workspaceFolder}
- **User provides learning**: Pattern, bug fix, workflow improvement, or insight
- **Session context available**: Recent work that triggered the learning

### Directory Structure (Auto-created)

```
${workspaceFolder}/
├── context/
│   ├── README.md                    # Lightweight index (prioritize)
│   ├── tech/
│   │   ├── README.md               # Tech domain index
│   │   ├── testing-strategy.md     # TDD/E2E patterns
│   │   ├── build-system.md         # Build configurations
│   │   ├── architecture.md         # System architecture decisions
│   │   └── frameworks.md           # Framework-specific knowledge
│   └── experience/
│       ├── README.md               # Experience domain index
│       ├── error-patterns.md       # Common errors and solutions
│       ├── test-patterns.md        # Effective test patterns
│       ├── debugging-tactics.md    # Debugging strategies
│       └── workflow-improvements.md # Process optimizations
│
└── requirements/
    ├── INDEX.md                     # Requirement index (prioritize)
    ├── in-progress/                 # Active requirements
    └── completed/                   # Finalized requirements
```

### What This Workflow Covers
1. ✅ Intelligent context loading (index-first strategy)
2. ✅ Learning analysis and categorization
3. ✅ Domain knowledge synthesis
4. ✅ Skill creation for complex workflows
5. ✅ Incremental document updates (no pollution)
6. ✅ Cross-referencing and deduplication
7. ✅ Quality validation before storage

### What This Does NOT Cover
- ❌ Code implementation (use Work stage)
- ❌ Architecture design (use Plan stage)
- ❌ Code review (use Review stage)

## Workflow

Use the **todo list** to track progress through all learning stages.

### Phase 1: Intelligent Context Loading

**Objective**: Load ONLY relevant context for the current learning synthesis.

**Index-First Strategy**:

1. **Load Primary Indexes** (read these first, always)
   ```
   context/README.md
   requirements/INDEX.md
   ```

2. **Analyze Learning Domain** (from user input)
   - Identify if it's technical or experiential
   - Determine specific domain (testing, build, debugging, etc.)
   - Note any requirement references

3. **Selectively Load Domain Context** (only what's needed)
   
   For technical learnings:
   ```
   context/tech/README.md
   context/tech/<relevant-domain>.md  # Only the specific domain
   ```
   
   For experiential learnings:
   ```
   context/experience/README.md
   context/experience/<relevant-domain>.md  # Only the specific domain
   ```

4. **Load Related Requirements** (if applicable)
   - Only if learning relates to specific requirements
   - Load only the specific requirement file, not all requirements
   - Use INDEX.md to locate the correct file

**Context Loading Rules**:
- ❌ Never load entire directories
- ❌ Never load documents "just in case"
- ✅ Load index files to understand structure
- ✅ Load specific domain files based on learning category
- ✅ Use grep/search to find specific patterns before loading files
- ✅ Load related requirements only when explicitly referenced

### Phase 2: Learning Analysis & Validation

**Objective**: Understand what was learned and validate its value.

1. **Extract the Core Learning**
   - What was the problem or challenge?
   - What was the solution or insight?
   - Why does this matter for future work?
   - What context is needed to understand it?

2. **Validate Repeatability**
   - Is this a one-time fix or a reusable pattern?
   - Will this apply to future situations?
   - Are there generalizable principles?
   - Does this contradict existing knowledge?

3. **Check for Existing Knowledge**
   - Search loaded context for similar patterns
   - Identify if this enhances or replaces existing guidance
   - Find opportunities to merge or consolidate
   - Avoid redundancy with existing documentation

4. **Determine Documentation Level**
   
   | Learning Type | Action Required |
   |--------------|----------------|
   | Simple pattern/tip | Add to existing domain knowledge file |
   | Complex workflow | Create new skill with scripts/templates |
   | Architecture decision | Update architecture documentation + create ADR |
   | Error pattern | Add to error-patterns.md with solution |
   | Process improvement | Update workflow documentation |

### Phase 3: Domain Knowledge Synthesis

**Objective**: Update domain knowledge incrementally without pollution.

**For Simple Learnings** (adding to existing files):

1. **Read the Target File**
   - Load the specific domain file (e.g., `error-patterns.md`)
   - Identify the appropriate section
   - Check for similar existing entries

2. **Craft the Update** (incremental, not rewrite)
   ```markdown
   ## [Category Name]
   
   ### Existing Pattern 1
   [Existing content - DO NOT MODIFY unless merging]
   
   ### Existing Pattern 2
   [Existing content - DO NOT MODIFY unless merging]
   
   ### [NEW] Your New Pattern  ← ADD THIS ONLY
   **Problem**: [Concise problem description]
   **Solution**: [Actionable solution]
   **Example**: 
   ```code
   [Working example]
   ```
   **Context**: [When this applies]
   **References**: [Links to relevant requirements/docs]
   ```

3. **Apply Surgical Update**
   - Use `replace_string_in_file` to insert new section
   - Include sufficient context (3-5 lines before/after)
   - Update table of contents if file has one
   - Add cross-references to related patterns

4. **Update Domain Index**
   - Update `context/tech/README.md` or `context/experience/README.md`
   - Add one-line summary of new knowledge
   - Maintain index structure

**For Complex Learnings** (creating new domain files):

1. **Create Structured Domain File**
   
   Template for new domain knowledge file:
   
   ````markdown
   # {Domain Name}
   
   **Last Updated**: {Date}
   **Maintained By**: {Team/Person}
   
   ## Overview
   
   [One paragraph: what this domain covers and why it matters]
   
   ## Table of Contents
   
   - [Quick Reference](#quick-reference)
   - [Patterns](#patterns)
   - [Anti-Patterns](#anti-patterns)
   - [Examples](#examples)
   - [Related Resources](#related-resources)
   
   ## Quick Reference
   
   | Scenario | Solution | Link |
   |----------|----------|------|
   | [Common scenario 1] | [Quick solution] | [Link to detail] |
   
   ## Patterns
   
   ### Pattern 1: {Name}
   
   **Context**: [When to use this]
   **Solution**: [How to implement]
   **Example**:
   ```code
   [Working example]
   ```
   **Benefits**: [Why this works]
   **Gotchas**: [What to watch out for]
   
   ## Anti-Patterns
   
   ### Anti-Pattern 1: {Name}
   
   **Problem**: [What's wrong with this approach]
   **Why It Fails**: [Explanation]
   **Better Alternative**: [Link to correct pattern]
   
   ## Examples
   
   ### Example 1: {Scenario}
   
   [Complete, working example with context]
   
   ## Related Resources
   
   - [Related domain file 1](./other-domain.md)
   - [Relevant skill](../../.github/skills/skill-name/SKILL.md)
   - [External documentation](https://example.com)
   
   ---
   
   **Change Log**:
   | Date | Change | Author |
   |------|--------|--------|
   | {Date} | Initial creation | {Name} |
   ````

2. **Update Context README**
   - Add entry to appropriate section (tech or experience)
   - Link to new domain file
   - Update statistics

### Phase 4: Skill Creation (When Needed)

**Objective**: Create reusable skills for complex, repeatable workflows.

**When to Create a Skill** (not just documentation):

- ✅ Workflow requires multiple tools/commands
- ✅ Pattern includes scripts, templates, or resources
- ✅ Process has 3+ steps that need bundling
- ✅ Workflow is triggered by specific user requests
- ✅ Solution needs executable scripts or generators

**Skill Creation Process**:

1. **Read Skill Creation Instructions**
   ```
   .github/instructions/agent-skills.instructions.md
   .github/skills/skill-creator/SKILL.md
   ```

2. **Invoke Skill Creator**
   - Use skill-creator skill for guidance
   - Follow professional skill structure
   - Include proper frontmatter with triggers

3. **Skill Structure Requirements**
   
   ```
   .github/skills/{skill-name}/
   ├── SKILL.md                 # Main skill instructions
   ├── scripts/                 # Executable scripts (if needed)
   │   └── {script-name}.sh
   ├── templates/               # File templates (if needed)
   │   └── {template-name}.md
   ├── references/              # Reference materials (if needed)
   │   └── {reference-name}.md
   └── examples/                # Working examples (if needed)
       └── {example-name}/
   ```

4. **Skill Frontmatter Requirements**
   
   ```yaml
   ---
   name: {skill-name}
   description: '{What it does}. Use when {specific triggers/scenarios/keywords}. Handles {capabilities}'
   license: MIT
   ---
   ```
   
   **Critical**: Description MUST include:
   - What the skill does
   - When to use it (triggers)
   - Keywords users might mention
   - Specific capabilities

5. **Skill Body Structure**
   
   ```markdown
   # {Skill Name}
   
   ## Overview
   [One paragraph: purpose and value]
   
   ## When to Use
   [Specific scenarios, triggers, file types]
   
   ## Prerequisites
   [Required tools, files, context]
   
   ## Workflow
   
   ### Step 1: {Action}
   [Detailed instructions]
   
   ### Step 2: {Action}
   [Detailed instructions]
   
   ## Examples
   
   ### Example 1: {Scenario}
   ```bash
   # Command or code example
   ```
   
   ## Troubleshooting
   
   | Issue | Cause | Solution |
   |-------|-------|----------|
   | [Common issue] | [Root cause] | [How to fix] |
   
   ## Related Resources
   - [Domain knowledge](../../../context/tech/domain.md)
   - [Other skill](../other-skill/SKILL.md)
   ```

6. **Test the Skill**
   - Verify it activates on appropriate triggers
   - Test with example scenarios
   - Validate scripts execute correctly
   - Ensure templates generate properly

7. **Document in Context**
   - Reference the skill in relevant domain knowledge
   - Add to context README if significant
   - Cross-link from related documentation

### Phase 5: Requirement Linkage (When Applicable)

**Objective**: Link learnings to specific requirements for traceability.

**Only if learning relates to a requirement**:

1. **Identify Related Requirements**
   - Use requirements/INDEX.md to find relevant requirements
   - Load only the specific requirement files
   - Check if learning impacts requirement

2. **Update Requirement Documentation**
   
   Add learning to relevant requirement file section:
   
   ```markdown
   ## Implementation Learnings
   
   ### {Learning Title} - {Date}
   
   **Context**: [What was being implemented]
   **Discovery**: [What was learned]
   **Impact**: [How this affects the requirement]
   **Action Taken**: [Changes made to code/docs/tests]
   **Knowledge Captured**: [Link to domain knowledge or skill]
   ```

3. **Update Requirement Status** (if needed)
   - Mark as updated in INDEX.md
   - Add changelog entry
   - Update completion percentage if applicable

### Phase 6: Quality Validation & Cleanup

**Objective**: Ensure knowledge is high-quality and accessible.

**Validation Checklist**:

- [ ] **Clarity**: Is the learning clearly explained?
- [ ] **Actionability**: Can someone use this guidance directly?
- [ ] **Examples**: Are there working, tested examples?
- [ ] **Context**: Is it clear when/why to apply this?
- [ ] **Non-Redundancy**: Doesn't duplicate existing knowledge
- [ ] **Linkage**: Properly cross-referenced with related docs
- [ ] **Scannability**: Easy to find and quick to understand
- [ ] **Minimal Pollution**: Only added/updated what's necessary

**Cleanup Tasks**:

1. **Remove Redundancy**
   - If new learning overlaps existing knowledge, merge them
   - Consolidate similar patterns into one comprehensive pattern
   - Remove outdated information that's been superseded

2. **Update Cross-References**
   - Add bidirectional links between related knowledge
   - Update "Related Resources" sections
   - Link from index files to new content

3. **Verify File Integrity**
   - Check markdown formatting is correct
   - Verify code examples syntax
   - Test that links resolve correctly
   - Ensure tables are properly formatted

4. **Update Metrics** (in README files)
   - Increment pattern counts
   - Update "Last Updated" dates
   - Add to change logs

### Phase 7: Synthesis Summary

**Objective**: Provide clear summary of what was learned and where it's stored.

**Generate Summary**:

```markdown
## Learning Synthesis Complete

### What Was Learned
[Concise summary of the core learning]

### Where It's Stored
- **Domain Knowledge**: [Link to updated domain file(s)]
- **Skills Created**: [Links to new skills, if any]
- **Requirements Updated**: [Links to requirement files, if any]

### Key Takeaways
1. [Actionable takeaway 1]
2. [Actionable takeaway 2]
3. [Actionable takeaway 3]

### Recommended Next Steps
- [Suggestion for applying this learning]
- [Related patterns to explore]
- [Potential improvements to consider]

### Quick Access
Use these commands/searches to access this knowledge:
- Search: `{relevant keywords}`
- Skill: `{skill-name}` (if created)
- Document: `context/{domain}/{file}.md`
```

## Output Expectations

### Primary Outputs

1. **Updated Domain Knowledge Files**
   - Incremental updates to existing files
   - OR new domain files following template
   - Clean, non-polluted additions

2. **New Skills** (when warranted)
   - Complete skill directory with SKILL.md
   - Scripts, templates, references as needed
   - Proper frontmatter for discoverability

3. **Updated Index Files**
   - context/README.md
   - context/{tech|experience}/README.md
   - requirements/INDEX.md (if applicable)

4. **Synthesis Summary**
   - Clear documentation of what was learned
   - Locations of all updates
   - Recommended usage

### Success Criteria

- ✅ Learning is captured in appropriate domain
- ✅ Only relevant context was loaded (no waste)
- ✅ Updates are surgical, not wholesale rewrites
- ✅ Skills created for complex workflows
- ✅ Cross-references are bidirectional
- ✅ Examples are tested and working
- ✅ Documentation is scannable and actionable

### Failure Conditions

- ❌ Loaded too much unnecessary context
- ❌ Created redundant documentation
- ❌ Polluted existing files with full rewrites
- ❌ Failed to create skill for complex workflow
- ❌ Learning is too vague to be actionable
- ❌ Missing links to related resources

## Quality Assurance

### Context Loading Validation

```bash
# Verify minimal context loading
# Should load < 10 files for typical learning synthesis

# Check index files exist
ls -la context/README.md
ls -la requirements/INDEX.md

# Verify domain structure
tree context/
```

### Documentation Quality Check

**10/10 Quality Criteria**:

1. **Zero Knowledge Loss**: Every detail captured
2. **Maximum Scannability**: Clear headings, tables, bullets
3. **Minimal Redundancy**: No duplicate content
4. **Actionable Examples**: Working, tested code
5. **Clear Context**: When/why to use
6. **Proper Cross-refs**: Bidirectional links
7. **Surgical Updates**: No wholesale rewrites
8. **Domain Separation**: Right content in right file
9. **Progressive Loading**: Index → specific docs
10. **Verified Accuracy**: Examples tested

### Skill Quality Check (if created)

- [ ] Frontmatter has descriptive triggers
- [ ] Description includes what/when/keywords
- [ ] Workflow is step-by-step
- [ ] Examples are complete and tested
- [ ] Scripts execute successfully
- [ ] Templates generate correctly
- [ ] Activates on appropriate user prompts

## Integration with Other Stages

### From Review Stage
- Security findings → experience/error-patterns.md
- Refactoring learnings → tech/architecture.md
- Code quality insights → experience/test-patterns.md

### From Work Stage
- Build errors → experience/error-patterns.md
- Test strategies → tech/testing-strategy.md
- Framework patterns → tech/frameworks.md

### From Plan Stage
- Architecture decisions → tech/architecture.md
- Requirement learnings → link to specific requirements
- Design patterns → tech/architecture.md

### To Future Work
- Skills available for reuse
- Patterns prevent repeated mistakes
- Context enables faster onboarding
- Knowledge compounds over time

## Advanced Patterns

### Session Reflection Pattern

For end-of-session synthesis:

1. **Review session context**
2. **Identify 3-5 key learnings**
3. **Synthesize each learning individually**
4. **Generate session summary document** (optional)
   ```
   context/sessions/YYYY-MM-DD-session-summary.md
   ```

### Continuous Learning Loop

Integrate with session hooks:

1. **SessionStart Hook**: Load relevant prior learnings
2. **Work Phase**: Accumulate learnings during work
3. **Stop Hook**: Trigger synthesis at session end
4. **Next Session**: Prior learnings inform new work

### Knowledge Compounding

Build reusable patterns:

```
Learning → Domain Knowledge → Skill → Best Practice
   ↑                                      ↓
   └──────── Continuous Improvement ──────┘
```

## Examples

### Example 1: Error Pattern Learning

**User Input**: 
```
/learn We kept getting "EADDRINUSE" errors in tests because 
Jest wasn't cleaning up server instances between test files
```

**Context Loading**:
- ✅ Load: context/README.md
- ✅ Load: context/experience/README.md
- ✅ Load: context/experience/error-patterns.md
- ❌ Skip: All other files (not relevant)

**Update Applied** (surgical insertion):
```markdown
### EADDRINUSE - Port Already in Use

**Problem**: Tests fail with "EADDRINUSE" error
**Root Cause**: Server instances not cleaned up between test files
**Solution**: Add global teardown in Jest configuration

```javascript
// jest.config.js
module.exports = {
  globalTeardown: './tests/teardown.js'
}

// tests/teardown.js
module.exports = async () => {
  // Close all server instances
  if (global.__SERVER__) {
    await global.__SERVER__.close()
  }
}
```

**Context**: Affects integration tests that spawn servers
**Related**: [Testing Strategy](../tech/testing-strategy.md#integration-tests)
```

### Example 2: Complex Workflow → Skill Creation

**User Input**:
```
/learn We have a complex process for creating ADRs that involves 
templates, numbering, and linking. Should be automated.
```

**Actions Taken**:
1. Loaded context/README.md and context/tech/README.md
2. Determined this needs a skill (multi-step workflow)
3. Created `.github/skills/adr-creator/SKILL.md`
4. Added template in `scripts/adr-template.md`
5. Updated context/tech/architecture.md with reference
6. Tested skill activation with "create an ADR"

**Skill Created**:
```
.github/skills/adr-creator/
├── SKILL.md (workflow for ADR creation)
├── templates/
│   └── adr-template.md
└── scripts/
    └── generate-adr-number.sh
```

## Troubleshooting

| Issue | Cause | Solution |
|-------|-------|----------|
| Loaded too many files | Didn't check indexes first | Always load README/INDEX first, then selectively |
| Duplicate content | Didn't search existing knowledge | Always search before adding new content |
| Polluted documents | Full file rewrite instead of surgical update | Use replace_string_in_file with precise context |
| Skill not activating | Poor description triggers | Add specific keywords and scenarios to description |
| Learning too vague | Insufficient analysis | Ask clarifying questions in Phase 2 |
| Missing cross-references | Didn't check related docs | Review related domain files and add links |

## Related Resources

### Skills Referenced
- **skill-creator**: Guide for creating effective skills
- **make-skill-template**: Scaffold new skills

### Instructions Referenced
- **agent-skills.instructions.md**: Skill creation guidelines
- **memory-bank.instructions.md**: Memory management system

### Prompts Referenced
- **remember**: Quick memory capture (global/workspace)
- **memory-merger**: Consolidate mature learnings

---

**Version**: 1.0  
**Last Updated**: 2026-01-30  
**Maintained By**: GitHub Copilot Code Repository  
**Based On**: [The Longform Guide to Everything Claude Code](https://github.com/affaan-m/everything-claude-code/blob/main/the-longform-guide.md)
