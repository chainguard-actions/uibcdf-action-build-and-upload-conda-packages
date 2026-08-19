<!-- markdownlint-disable -->

# Hardening Report: uibcdf--action-build-and-upload-conda-packages/v1.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **uibcdf--action-build-and-upload-conda-packages/v1.2.0** was hardened automatically. 27 finding(s) were identified and resolved across 4 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a) violation: Multiple `run:` blocks in action.yml directly interpolate `${{ inputs.* }}` and `${{ github.* }}` expressions into shell commands without routing them through env: variables. This allows an attacker-controlled value to break out of the shell context and execute arbitrary commands.

Step 'Getting release data' (line 63): `echo "Trigger event and type: $GITHUB_EVENT_NAME, ${{github.event.action}}"`

Step 'Checking if meta.yaml is in meta_yaml_dir' (line 70): `echo "...not found in ${{ inputs.meta_yaml_dir }}."` and similar.

Step 'Packages compilation' (lines 79–161): `if "${{ inputs.mambabuild }}"; then`, `--python ${{ inputs.python-version }}`, and all `if "${{ inputs.platform_* }}"; then` conditionals.

Step 'Packages uploading' (lines 165–195): `label=${{ inputs.label }}`, `if "${{ inputs.overwrite }}"; then`, `if [ "${{github.event.action}}" == "released" ]`, `export ANACONDA_API_TOKEN=${{ inputs.token }}`, `anaconda upload --user ${{ inputs.user }} ...`. All of these allow shell metacharacter injection via attacker-supplied input values.

Locations:

- `action.yml:63`
- `action.yml:70`
- `action.yml:79`
- `action.yml:165`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.meta_yaml_dir }}" appears directly in run: block of step "Checking if meta.yaml is in meta_yaml_dir"; move to env: map

Locations:

- `action.yml:108`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.meta_yaml_dir }}" appears directly in run: block of step "Checking if meta.yaml is in meta_yaml_dir"; move to env: map

Locations:

- `action.yml:111`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.mambabuild }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:121`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.python-version }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:122`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.python-version }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:123`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.python-version }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:125`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.python-version }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:126`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_all }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:141`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-64 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:146`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_osx-64 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:151`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_osx-arm64 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:156`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-32 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:161`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-ppc64 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:166`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-ppc64le }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:171`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-s390x }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:176`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-armv6l }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:181`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-armv7l }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:186`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-aarch64 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:191`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_win-32 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:196`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_win-64 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:201`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_host }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:206`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.label }}" appears directly in run: block of step "Packages uploading"; move to env: map

Locations:

- `action.yml:219`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.overwrite }}" appears directly in run: block of step "Packages uploading"; move to env: map

Locations:

- `action.yml:221`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.token }}" appears directly in run: block of step "Packages uploading"; move to env: map

Locations:

- `action.yml:234`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.user }}" appears directly in run: block of step "Packages uploading"; move to env: map

Locations:

- `action.yml:240`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.user }}" appears directly in run: block of step "Packages uploading"; move to env: map

Locations:

- `action.yml:241`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Rewrote action.yml to fix all script injection findings across 4 steps:

1. 'Getting release data': Moved ${{ github.event.action }} to env: GITHUB_EVENT_ACTION, replaced inline expression in echo with $GITHUB_EVENT_ACTION.

2. 'Checking if meta.yaml is in meta_yaml_dir': Moved ${{ inputs.meta_yaml_dir }} to env: META_YAML_DIR, replaced both inline expressions in the run: block with $META_YAML_DIR. (working-directory: still uses the expression safely as a YAML field, not shell.)

3. 'Packages compilation': Moved all 16 platform/build inputs (${{ inputs.mambabuild }}, ${{ inputs.python-version }}, and all 14 platform_* inputs) to env: block with safe variable names (INPUT_MAMBABUILD, INPUT_PYTHON_VERSION, INPUT_PLATFORM_*, etc.). All if-conditionals and command arguments now use the env vars.

4. 'Packages uploading': Moved ${{ inputs.label }}, ${{ inputs.overwrite }}, ${{ inputs.token }}, ${{ inputs.user }}, ${{ github.event.action }}, and ${{ steps.packages-compilation.outputs.out_dir }} to env: block. All shell references updated to use env vars ($INPUT_LABEL, $INPUT_OVERWRITE, $INPUT_TOKEN, $INPUT_USER, $GITHUB_EVENT_ACTION, $PACKAGES_OUT_DIR).

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed two script-injection findings in hardened/action/action.yml:
1. 'Packages compilation' step (lines 126, 129): Both echo strings now quote $INPUT_PYTHON_VERSION using escaped double-quotes (\"$INPUT_PYTHON_VERSION\") inside the double-quoted echo string, preventing shell metacharacter injection from the python-version input.
2. 'Packages uploading' step (lines 185, 193): (a) The for-loop glob now uses "$PACKAGES_OUT_DIR"/* with the variable properly quoted to prevent word-splitting and glob expansion on the directory path. The inner dirname loop variable is also quoted. (b) The echo string now uses \"$INPUT_USER\" to properly quote the user input value.

### Iteration 3

**Fixes applied:** script-injection

**Notes:**

Fixed unquoted shell variables in the 'Packages uploading' step of action.yml. Changed `$label` to `"$label"` to prevent shell metacharacter injection from the user-controlled `inputs.label` value. Changed `$force` to `${force:+"$force"}` to both properly quote the flag when set and avoid passing an empty argument when the force flag is not needed. Both the echo statement and the anaconda upload command were updated consistently.

### Iteration 4

**Fixes applied:** script-injection

**Notes:**

Fixed the script injection vulnerability in the 'Packages uploading' step of action.yml. The `$filename` variable was unquoted in both the `echo` command (line 232) and the `anaconda upload` command (line 233). Since `$filename` is derived from iterating over `"$PACKAGES_OUT_DIR"/*` where `PACKAGES_OUT_DIR` is set from the workflow-controllable `steps.packages-compilation.outputs.out_dir` value, an unquoted expansion could allow shell metacharacters to be interpreted as shell commands. Both occurrences have been fixed by properly quoting the variable as `"$filename"`.

