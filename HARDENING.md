<!-- markdownlint-disable -->

# Hardening Report: uibcdf--action-build-and-upload-conda-packages/v1.4.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **uibcdf--action-build-and-upload-conda-packages/v1.4.0** was hardened automatically. 27 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a): ${{ }} expressions are directly interpolated inside run: shell command strings throughout action.yml across all four composite action steps. Step 'Getting release data': ${{github.event.action}} in an echo command. Step 'Checking if meta.yaml': ${{ inputs.meta_yaml_dir }} in echo commands. Step 'Packages compilation': ${{ inputs.mambabuild }}, ${{ inputs.python-version }}, ${{ inputs.platform_all }}, ${{ inputs.platform_linux-64 }}, ${{ inputs.platform_osx-64 }}, ${{ inputs.platform_osx-arm64 }}, ${{ inputs.platform_linux-32 }}, ${{ inputs.platform_linux-ppc64 }}, ${{ inputs.platform_linux-ppc64le }}, ${{ inputs.platform_linux-s390x }}, ${{ inputs.platform_linux-armv6l }}, ${{ inputs.platform_linux-armv7l }}, ${{ inputs.platform_linux-aarch64 }}, ${{ inputs.platform_win-32 }}, ${{ inputs.platform_win-64 }}, ${{ inputs.platform_host }} directly in shell if-conditions and conda commands. Step 'Packages uploading': ${{ inputs.label }}, ${{ inputs.overwrite }}, ${{github.event.action}}, ${{ inputs.token }}, ${{ steps.packages-compilation.outputs.out_dir }}, and ${{ inputs.user }} directly in shell commands. Any caller of this composite action can inject arbitrary shell commands via these inputs.

Locations:

- `action.yml:100`
- `action.yml:106`
- `action.yml:108`
- `action.yml:120`
- `action.yml:122`
- `action.yml:124`
- `action.yml:130`
- `action.yml:135`
- `action.yml:140`
- `action.yml:145`
- `action.yml:150`
- `action.yml:155`
- `action.yml:160`
- `action.yml:165`
- `action.yml:170`
- `action.yml:175`
- `action.yml:180`
- `action.yml:185`
- `action.yml:190`
- `action.yml:193`
- `action.yml:196`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.meta_yaml_dir }}" appears directly in run: block of step "Checking if meta.yaml is in meta_yaml_dir"; move to env: map

Locations:

- `action.yml:116`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.meta_yaml_dir }}" appears directly in run: block of step "Checking if meta.yaml is in meta_yaml_dir"; move to env: map

Locations:

- `action.yml:119`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.mambabuild }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:129`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.python-version }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:130`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.python-version }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:131`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.python-version }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:133`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.python-version }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:134`

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

- `action.yml:220`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.overwrite }}" appears directly in run: block of step "Packages uploading"; move to env: map

Locations:

- `action.yml:222`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.token }}" appears directly in run: block of step "Packages uploading"; move to env: map

Locations:

- `action.yml:235`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.user }}" appears directly in run: block of step "Packages uploading"; move to env: map

Locations:

- `action.yml:241`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.user }}" appears directly in run: block of step "Packages uploading"; move to env: map

Locations:

- `action.yml:242`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Fixed all script injection vulnerabilities in action.yml by moving every ${{ }} expression out of run: shell command strings into env: blocks. Changes made:

1. Step 'Getting release data': Moved ${{ github.event.action }} to env: as GITHUB_EVENT_ACTION, updated run: to use $GITHUB_EVENT_ACTION.

2. Step 'Checking if meta.yaml is in meta_yaml_dir': Moved ${{ inputs.meta_yaml_dir }} to env: as META_YAML_DIR, updated run: to use $META_YAML_DIR (working-directory: still uses the expression safely as a YAML field).

3. Step 'Packages compilation': Moved all 16 platform/build inputs (${{ inputs.mambabuild }}, ${{ inputs.python-version }}, all 14 platform inputs) to env: block with safe variable names (INPUT_MAMBABUILD, INPUT_PYTHON_VERSION, INPUT_PLATFORM_ALL, INPUT_PLATFORM_LINUX_64, etc.), updated all run: shell commands to use the corresponding environment variables.

4. Step 'Packages uploading': Moved ${{ inputs.label }}, ${{ inputs.overwrite }}, ${{ github.event.action }}, ${{ inputs.token }}, ${{ steps.packages-compilation.outputs.out_dir }}, and ${{ inputs.user }} to env: block as INPUT_LABEL, INPUT_OVERWRITE, GITHUB_EVENT_ACTION, ANACONDA_API_TOKEN, PACKAGES_OUT_DIR, and INPUT_USER respectively. Updated all run: shell commands to use the corresponding environment variables. Also removed the explicit 'export ANACONDA_API_TOKEN=...' line since it's now set via env: block.

### Iteration 2

**Fixes applied:** script-injection, github-env-injection

**Notes:**

Fixed three security findings in action.yml:
1. script-injection (lines 142, 145): Quoted $INPUT_PYTHON_VERSION within the echo strings using escaped quotes: `echo "... \"$INPUT_PYTHON_VERSION\""`. The actual conda build commands already had the variable properly quoted.
2. script-injection (line 219): Fixed the anaconda upload command to use `${force:+"$force"}` (drops argument when empty, quoted when present), `"$label"`, and `"$filename"` — all workflow-controllable variables are now properly double-quoted.
3. github-env-injection (line 228): Added sanitization of the paths array before writing to GITHUB_OUTPUT: `safe_paths=$(printf '%s ' "${paths[@]}" | tr -d '\n\r')` followed by `echo "paths=$safe_paths" >> "$GITHUB_OUTPUT"` to prevent newline injection attacks.

