<!-- markdownlint-disable -->

# Hardening Report: uibcdf--action-build-and-upload-conda-packages/v1.5.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **uibcdf--action-build-and-upload-conda-packages/v1.5.0** was hardened automatically. 33 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Multiple run: blocks in action.yml directly interpolate ${{ inputs.* }} and ${{ github.* }} expressions inside shell commands, enabling script injection. An attacker controlling these inputs can inject arbitrary shell commands.

Step 'Create GitHub Release' (sub-rule a): github.ref_name is interpolated directly into gh release view and gh release create shell commands: `gh release view "${{ github.ref_name }}"`

Step 'Sanity checks on inputs' (sub-rule a): inputs.upload, inputs.token, inputs.user, inputs.label, inputs.overwrite, inputs.conda_build_args, inputs.conda_convert_args, inputs.anaconda_upload_args are all interpolated directly into shell if-conditions and ${{ contains(...) }} expressions that expand into raw shell: `if [ "${{ inputs.upload }}" == "true" ]` and `if ${{ contains(inputs.conda_build_args, '--no-anaconda-upload') }}`

Step 'Packages compilation' (sub-rule a): inputs.mambabuild, inputs.conda_build_args, inputs.conda_convert_args, and all inputs.platform_* values are interpolated directly into shell commands, including into a string passed to eval: `conda_build_command="conda $build_function . ... ${{ inputs.conda_build_args }}"` then `eval "$conda_build_command"`

Step 'Packages uploading' (sub-rule a): inputs.label, inputs.overwrite, inputs.token, inputs.user, inputs.anaconda_upload_args, and steps.packages-compilation.outputs.* are all interpolated directly into shell commands, with the final command string passed to eval: `command="anaconda upload --user ${{ inputs.user }} ... ${{ inputs.anaconda_upload_args }} $package_path"` then `eval "$command"`

Locations:

- `action.yml:87`
- `action.yml:100`
- `action.yml:148`
- `action.yml:196`

### github-env-injection (severity: high)

Multiple run: blocks write values derived from untrusted inputs to $GITHUB_OUTPUT without the required sanitization step (printf '%s' ... | tr -d '\n\r').

Step 'Packages compilation': HOST_PACKAGE is derived from eval "$conda_build_command --output" where conda_build_command contains ${{ inputs.conda_build_args }} (attacker-controlled). The result is written directly to $GITHUB_OUTPUT without sanitization: `echo "HOST_PACKAGE=$HOST_PACKAGE" >> $GITHUB_OUTPUT`

Step 'Packages uploading': paths is built from package_paths which is derived from find with ${{ steps.packages-compilation.outputs.out_dir }} and ${{ steps.packages-compilation.outputs.HOST_PACKAGE }} — both steps.*.outputs.* values are untrusted. The result is written to $GITHUB_OUTPUT without sanitization: `echo "paths=${paths[@]}" >> $GITHUB_OUTPUT`

Locations:

- `action.yml:158`
- `action.yml:209`

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

**Fixes applied:** script-injection, github-env-injection, static-inline-injection

**Notes:**

Fixed all security findings in hardened/action/action.yml:

1. Moved all ${{ inputs.* }} and ${{ github.* }} expressions from run: blocks into env: blocks for each step. References in shell scripts now use plain $VAR_NAME environment variables.

2. Replaced ${{ contains(inputs.*, '...') }} expressions (which expanded to literal 'true'/'false' in shell) with grep -qF string searches on the corresponding env vars.

3. Eliminated all eval usage with user-controlled input. Commands are now built as bash arrays (conda_build_args_array, conda_convert_args_array, anaconda_upload_args_array, force_args, platforms_options) and expanded with "${array[@]}" to keep each token as a separate argument.

4. Sanitized all GITHUB_OUTPUT writes with printf '%s' "$VAR" | tr -d '\n\r' to prevent newline injection (out_dir, HOST_PACKAGE, paths).

5. Fixed boolean input checks: replaced if "${{ inputs.mambabuild }}" (treating string as command) with if [ "$INPUT_MAMBABUILD" = "true" ].

6. The github.ref_name expression in the Create GitHub Release step was moved to env: REF_NAME and referenced as $REF_NAME in the shell script.

