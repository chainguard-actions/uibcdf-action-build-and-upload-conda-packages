<!-- markdownlint-disable -->

# Hardening Report: uibcdf--action-build-and-upload-conda-packages--promote/v2.2.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **uibcdf--action-build-and-upload-conda-packages--promote/v2.2.2** was hardened automatically. 5 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The composite action's `run:` block (sub-rule a) directly interpolates four `inputs.*` expressions inside the shell command string via YAML template substitution. Because `${{ }}` expansion occurs before the shell parses the string, an attacker-controlled input value containing shell metacharacters can break out of the quoted context and execute arbitrary commands. Offending lines:
  Line 45: `--package-spec "${{ inputs.package-spec }}"`
  Line 46: `--expected-sha256 "${{ inputs.expected-sha256 }}"`
  Line 47: `--from-label "${{ inputs.from-label }}"`
  Line 48: `--to-label "${{ inputs.to-label }}"`
Fix: move each value into an `env:` variable (e.g. `PACKAGE_SPEC: ${{ inputs.package-spec }}`) and reference it as `"$PACKAGE_SPEC"` in the shell script.

Locations:

- `action.yml:45`
- `action.yml:46`
- `action.yml:47`
- `action.yml:48`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.package-spec }}" appears directly in run: block of step "Promote and verify the exact package"; move to env: map

Locations:

- `action.yml:44`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.expected-sha256 }}" appears directly in run: block of step "Promote and verify the exact package"; move to env: map

Locations:

- `action.yml:44`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.from-label }}" appears directly in run: block of step "Promote and verify the exact package"; move to env: map

Locations:

- `action.yml:44`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.to-label }}" appears directly in run: block of step "Promote and verify the exact package"; move to env: map

Locations:

- `action.yml:44`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Moved all four ${{ inputs.* }} expressions (package-spec, expected-sha256, from-label, to-label) from the run: block into the env: block of the 'Promote and verify the exact package' step. The shell script now references them as plain environment variables ($PACKAGE_SPEC, $EXPECTED_SHA256, $FROM_LABEL, $TO_LABEL), eliminating the script injection risk.

