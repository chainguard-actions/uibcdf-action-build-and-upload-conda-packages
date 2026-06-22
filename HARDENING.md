<!-- markdownlint-disable -->

# Hardening Report: uibcdf--action-build-and-upload-conda-packages/v1.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **uibcdf--action-build-and-upload-conda-packages/v1.1.0** was hardened automatically. 24 finding(s) were identified and resolved across 3 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a): Multiple `run:` blocks in action.yml directly interpolate GitHub Actions expressions inside shell command strings, enabling script injection. Affected steps and examples:

• 'Getting release data' step: `echo "Trigger event and type: $GITHUB_EVENT_NAME, ${{github.event.action}}"`

• 'Checking if meta.yaml is in meta_yaml_dir' step: `echo "...not found in ${{ inputs.meta_yaml_dir }}."` and `echo "...found in ${{ inputs.meta_yaml_dir }}."` — attacker-controlled path injected into echo.

• 'Packages compilation' step: `conda build . --no-anaconda-upload --output-folder $out_dir --python ${{ inputs.python-version }}` and `if "${{ inputs.platform_all }}"; then` (and 12 similar platform conditionals) — all inputs interpolated directly into shell.

• 'Packages uploading' step: `label=${{ inputs.label }}`, `if "${{ inputs.overwrite }}"; then`, `${{github.event.action}}` (×3), `export ANACONDA_API_TOKEN=${{ inputs.token }}`, `for dirname in ${{ steps.packages-compilation.outputs.out_dir }}/*`, `anaconda upload --user ${{ inputs.user }} ...` — all interpolated directly into shell without quoting or env: indirection.

An attacker who controls any of these inputs (e.g. `inputs.label`, `inputs.user`, `inputs.token`, `inputs.meta_yaml_dir`, `inputs.python-version`) can inject arbitrary shell commands.

Locations:

- `action.yml:83`
- `action.yml:90`
- `action.yml:92`
- `action.yml:103`
- `action.yml:104`
- `action.yml:110`
- `action.yml:115`
- `action.yml:120`
- `action.yml:125`
- `action.yml:130`
- `action.yml:135`
- `action.yml:140`
- `action.yml:145`
- `action.yml:150`
- `action.yml:155`
- `action.yml:160`
- `action.yml:165`
- `action.yml:166`
- `action.yml:168`
- `action.yml:171`
- `action.yml:173`
- `action.yml:176`
- `action.yml:179`
- `action.yml:181`
- `action.yml:185`
- `action.yml:186`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.meta_yaml_dir }}" appears directly in run: block of step "Checking if meta.yaml is in meta_yaml_dir"; move to env: map

Locations:

- `action.yml:104`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.meta_yaml_dir }}" appears directly in run: block of step "Checking if meta.yaml is in meta_yaml_dir"; move to env: map

Locations:

- `action.yml:107`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.python-version }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:117`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.python-version }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:118`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_all }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:132`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-64 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:137`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_osx-64 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:142`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_osx-arm64 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:147`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-32 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:152`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-ppc64 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:157`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-ppc64le }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:162`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-s390x }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:167`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-armv6l }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:172`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-armv7l }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:177`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_linux-aarch64 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:182`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_win-32 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:187`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_win-64 }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:192`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.platform_host }}" appears directly in run: block of step "Packages compilation"; move to env: map

Locations:

- `action.yml:197`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.label }}" appears directly in run: block of step "Packages uploading"; move to env: map

Locations:

- `action.yml:210`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.overwrite }}" appears directly in run: block of step "Packages uploading"; move to env: map

Locations:

- `action.yml:212`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.token }}" appears directly in run: block of step "Packages uploading"; move to env: map

Locations:

- `action.yml:225`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.user }}" appears directly in run: block of step "Packages uploading"; move to env: map

Locations:

- `action.yml:231`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.user }}" appears directly in run: block of step "Packages uploading"; move to env: map

Locations:

- `action.yml:232`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Fixed all script injection vulnerabilities in action.yml by moving every ${{ }} expression out of run: shell blocks into env: maps. Changes made to 4 steps: (1) 'Getting release data': moved github.event.action to EVENT_ACTION env var; (2) 'Checking if meta.yaml is in meta_yaml_dir': moved inputs.meta_yaml_dir to META_YAML_DIR env var; (3) 'Packages compilation': moved inputs.python-version and all 13 platform inputs to env vars (PYTHON_VERSION, PLATFORM_ALL, PLATFORM_LINUX_64, PLATFORM_OSX_64, PLATFORM_OSX_ARM64, PLATFORM_LINUX_32, PLATFORM_LINUX_PPC64, PLATFORM_LINUX_PPC64LE, PLATFORM_LINUX_S390X, PLATFORM_LINUX_ARMV6L, PLATFORM_LINUX_ARMV7L, PLATFORM_LINUX_AARCH64, PLATFORM_WIN_32, PLATFORM_WIN_64, PLATFORM_HOST); (4) 'Packages uploading': moved inputs.label, inputs.overwrite, inputs.token, inputs.user, github.event.action, and steps.packages-compilation.outputs.out_dir to env vars (INPUT_LABEL, INPUT_OVERWRITE, INPUT_TOKEN, INPUT_USER, EVENT_ACTION, COMPILATION_OUT_DIR). The working-directory: fields still use ${{ inputs.meta_yaml_dir }} which is safe as it is not a shell script context.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed two script-injection findings in action.yml's 'Packages uploading' step: (1) Double-quoted $COMPILATION_OUT_DIR in the glob expansion: `for dirname in "$COMPILATION_OUT_DIR"/*` to prevent shell metacharacter injection from the workflow-controllable steps output value. (2) Double-quoted $INPUT_USER inside the echo command to prevent shell metacharacter injection from the untrusted caller-supplied 'user' input. The actual anaconda upload invocation already had $INPUT_USER properly quoted.

### Iteration 3

**Fixes applied:** script-injection

**Notes:**

Fixed two script-injection vulnerabilities in action.yml:
1. 'Packages compilation' step (line 113): Replaced all `if "$PLATFORM_*"; then` constructs (which executed the variable value as a shell command) with proper `if [ "$PLATFORM_*" = "true" ]; then` string comparisons. Also fixed `if ! "$PLATFORM_HOST"; then` to `if [ "$PLATFORM_HOST" != "true" ]; then`.
2. 'Packages uploading' step (line 232): Replaced `if "$INPUT_OVERWRITE"; then` with `if [ "$INPUT_OVERWRITE" = "true" ]; then`. Replaced the unquoted `$force` string variable (which allowed word-splitting of attacker-controlled values) with a bash array `force_args` that is safely expanded as `"${force_args[@]}"` in the anaconda upload command, ensuring proper argument separation without injection risk.

