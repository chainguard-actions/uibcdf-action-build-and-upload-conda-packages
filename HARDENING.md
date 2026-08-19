<!-- markdownlint-disable -->

# Hardening Report: uibcdf--action-build-and-upload-conda-packages/v1.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **uibcdf--action-build-and-upload-conda-packages/v1.3.0** was hardened automatically. 27 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Multiple `${{ ... }}` expressions from `inputs.*`, `github.*`, and `steps.*.outputs.*` contexts are directly interpolated inside `run:` shell command strings throughout action.yml (sub-rule a). This allows an attacker who controls input values to inject arbitrary shell commands. Affected expressions include: `${{github.event.action}}` in the 'Getting release data' step; `${{ inputs.meta_yaml_dir }}` in the 'Checking if meta.yaml' step's run block; `${{ inputs.mambabuild }}`, `${{ inputs.python-version }}`, and all `${{ inputs.platform_* }}` / `${{ inputs.platform_host }}` flags in the 'Packages compilation' step; and `${{ inputs.label }}`, `${{ inputs.overwrite }}`, `${{github.event.action}}`, `${{ inputs.token }}`, `${{ steps.packages-compilation.outputs.out_dir }}`, and `${{ inputs.user }}` in the 'Packages uploading' step. All of these must be moved to `env:` variables and then referenced as quoted shell variables (e.g. `"$INPUT_LABEL"`) instead of being interpolated directly.

Locations:

- `action.yml:72`
- `action.yml:80`
- `action.yml:83`
- `action.yml:85`
- `action.yml:93`
- `action.yml:95`
- `action.yml:98`
- `action.yml:100`
- `action.yml:103`
- `action.yml:113`
- `action.yml:119`
- `action.yml:125`
- `action.yml:131`
- `action.yml:137`
- `action.yml:143`
- `action.yml:149`
- `action.yml:155`
- `action.yml:161`
- `action.yml:167`
- `action.yml:173`
- `action.yml:179`
- `action.yml:185`
- `action.yml:191`
- `action.yml:200`
- `action.yml:202`
- `action.yml:204`
- `action.yml:209`
- `action.yml:214`
- `action.yml:221`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.meta_yaml_dir }}" appears directly in run: block of step "Checking if meta.yaml is in meta_yaml_dir"; move to env: map

Locations:

- `action.yml:112`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.meta_yaml_dir }}" appears directly in run: block of step "Checking if meta.yaml is in meta_yaml_dir"; move to env: map

Locations:

- `action.yml:115`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.mambabuild }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:125`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.python-version }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:126`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.python-version }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:127`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.python-version }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:129`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.python-version }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:130`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_all }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:145`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-64 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:150`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_osx-64 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:155`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_osx-arm64 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:160`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-32 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:165`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-ppc64 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:170`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-ppc64le }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:175`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-s390x }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:180`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-armv6l }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:185`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-armv7l }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:190`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-aarch64 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:195`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_win-32 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:200`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_win-64 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:205`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_host }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:210`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.label }}" appears directly in run: block of step "Packages uploading"; move to env: map

Locations:

- `action.yml:224`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.overwrite }}" appears directly in run: block of step "Packages uploading"; move to env: map

Locations:

- `action.yml:226`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.token }}" appears directly in run: block of step "Packages uploading"; move to env: map

Locations:

- `action.yml:239`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.user }}" appears directly in run: block of step "Packages uploading"; move to env: map

Locations:

- `action.yml:245`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.user }}" appears directly in run: block of step "Packages uploading"; move to env: map

Locations:

- `action.yml:246`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Fixed all script injection findings in action.yml by moving every ${{ ... }} expression out of run: shell blocks into env: maps:

1. 'Getting release data' step: moved ${{github.event.action}} to env: GITHUB_EVENT_ACTION
2. 'Checking if meta.yaml' step: moved ${{ inputs.meta_yaml_dir }} to env: INPUT_META_YAML_DIR (working-directory: field retains the expression as it is not a shell script)
3. 'Packages compilation' step: moved ${{ inputs.mambabuild }}, ${{ inputs.python-version }}, and all 13 platform inputs (${{ inputs.platform_* }}, ${{ inputs.platform_host }}) to env: block with descriptive variable names (INPUT_MAMBABUILD, INPUT_PYTHON_VERSION, INPUT_PLATFORM_ALL, INPUT_PLATFORM_LINUX_64, etc.)
4. 'Packages uploading' step: moved ${{ inputs.label }}, ${{ inputs.overwrite }}, ${{github.event.action}}, ${{ inputs.token }}, ${{ steps.packages-compilation.outputs.out_dir }}, and ${{ inputs.user }} to env: block (INPUT_LABEL, INPUT_OVERWRITE, GITHUB_EVENT_ACTION, INPUT_TOKEN, PACKAGES_OUT_DIR, INPUT_USER)

All shell references to these values now use double-quoted $VAR_NAME syntax to prevent word splitting and injection.

