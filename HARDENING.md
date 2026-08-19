<!-- markdownlint-disable -->

# Hardening Report: uibcdf--action-build-and-upload-conda-packages/v1.4.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **uibcdf--action-build-and-upload-conda-packages/v1.4.0** was hardened automatically. 30 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a): The 'Getting release data' step directly interpolates a GitHub Actions expression inside a run: shell command. The expression `${{github.event.action}}` is substituted into the shell string before the shell parses it, allowing an attacker to inject arbitrary shell commands via the event action value. Offending line: `echo "Trigger event and type: $GITHUB_EVENT_NAME, ${{github.event.action}}"`

Locations:

- `action.yml:76`

### script-injection (severity: high)

Rule (a): The 'Checking if meta.yaml is in meta_yaml_dir' step directly interpolates `${{ inputs.meta_yaml_dir }}` inside run: shell echo commands. An attacker-controlled input value is substituted into the shell string before parsing, enabling shell command injection. Offending lines include: `echo "...was not found in ${{ inputs.meta_yaml_dir }}."` and `echo "...was found in ${{ inputs.meta_yaml_dir }}."`. The same input is also used in the working-directory field.

Locations:

- `action.yml:87`
- `action.yml:89`

### script-injection (severity: high)

Rule (a): The 'Packages compilation' step directly interpolates multiple `${{ inputs.* }}` expressions inside run: shell commands. This includes: `${{ inputs.mambabuild }}` used in an `if` conditional (e.g. `if "${{ inputs.mambabuild }}"`), `${{ inputs.python-version }}` appended directly to conda build/mambabuild commands (e.g. `conda mambabuild . ... --python ${{ inputs.python-version }}`), and all platform inputs (`${{ inputs.platform_all }}`, `${{ inputs.platform_linux-64 }}`, `${{ inputs.platform_osx-64 }}`, `${{ inputs.platform_osx-arm64 }}`, `${{ inputs.platform_linux-32 }}`, `${{ inputs.platform_linux-ppc64 }}`, `${{ inputs.platform_linux-ppc64le }}`, `${{ inputs.platform_linux-s390x }}`, `${{ inputs.platform_linux-armv6l }}`, `${{ inputs.platform_linux-armv7l }}`, `${{ inputs.platform_linux-aarch64 }}`, `${{ inputs.platform_win-32 }}`, `${{ inputs.platform_win-64 }}`, `${{ inputs.platform_host }}`) used in `if` conditionals. All of these are substituted into the shell before parsing, enabling command injection.

Locations:

- `action.yml:97`
- `action.yml:99`
- `action.yml:101`
- `action.yml:102`
- `action.yml:108`
- `action.yml:113`
- `action.yml:118`
- `action.yml:123`
- `action.yml:128`
- `action.yml:133`
- `action.yml:138`
- `action.yml:143`
- `action.yml:148`
- `action.yml:153`
- `action.yml:158`
- `action.yml:163`

### script-injection (severity: high)

Rule (a): The 'Packages uploading' step directly interpolates multiple `${{ inputs.* }}`, `${{ github.* }}`, and `${{ steps.*.outputs.* }}` expressions inside run: shell commands. Specifically: `label=${{ inputs.label }}` (unquoted assignment from user input), `if "${{ inputs.overwrite }}"` (conditional from user input), `${{github.event.action}}` used in string comparisons and echo (attacker-controlled via event), `export ANACONDA_API_TOKEN=${{ inputs.token }}` (token value injected directly into shell — extremely dangerous as it exposes the token value in the shell command string), `for dirname in ${{ steps.packages-compilation.outputs.out_dir }}/*` (step output injected into a glob expansion), and `anaconda upload --user ${{ inputs.user }}` (user input injected into a command argument). Any of these can contain shell metacharacters enabling command injection.

Locations:

- `action.yml:170`
- `action.yml:172`
- `action.yml:176`
- `action.yml:178`
- `action.yml:180`
- `action.yml:183`
- `action.yml:188`
- `action.yml:192`
- `action.yml:194`

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

Fixed all script injection vulnerabilities in hardened/action/action.yml by moving every ${{ }} expression out of run: shell blocks and into the step's env: block. Changes made:

1. 'Getting release data' step: Moved ${{github.event.action}} to env: as EVENT_ACTION, replaced inline expression with $EVENT_ACTION in the echo command.

2. 'Checking if meta.yaml is in meta_yaml_dir' step: Moved ${{ inputs.meta_yaml_dir }} to env: as META_YAML_DIR, replaced both inline expressions in echo commands with $META_YAML_DIR. The working-directory: field retains the expression (safe - not a shell command).

3. 'Packages compilation' step: Moved all 16 ${{ inputs.* }} expressions to env: block with descriptive names (INPUT_MAMBABUILD, INPUT_PYTHON_VERSION, INPUT_PLATFORM_ALL, INPUT_PLATFORM_LINUX_64, INPUT_PLATFORM_OSX_64, INPUT_PLATFORM_OSX_ARM64, INPUT_PLATFORM_LINUX_32, INPUT_PLATFORM_LINUX_PPC64, INPUT_PLATFORM_LINUX_PPC64LE, INPUT_PLATFORM_LINUX_S390X, INPUT_PLATFORM_LINUX_ARMV6L, INPUT_PLATFORM_LINUX_ARMV7L, INPUT_PLATFORM_LINUX_AARCH64, INPUT_PLATFORM_WIN_32, INPUT_PLATFORM_WIN_64, INPUT_PLATFORM_HOST). All run: shell commands now reference plain env vars.

4. 'Packages uploading' step: Moved ${{ inputs.label }}, ${{ inputs.overwrite }}, ${{ github.event.action }}, ${{ inputs.token }}, ${{ steps.packages-compilation.outputs.out_dir }}, and ${{ inputs.user }} to env: block. The token is now passed as ANACONDA_API_TOKEN environment variable (which anaconda CLI reads automatically) rather than being exported inline in the shell. All run: shell commands now reference plain env vars.

### Iteration 2

**Fixes applied:** script-injection, github-env-injection

**Notes:**

Fixed two high-severity findings in the 'Packages uploading' step of action.yml:

1. script-injection: (a) The debug echo line now properly escapes/quotes all variables ($INPUT_USER, $label, $filename) to prevent shell metacharacter injection. (b) The unquoted $force variable was replaced with a bash array (upload_args) that is populated conditionally and expanded as "${upload_args[@]}" in the actual anaconda upload command, ensuring each argument stays properly quoted and separate.

2. github-env-injection: The paths array content is now sanitized via `printf '%s ' "${paths[@]}" | tr -d '\n\r'` before writing to $GITHUB_OUTPUT, stripping any embedded newlines or carriage returns that could inject additional output entries.

