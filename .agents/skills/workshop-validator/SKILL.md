---
name: workshop-validator
description: Validate the workshop content from the user perspective
---

# Workshop Validator Skill

This skill provides automated validation functions for workshop exercises, focusing on technical checks and quality assurance.

## Purpose

Validate workshop exercises from an intermediate user's perspective, identifying:
- Time feasibility issues
- Content clarity problems
- Technical blockers
- Missing prerequisites
- Broken links and references

## Usage

Invoke this skill when you need to:
1. **Validate a single exercise**: Check one exercise file for completeness
2. **Validate all exercises**: Systematic review of entire workshop
3. **Check specific aspects**: Links, commands, time estimates, or prerequisites only
4. **Prepare workshop updates**: Identify issues before content revision

## Skill Functions

### 1. Link and File Reference Validation

**Function**: `validate_references(exercise_file)`

Checks:
- External URLs are accessible
- Internal file paths exist in workspace
- Screenshot references point to actual image files
- Cross-references to other exercises are valid

**Output**: List of broken links/references with locations

### 2. Command Syntax Verification

**Function**: `validate_commands(exercise_file)`

Checks:
- Bash/PowerShell commands have correct syntax
- File paths use correct separators for OS
- Commands reference existing files/directories
- Environment variables are properly formatted

**Output**: List of command issues with corrections

### 3. Time Estimation Analysis

**Function**: `estimate_time(exercise_file)`

Analyzes:
- Step complexity (reading, typing, waiting)
- Tool installation overhead
- Expected troubleshooting time
- Learning curve for new concepts

**Formula**:

```
Time = (Steps × Average Time per Step) + Overhead + Troubleshooting
```

Where:
- Steps: Number of distinct actions in the exercise
- Average Time per Step: Time taken for each action
- Overhead: Time for tool setup and configuration
- Troubleshooting: Time for fixing issues encountered

## Example Exercises

- **Exercise 1**: Basic file operations and directory structure
- **Exercise 2**: Scripting with loops and conditionals
- **Exercise 3**: Using external tools and libraries

## Skill Status

- **Active**: Yes
- **Maintenance**: Regular
- **Updates**: Quarterly

## Support

For support or feedback, contact the skill developer or the workshop facilitator.