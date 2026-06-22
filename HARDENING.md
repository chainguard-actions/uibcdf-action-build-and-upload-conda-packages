<!-- markdownlint-disable -->

# Hardening Report: uibcdf--action-build-and-upload-conda-packages/v1.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **uibcdf--action-build-and-upload-conda-packages/v1.2.0** was hardened automatically. 27 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Multiple run: blocks in action.yml directly interpolate ${{ inputs.* }}, ${{ github.* }}, and ${{ steps.*.outputs.* }} expressions inside shell command strings (rule a). GitHub Actions substitutes these expressions before the shell parses them, so an attacker-controlled value containing shell metacharacters (`;`, `|`, `$(...)`, etc.) would be executed as arbitrary shell code.

Affected instances include:
- Step 'Getting release data': `echo "Trigger event and type: $GITHUB_EVENT_NAME, ${{github.event.action}}"`
- Step 'Checking if meta.yaml is in meta_yaml_dir': `working-directory: ./${{ inputs.meta_yaml_dir }}` and echo messages containing `${{ inputs.meta_yaml_dir }}`
- Step 'Packages compilation': `if "${{ inputs.mambabuild }}"; then`, `--python ${{ inputs.python-version }}`, and all `if "${{ inputs.platform_* }}"; then` conditionals
- Step 'Packages uploading': `label=${{ inputs.label }}`, `if "${{ inputs.overwrite }}"; then`, `if [ "${{github.event.action}}" == "released" ]`, `export ANACONDA_API_TOKEN=${{ inputs.token }}`, `for dirname in ${{ steps.packages-compilation.outputs.out_dir }}/*`, `anaconda upload --user ${{ inputs.user }} ...`

All of these must be moved to env: variables and then referenced as quoted shell variables (e.g., "$INPUT_LABEL") to prevent injection.

Locations:

- `action.yml:83`
- `action.yml:88`
- `action.yml:95`
- `action.yml:175`

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

Rewrote action.yml to fix all shell injection vulnerabilities across 4 steps:

1. 'Getting release data': Moved ${{ github.event.action }} to env: GITHUB_EVENT_ACTION, referenced as $GITHUB_EVENT_ACTION in the run: block.

2. 'Checking if meta.yaml is in meta_yaml_dir': Moved ${{ inputs.meta_yaml_dir }} to env: INPUT_META_YAML_DIR, referenced as $INPUT_META_YAML_DIR in echo messages. The working-directory: field still uses ${{ inputs.meta_yaml_dir }} as it is a YAML field value, not a shell string.

3. 'Packages compilation': Moved all 16 inputs (${{ inputs.mambabuild }}, ${{ inputs.python-version }}, and all 14 platform inputs) to env: block with descriptive names (INPUT_MAMBABUILD, INPUT_PYTHON_VERSION, INPUT_PLATFORM_ALL, INPUT_PLATFORM_LINUX_64, etc.). All if conditionals and command arguments now reference these env vars.

4. 'Packages uploading': Moved ${{ inputs.label }}, ${{ inputs.overwrite }}, ${{ inputs.token }}, ${{ inputs.user }}, ${{ github.event.action }}, and ${{ steps.packages-compilation.outputs.out_dir }} to env: block. All shell references updated to use $INPUT_LABEL, $INPUT_OVERWRITE, $INPUT_TOKEN, $INPUT_USER, $GITHUB_EVENT_ACTION, and $PACKAGES_OUT_DIR respectively.

