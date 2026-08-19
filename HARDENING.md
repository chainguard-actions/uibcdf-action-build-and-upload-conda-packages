<!-- markdownlint-disable -->

# Hardening Report: uibcdf--action-build-and-upload-conda-packages/v1.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **uibcdf--action-build-and-upload-conda-packages/v1.1.0** was hardened automatically. 24 finding(s) were identified and resolved across 4 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): Multiple ${{ }} expressions are interpolated directly inside run: shell command strings across all four composite action steps. This allows an attacker who controls the calling workflow's inputs or event payload to inject arbitrary shell commands.

Step 1 ('Getting release data', line ~82): `echo "Trigger event and type: $GITHUB_EVENT_NAME, ${{github.event.action}}"` — github.event.action is attacker-controlled.

Step 2 ('Checking if meta.yaml is in meta_yaml_dir', lines ~91–93): `${{ inputs.meta_yaml_dir }}` is interpolated directly into echo statements inside the run: block.

Step 3 ('Packages compilation', lines ~100–160): `${{ inputs.python-version }}` is passed directly to `conda build ... --python ${{ inputs.python-version }}`; all platform boolean inputs (`${{ inputs.platform_all }}`, `${{ inputs.platform_linux-64 }}`, etc.) are interpolated directly into `if "${{ inputs.platform_* }}"; then` conditionals — a crafted value like `true; malicious_command` would execute arbitrary code.

Step 4 ('Packages uploading', lines ~165–195): `${{ inputs.label }}` is assigned as `label=${{ inputs.label }}` (unquoted, allowing word-splitting and injection); `${{ inputs.overwrite }}` is used in `if "${{ inputs.overwrite }}"`; `${{ github.event.action }}` appears three times in comparisons; `${{ inputs.token }}` is interpolated as `export ANACONDA_API_TOKEN=${{ inputs.token }}`; `${{ inputs.user }}` is passed directly to `anaconda upload --user ${{ inputs.user }}`.

All of these must be moved to env: variables and the env vars must be double-quoted in the shell script.

Locations:

- `action.yml:82`
- `action.yml:91`
- `action.yml:93`
- `action.yml:101`
- `action.yml:102`
- `action.yml:111`
- `action.yml:116`
- `action.yml:121`
- `action.yml:126`
- `action.yml:131`
- `action.yml:136`
- `action.yml:141`
- `action.yml:146`
- `action.yml:151`
- `action.yml:156`
- `action.yml:161`
- `action.yml:166`
- `action.yml:171`
- `action.yml:176`
- `action.yml:181`
- `action.yml:186`
- `action.yml:191`
- `action.yml:196`
- `action.yml:201`
- `action.yml:206`
- `action.yml:211`
- `action.yml:216`
- `action.yml:221`
- `action.yml:226`
- `action.yml:231`
- `action.yml:236`
- `action.yml:241`

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

Fixed all script injection findings in hardened/action/action.yml by moving every ${{ }} expression out of run: shell blocks and into env: maps for each step:

1. Step 'Getting release data': Moved `${{github.event.action}}` to env var GITHUB_EVENT_ACTION.

2. Step 'Checking if meta.yaml is in meta_yaml_dir': Moved `${{ inputs.meta_yaml_dir }}` (used in echo statements) to env var META_YAML_DIR. The working-directory: field retains the expression as it is a YAML field, not shell code.

3. Step 'Packages compilation': Moved all 15 platform inputs and python-version to env vars (INPUT_PYTHON_VERSION, INPUT_PLATFORM_ALL, INPUT_PLATFORM_LINUX_64, INPUT_PLATFORM_OSX_64, INPUT_PLATFORM_OSX_ARM64, INPUT_PLATFORM_LINUX_32, INPUT_PLATFORM_LINUX_PPC64, INPUT_PLATFORM_LINUX_PPC64LE, INPUT_PLATFORM_LINUX_S390X, INPUT_PLATFORM_LINUX_ARMV6L, INPUT_PLATFORM_LINUX_ARMV7L, INPUT_PLATFORM_LINUX_AARCH64, INPUT_PLATFORM_WIN_32, INPUT_PLATFORM_WIN_64, INPUT_PLATFORM_HOST). All are double-quoted in the shell script.

4. Step 'Packages uploading': Moved `${{ inputs.label }}`, `${{ inputs.overwrite }}`, `${{ inputs.token }}`, `${{ inputs.user }}`, `${{ github.event.action }}`, and `${{ steps.packages-compilation.outputs.out_dir }}` to env vars (INPUT_LABEL, INPUT_OVERWRITE, INPUT_TOKEN, INPUT_USER, GITHUB_EVENT_ACTION, PACKAGES_OUT_DIR). All are double-quoted in the shell script.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed two script-injection findings in hardened/action/action.yml:
1. Line 137 ('Packages compilation' step): Quoted $INPUT_PYTHON_VERSION in the echo diagnostic line: changed `--python $INPUT_PYTHON_VERSION` to `--python \"$INPUT_PYTHON_VERSION\"` inside the double-quoted echo string.
2. Line 195 ('Packages uploading' step): Quoted $PACKAGES_OUT_DIR in the for loop: changed `for dirname in $PACKAGES_OUT_DIR/*` to `for dirname in "$PACKAGES_OUT_DIR"/*` to prevent glob/word-splitting attacks.
3. Line 198 ('Packages uploading' step): Quoted $INPUT_USER in the echo diagnostic line: changed `--user $INPUT_USER` to `--user \"$INPUT_USER\"` inside the double-quoted echo string.

### Iteration 3

**Fixes applied:** script-injection

**Notes:**

Fixed two script-injection findings in hardened/action/action.yml:

1. Packages uploading step (line 243): Changed unquoted `$force` to `${force:+"$force"}` in the `anaconda upload` command. This uses bash conditional expansion to drop the argument entirely when `$force` is empty (avoiding an empty positional argument) while still double-quoting it when present (preventing shell injection).

2. Packages compilation step (lines 119-185): Added double-quotes around all unquoted variable expansions:
   - `for dirname in "$out_dir"/*` (glob base)
   - `for filename in "$dirname"/*` (glob base)
   - `du -sh "$HOST_PACKAGE"`
   - All 13 `conda convert -p <platform> "$HOST_PACKAGE" -o "$out_dir"` invocations
   - `rm "$HOST_PACKAGE"`

The `$label` variable was already properly quoted in the previous iteration.

### Iteration 4

**Fixes applied:** script-injection

**Notes:**

Fixed the unquoted shell variable expansion in the 'Packages uploading' step at line ~233. Changed `for filename in $dirname/*;` to `for filename in "$dirname"/*;` to prevent shell metacharacter injection via the workflow-controllable `PACKAGES_OUT_DIR` step output value.

