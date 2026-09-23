<!-- markdownlint-disable -->

# Hardening Report: uibcdf--action-build-and-upload-conda-packages--promote/v2.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **uibcdf--action-build-and-upload-conda-packages--promote/v2.2.0** was hardened automatically. 5 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The `run:` block in the composite action step directly interpolates multiple `${{ inputs.* }}` expressions into the shell command string (sub-rule a). Specifically, `${{ inputs.package-spec }}`, `${{ inputs.expected-sha256 }}`, `${{ inputs.from-label }}`, and `${{ inputs.to-label }}` are substituted by the Actions template engine before the shell ever sees the command. An attacker-controlled input containing shell metacharacters (`;`, `|`, `$(...)`, backticks, etc.) could escape the quoted argument and execute arbitrary commands on the runner. The fix is to move each input into an `env:` variable and reference the env var (double-quoted) in the `run:` script instead:

```yaml
env:
  PACKAGE_SPEC: ${{ inputs.package-spec }}
  EXPECTED_SHA256: ${{ inputs.expected-sha256 }}
  FROM_LABEL: ${{ inputs.from-label }}
  TO_LABEL: ${{ inputs.to-label }}
run: |
  python "$GITHUB_ACTION_PATH/../scripts/promote_conda_package.py" \
    --package-spec "$PACKAGE_SPEC" \
    --expected-sha256 "$EXPECTED_SHA256" \
    --from-label "$FROM_LABEL" \
    --to-label "$TO_LABEL" \
    --output "$RUNNER_TEMP/conda-promotion-receipt.json"
```

Offending lines use: `--package-spec "${{ inputs.package-spec }}"`, `--expected-sha256 "${{ inputs.expected-sha256 }}"`, `--from-label "${{ inputs.from-label }}"`, `--to-label "${{ inputs.to-label }}"`.

Locations:

- `action.yml:47`

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

Moved all four ${{ inputs.* }} expressions (${{ inputs.package-spec }}, ${{ inputs.expected-sha256 }}, ${{ inputs.from-label }}, ${{ inputs.to-label }}) from the run: block into the step's env: block as PACKAGE_SPEC, EXPECTED_SHA256, FROM_LABEL, and TO_LABEL respectively. The shell script now references these as double-quoted environment variables, eliminating the shell injection risk. The run: scalar style was changed from >- (folded/strip) to | (literal block) to accommodate the multi-line script with backslash continuations.

