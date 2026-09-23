<!-- markdownlint-disable -->

# Hardening Report: uibcdf--action-build-and-upload-conda-packages/v2.2.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **uibcdf--action-build-and-upload-conda-packages/v2.2.2** was hardened automatically. 5 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a): The 'Sanity checks on inputs' step in action.yml directly interpolates ${{ contains(inputs.conda_build_args, ...) }}, ${{ contains(inputs.conda_convert_args, ...) }}, and ${{ contains(inputs.anaconda_upload_args, ...) }} inside the run: shell script. Even though contains() returns true/false, the expressions undergo YAML template substitution before the shell sees them, making this a script-injection risk. Offending lines include:
  if ${{ contains(inputs.conda_build_args, '--no-anaconda-upload') }}; then
  if ${{ contains(inputs.conda_build_args, '--output-folder') }}; then
  if ${{ contains(inputs.conda_convert_args, '--output-folder') }}; then
  if (${{ contains(inputs.anaconda_upload_args, '--label') }} || ${{ contains(inputs.anaconda_upload_args, '-l') }}) ...
  if (${{ contains(inputs.anaconda_upload_args, '--user') }} || ${{ contains(inputs.anaconda_upload_args, '-u') }}) ...
  if ${{ contains(inputs.anaconda_upload_args, '--token') }} ...
  if ${{ contains(inputs.anaconda_upload_args, '--force') }} ...

Locations:

- `action.yml:119`
- `action.yml:124`
- `action.yml:130`
- `action.yml:137`
- `action.yml:143`
- `action.yml:149`
- `action.yml:154`

### script-injection (severity: high)

Rule (b): The 'Packages compilation' step in action.yml uses $CONDA_BUILD_ARGS and $CONDA_CONVERT_ARGS unquoted in shell commands. These env vars are sourced from inputs.conda_build_args and inputs.conda_convert_args respectively (workflow-controllable). Unquoted expansion allows shell metacharacter injection. The comment in the code acknowledges the unquoting is intentional for word-splitting, but this does not eliminate the injection risk. Offending lines:
  conda "$build_function" . --no-anaconda-upload --output-folder "$out_dir" $CONDA_BUILD_ARGS
  conda convert "$host_package" -o "$out_dir" $CONDA_CONVERT_ARGS $platforms_options

Locations:

- `action.yml:185`
- `action.yml:210`

### script-injection (severity: high)

Rule (b): The 'Packages uploading' step in action.yml uses $ANACONDA_UPLOAD_ARGS unquoted in the anaconda upload command. This env var is sourced from inputs.anaconda_upload_args (workflow-controllable). Unquoted expansion allows shell metacharacter injection. Offending line:
  if anaconda upload --user "$ANACONDA_USER" $force --label "$label" $ANACONDA_UPLOAD_ARGS "$package_path"; then

Locations:

- `action.yml:243`

### script-injection (severity: high)

Rule (a): The 'Promote and verify the exact package' step in promote/action.yml directly interpolates ${{ inputs.package-spec }}, ${{ inputs.expected-sha256 }}, ${{ inputs.from-label }}, and ${{ inputs.to-label }} inside the run: block as CLI arguments. These are attacker-controlled values that undergo YAML template substitution before the shell executes the command, enabling script injection. Offending lines:
  --package-spec "${{ inputs.package-spec }}"
  --expected-sha256 "${{ inputs.expected-sha256 }}"
  --from-label "${{ inputs.from-label }}"
  --to-label "${{ inputs.to-label }}"

Locations:

- `promote/action.yml:44`
- `promote/action.yml:45`
- `promote/action.yml:46`
- `promote/action.yml:47`

### github-env-injection (severity: high)

The 'Write structured producer evidence' step in action.yml writes values derived from github.* context variables (github.run_id, github.run_attempt, github.job, github.sha, github.repository) and inputs.evidence_matrix_index to $GITHUB_OUTPUT without the required sanitization step (printf '%s' ... | tr -d '\n\r'). The variables $output and $artifact_name are constructed from these unsanitized github context values and then written directly:
  echo "path=$output" >> "$GITHUB_OUTPUT"
  echo "artifact_name=$artifact_name" >> "$GITHUB_OUTPUT"
A newline injected into github.job or inputs.evidence_matrix_index could poison subsequent GITHUB_OUTPUT entries.

Locations:

- `action.yml:296`
- `action.yml:297`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, github-env-injection

**Notes:**

Fixed all 5 findings across action.yml and promote/action.yml:

1. action.yml 'Sanity checks on inputs': Replaced all ${{ contains(inputs.*) }} expressions with shell-based [[ "$VAR" == *"..."* ]] pattern matching. Added CONDA_BUILD_ARGS, CONDA_CONVERT_ARGS, and ANACONDA_UPLOAD_ARGS to the step's env: block so the values are passed as environment variables, never interpolated into the shell script via YAML template substitution.

2. action.yml 'Packages compilation': Tokenized $CONDA_BUILD_ARGS and $CONDA_CONVERT_ARGS into bash arrays using the xargs/printf NUL-delimited pattern (guarded by [ -n "$VAR" ] to prevent empty-input issues). Arrays are expanded with "${array[@]+...}" to safely pass multiple arguments.

3. action.yml 'Packages uploading': Same xargs tokenization applied to $ANACONDA_UPLOAD_ARGS before use in the anaconda upload command.

4. promote/action.yml 'Promote and verify the exact package': Moved ${{ inputs.package-spec }}, ${{ inputs.expected-sha256 }}, ${{ inputs.from-label }}, and ${{ inputs.to-label }} from the run: block into the env: block as PACKAGE_SPEC, EXPECTED_SHA256, FROM_LABEL, TO_LABEL. Shell script now references plain environment variables.

5. action.yml 'Write structured producer evidence': Sanitized SUBJECT_RUN_ID, SUBJECT_RUN_ATTEMPT, SUBJECT_JOB_KEY, and artifact_index with printf '%s' ... | tr -d '\n\r' before constructing the output path and artifact_name. Final values written to GITHUB_OUTPUT are also passed through tr -d '\n\r'.

