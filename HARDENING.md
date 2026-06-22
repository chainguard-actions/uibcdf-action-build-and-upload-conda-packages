<!-- markdownlint-disable -->

# Hardening Report: uibcdf--action-build-and-upload-conda-packages/v1.5.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **uibcdf--action-build-and-upload-conda-packages/v1.5.0** was hardened automatically. 34 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a): Multiple ${{ ... }} expressions are directly interpolated inside run: shell command strings throughout action.yml. In the 'Create GitHub Release' step, ${{ github.ref_name }} is used directly in gh CLI arguments. In the 'Sanity checks on inputs' step, ${{ inputs.upload }}, ${{ inputs.token }}, ${{ inputs.user }}, ${{ inputs.conda_build_args }}, ${{ inputs.conda_convert_args }}, ${{ inputs.anaconda_upload_args }}, ${{ inputs.label }}, and ${{ inputs.overwrite }} are all interpolated directly into shell conditionals. In the 'Packages compilation' step, ${{ inputs.mambabuild }}, ${{ inputs.conda_build_args }}, ${{ inputs.conda_convert_args }}, and all platform inputs are interpolated directly into shell commands — including being embedded in a string passed to eval. In the 'Packages uploading' step, ${{ inputs.label }}, ${{ inputs.token }}, ${{ inputs.user }}, ${{ inputs.overwrite }}, and ${{ inputs.anaconda_upload_args }} are interpolated directly into shell commands, with the resulting string passed to eval. An attacker controlling any of these inputs can inject arbitrary shell commands.

Locations:

- `action.yml:108`
- `action.yml:109`
- `action.yml:110`
- `action.yml:111`
- `action.yml:126`
- `action.yml:128`
- `action.yml:132`
- `action.yml:155`
- `action.yml:168`
- `action.yml:176`
- `action.yml:195`
- `action.yml:213`
- `action.yml:232`
- `action.yml:248`
- `action.yml:267`
- `action.yml:280`
- `action.yml:295`
- `action.yml:310`
- `action.yml:325`
- `action.yml:340`
- `action.yml:355`
- `action.yml:370`
- `action.yml:385`

### suspicious-run-content (severity: high)

eval-dynamic: The 'Packages compilation' step constructs a shell command string by embedding ${{ inputs.conda_build_args }} and ${{ inputs.conda_convert_args }} directly into variables, then executes them via eval: `eval "$conda_build_command"` and `eval "${conda_convert_command}${platforms_options}"`. The 'Packages uploading' step similarly builds a command string with ${{ inputs.user }}, ${{ inputs.anaconda_upload_args }}, and other user-controlled inputs, then executes it via `eval "$command"`. This allows an attacker to inject arbitrary shell commands through these inputs.

Locations:

- `action.yml:222`
- `action.yml:223`
- `action.yml:395`
- `action.yml:396`

### github-env-injection (severity: high)

Unsanitized values are written to $GITHUB_OUTPUT without applying the required `printf '%s' ... | tr -d '\n\r'` sanitization. (1) `HOST_PACKAGE` is derived from eval of a command built with ${{ inputs.conda_build_args }} and written as `echo "HOST_PACKAGE=$HOST_PACKAGE" >> $GITHUB_OUTPUT`. (2) `paths=${paths[@]}` is written to $GITHUB_OUTPUT where `package_paths` was found using `find` with a path derived from step outputs and ${{ inputs.anaconda_upload_args }} was embedded in the upload command. These unsanitized writes allow newline injection into the GitHub output context.

Locations:

- `action.yml:224`
- `action.yml:399`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.upload }}" appears directly in run: block of step "Sanity checks on inputs"; move to env: map

Locations:

- `action.yml:135`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.token }}" appears directly in run: block of step "Sanity checks on inputs"; move to env: map

Locations:

- `action.yml:136`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.user }}" appears directly in run: block of step "Sanity checks on inputs"; move to env: map

Locations:

