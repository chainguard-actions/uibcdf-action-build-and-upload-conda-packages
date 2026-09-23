<!-- markdownlint-disable -->

# Hardening Report: uibcdf--action-build-and-upload-conda-packages/v2.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **uibcdf--action-build-and-upload-conda-packages/v2.2.0** was hardened automatically. 2 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The 'Sanity checks on inputs' run: block in action.yml directly interpolates ${{ inputs.* }} expressions inside shell if-statements via contains() calls. For example: `if ${{ contains(inputs.conda_build_args, '--no-anaconda-upload') }};`, `if ${{ contains(inputs.conda_convert_args, '--output-folder') }};`, and multiple `if (${{ contains(inputs.anaconda_upload_args, ...) }})` expressions. These inputs.* values are substituted into the shell script by the Actions runner before the shell executes them, allowing an attacker-controlled input to inject arbitrary shell syntax.

Locations:

- `action.yml:113`
- `action.yml:118`
- `action.yml:123`
- `action.yml:128`
- `action.yml:128`
- `action.yml:133`
- `action.yml:133`
- `action.yml:138`
- `action.yml:143`

### script-injection (severity: high)

Sub-rule (a): The 'Promote and verify the exact package' run: block in promote/action.yml directly interpolates ${{ inputs.package-spec }}, ${{ inputs.expected-sha256 }}, ${{ inputs.from-label }}, and ${{ inputs.to-label }} as command-line arguments inside the shell script. For example: `--package-spec "${{ inputs.package-spec }}"`, `--expected-sha256 "${{ inputs.expected-sha256 }}"`, `--from-label "${{ inputs.from-label }}"`, `--to-label "${{ inputs.to-label }}"`. An attacker-controlled input value is substituted directly into the shell command string before the shell parses it, enabling command injection.

Locations:

- `promote/action.yml:43`
- `promote/action.yml:44`
- `promote/action.yml:45`
- `promote/action.yml:46`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed two script-injection findings:

1. hardened/action/action.yml ('Sanity checks on inputs' step): Replaced all ${{ contains(inputs.conda_build_args, ...) }}, ${{ contains(inputs.conda_convert_args, ...) }}, and ${{ contains(inputs.anaconda_upload_args, ...) }} expressions that were embedded directly in shell if-statements with bash glob pattern matching ([[ "$VAR" == *"substring"* ]]). Added CONDA_BUILD_ARGS, CONDA_CONVERT_ARGS, and ANACONDA_UPLOAD_ARGS to the step's env: block so the values are passed safely through the environment rather than interpolated into the shell script text.

2. hardened/action/promote/action.yml ('Promote and verify the exact package' step): Moved ${{ inputs.package-spec }}, ${{ inputs.expected-sha256 }}, ${{ inputs.from-label }}, and ${{ inputs.to-label }} from inline shell command interpolation into the step's env: block as PACKAGE_SPEC, EXPECTED_SHA256, FROM_LABEL, and TO_LABEL. Changed the run: block from a folded YAML scalar (>-) to a literal block (|) that references the environment variables with double-quoting.

### Iteration 2

**Fixes applied:** script-injection, github-env-injection

**Notes:**

Fixed three security findings in hardened/action/action.yml:

1. script-injection (Packages compilation, $CONDA_BUILD_ARGS): Replaced unquoted `$CONDA_BUILD_ARGS` expansion with an xargs-tokenized bash array `build_args`, populated via `printf '%s' "$CONDA_BUILD_ARGS" | xargs printf '%s\0'` with a NUL-delimited read loop. Expanded as `"${build_args[@]}"` to preserve argument boundaries and prevent shell metacharacter injection.

2. script-injection (Packages compilation, $CONDA_CONVERT_ARGS): Same fix with a `convert_args` array. The internally-constructed `$platforms_options` remains unquoted since it is built from a controlled set of platform names, not user input.

3. script-injection (Packages uploading, $ANACONDA_UPLOAD_ARGS): Same xargs-tokenization fix with an `upload_args` array.

4. github-env-injection (Write structured producer evidence): Added sanitization of `$output` and `$artifact_name` (which embed `$SUBJECT_JOB_KEY` from github.job and `$SUBJECT_MATRIX_INDEX` from inputs.evidence_matrix_index) using `printf '%s' "$VAR" | tr -d '\n\r'` before writing to $GITHUB_OUTPUT, preventing newline-based injection of additional key=value pairs.

