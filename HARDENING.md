<!-- markdownlint-disable -->

# Hardening Report: uibcdf--action-build-and-upload-conda-packages/v1.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **uibcdf--action-build-and-upload-conda-packages/v1.3.0** was hardened automatically. 27 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a): Multiple ${{ }} expressions are interpolated directly inside run: shell command strings across all four steps in action.yml. This allows an attacker who controls the calling workflow's inputs or github context to inject arbitrary shell commands.

Step 1 ('Getting release data', line 103): `echo "Trigger event and type: $GITHUB_EVENT_NAME, ${{github.event.action}}"`

Step 2 ('Checking if meta.yaml', lines 113, 116): `${{ inputs.meta_yaml_dir }}` interpolated directly in echo commands inside the run block.

Step 3 ('Packages compilation', lines 126, 128, 131, 146, 151, 156, 161, 166, 171, 176, 181, 186, 191, 196, 201, 206, 211): `${{ inputs.mambabuild }}`, `${{ inputs.python-version }}`, and all `${{ inputs.platform_* }}` values are interpolated directly into shell conditionals and command arguments.

Step 4 ('Packages uploading', lines 225, 227, 231, 233, 236, 240, 242, 246, 247): `${{ inputs.label }}`, `${{ inputs.overwrite }}`, `${{github.event.action}}`, `${{ inputs.token }}`, `${{ steps.packages-compilation.outputs.out_dir }}`, and `${{ inputs.user }}` are all interpolated directly into shell commands.

Locations:

- `action.yml:103`
- `action.yml:113`
- `action.yml:116`
- `action.yml:126`
- `action.yml:128`
- `action.yml:131`
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
- `action.yml:225`
- `action.yml:227`
- `action.yml:231`
- `action.yml:233`
- `action.yml:236`
- `action.yml:240`
- `action.yml:242`
- `action.yml:246`
- `action.yml:247`

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

Fixed all script injection vulnerabilities in action.yml by moving every ${{ }} expression out of run: shell strings and into the step's env: block. Changes made:

1. Step 'Getting release data': Moved ${{github.event.action}} to env: as GITHUB_EVENT_ACTION, updated shell to use $GITHUB_EVENT_ACTION.

2. Step 'Checking if meta.yaml is in meta_yaml_dir': Moved ${{ inputs.meta_yaml_dir }} to env: as META_YAML_DIR, updated echo commands to use $META_YAML_DIR. (working-directory: field still uses the expression as it's a YAML field, not a shell string.)

3. Step 'Packages compilation': Moved all inputs (${{ inputs.mambabuild }}, ${{ inputs.python-version }}, and all 13 platform inputs) to env: block as INPUT_MAMBABUILD, INPUT_PYTHON_VERSION, INPUT_PLATFORM_ALL, INPUT_PLATFORM_LINUX_64, INPUT_PLATFORM_OSX_64, INPUT_PLATFORM_OSX_ARM64, INPUT_PLATFORM_LINUX_32, INPUT_PLATFORM_LINUX_PPC64, INPUT_PLATFORM_LINUX_PPC64LE, INPUT_PLATFORM_LINUX_S390X, INPUT_PLATFORM_LINUX_ARMV6L, INPUT_PLATFORM_LINUX_ARMV7L, INPUT_PLATFORM_LINUX_AARCH64, INPUT_PLATFORM_WIN_32, INPUT_PLATFORM_WIN_64, INPUT_PLATFORM_HOST. Updated all shell conditionals and commands to reference plain env vars.

4. Step 'Packages uploading': Moved ${{ inputs.label }}, ${{ inputs.overwrite }}, ${{github.event.action}}, ${{ inputs.token }}, ${{ steps.packages-compilation.outputs.out_dir }}, and ${{ inputs.user }} to env: block as INPUT_LABEL, INPUT_OVERWRITE, GITHUB_EVENT_ACTION, INPUT_TOKEN, PACKAGES_OUT_DIR, INPUT_USER. Updated all shell commands to reference plain env vars.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed two script-injection findings in action.yml:

1. 'Packages compilation' step (lines 148/151): Replaced unquoted $INPUT_PYTHON_VERSION in echo lines with printf using %s format specifiers (printf 'conda ... --python %s\n' "$out_dir" "$INPUT_PYTHON_VERSION"), safely passing the variable as a printf argument. Also fixed the actual command lines to quote "$out_dir".

2. 'Packages uploading' step (lines 241/243/248):
   - Quoted $INPUT_TOKEN: export ANACONDA_API_TOKEN="$INPUT_TOKEN"
   - Quoted $PACKAGES_OUT_DIR in for loop: for dirname in "$PACKAGES_OUT_DIR"/*; and inner loops also quoted
   - Replaced unquoted $force with a bash array (force_args) to safely handle the optional --force flag, and quoted $label and $filename in the anaconda upload command