- `action.yml:141`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.label }}" appears directly in run: block of step "Sanity checks on inputs"; move to env: map

Locations:

- `action.yml:161`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.user }}" appears directly in run: block of step "Sanity checks on inputs"; move to env: map

Locations:

- `action.yml:166`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.token }}" appears directly in run: block of step "Sanity checks on inputs"; move to env: map

Locations:

- `action.yml:171`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.overwrite }}" appears directly in run: block of step "Sanity checks on inputs"; move to env: map

Locations:

- `action.yml:176`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.meta_yaml_dir }}" appears directly in run: block of step "Checking if meta.yaml is in meta_yaml_dir"; move to env: map

Locations:

- `action.yml:196`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.meta_yaml_dir }}" appears directly in run: block of step "Checking if meta.yaml is in meta_yaml_dir"; move to env: map

Locations:

- `action.yml:199`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.mambabuild }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:210`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.conda_build_args }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:215`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.conda_convert_args }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:224`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_all }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:225`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-64 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:228`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-32 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:231`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_osx-64 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:234`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_osx-arm64 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:237`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-ppc64 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:240`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-ppc64le }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:243`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-s390x }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:246`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-armv6l }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:249`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-armv7l }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:252`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-aarch64 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:255`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_win-32 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:258`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_win-64 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:261`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_host }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:268`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.label }}" appears directly in run: block of step "Packages uploading"; move to env: map

Locations:

- `action.yml:280`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.overwrite }}" appears directly in run: block of step "Packages uploading"; move to env: map

Locations:

- `action.yml:282`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.token }}" appears directly in run: block of step "Packages uploading"; move to env: map

Locations:

- `action.yml:285`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.user }}" appears directly in run: block of step "Packages uploading"; move to env: map

Locations:

- `action.yml:289`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.anaconda_upload_args }}" appears directly in run: block of step "Packages uploading"; move to env: map

Locations:

- `action.yml:289`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, suspicious-run-content, github-env-injection, static-inline-injection

**Notes:**

Fixed all security findings in action.yml: (1) Moved all ${{ inputs.* }} and ${{ github.ref_name }} expressions from run: blocks to env: maps in each step. (2) Replaced all eval-based command construction with bash array-based commands (build_cmd, convert_cmd, upload_cmd arrays) to eliminate eval injection. (3) Sanitized HOST_PACKAGE and paths values written to $GITHUB_OUTPUT using printf '%s' | tr -d '\n\r' to prevent newline injection. (4) The 'Create GitHub Release' step now uses $REF_NAME env var instead of ${{ github.ref_name }} inline. The 'Sanity checks on inputs' step uses env vars for all inputs and contains() results. The 'Checking if meta.yaml' step uses $INPUT_META_YAML_DIR. The 'Packages compilation' step uses env vars for all platform inputs and build args, with array-based command construction. The 'Packages uploading' step uses env vars for all upload inputs with array-based command construction.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed two script-injection findings in action.yml:

1. 'Sanity checks on inputs' step (line 175): Replaced all unquoted `if $CONTAINS_*` boolean env var expansions with properly quoted `[ "$CONTAINS_*" = "true" ]` comparisons. This applies to CONTAINS_NO_ANACONDA_UPLOAD, CONTAINS_OUTPUT_FOLDER_BUILD, CONTAINS_OUTPUT_FOLDER_CONVERT, CONTAINS_LABEL_LONG, CONTAINS_LABEL_SHORT, CONTAINS_USER_LONG, CONTAINS_USER_SHORT, CONTAINS_TOKEN, and CONTAINS_FORCE.

2. 'Packages uploading' step (line 310): Replaced the unquoted `for package_path in $package_paths` loop (where $package_paths was a plain string from command substitution) with `mapfile -d '' package_paths < <(find "$OUT_DIR" -type f -name "$(basename "$HOST_PACKAGE")" -print0)` followed by `for package_path in "${package_paths[@]}"`. This safely handles filenames with spaces or special characters using null-delimited find output and a bash array.

