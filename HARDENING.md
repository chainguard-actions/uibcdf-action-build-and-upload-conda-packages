<!-- markdownlint-disable -->

# Hardening Report: uibcdf--action-build-and-upload-conda-packages/v2.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **uibcdf--action-build-and-upload-conda-packages/v2.3.0** was hardened automatically. 2 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a): The 'Sanity checks on inputs' run: block in action.yml directly interpolates ${{ contains(inputs.xxx) }} expressions into the shell script. Multiple occurrences: `if ${{ contains(inputs.conda_build_args, '--no-anaconda-upload') }}`, `if ${{ contains(inputs.conda_build_args, '--output-folder') }}`, `if ${{ contains(inputs.conda_convert_args, '--output-folder') }}`, `if (${{ contains(inputs.anaconda_upload_args, '--label') }} || ${{ contains(inputs.anaconda_upload_args, '-l') }})`, `if (${{ contains(inputs.anaconda_upload_args, '--user') }} || ${{ contains(inputs.anaconda_upload_args, '-u') }})`, `if ${{ contains(inputs.anaconda_upload_args, '--token') }}`, and `if ${{ contains(inputs.anaconda_upload_args, '--force') }}`. Any ${{ ... }} expression interpolated directly inside a run: shell command string is a script-injection violation regardless of which context it reads from.

Locations:

- `action.yml:152`
- `action.yml:156`
- `action.yml:160`
- `action.yml:164`
- `action.yml:168`
- `action.yml:172`
- `action.yml:176`

### script-injection (severity: high)

Rule (b): Unquoted shell variable expansions of env vars that hold user-controlled inputs. In the 'Packages compilation' step, `$CONDA_BUILD_ARGS` (from inputs.conda_build_args) and `$CONDA_CONVERT_ARGS` (from inputs.conda_convert_args) are expanded unquoted in command invocations: `command conda "$build_function" . --no-anaconda-upload --output-folder "$out_dir" $CONDA_BUILD_ARGS` and `command conda convert "$host_package" -o "$out_dir" $CONDA_CONVERT_ARGS $platforms_options`. In the 'Packages uploading' step, `$ANACONDA_UPLOAD_ARGS` (from inputs.anaconda_upload_args) is expanded unquoted: `anaconda upload --user "$ANACONDA_USER" $force --label "$label" $ANACONDA_UPLOAD_ARGS "$package_path"`. Although the comment says these are intentionally unquoted for word-splitting, unquoted expansion of user-controlled values allows shell metacharacter injection (`;`, `|`, `&`, `$(...)`, backticks, glob chars).

Locations:

- `action.yml:228`
- `action.yml:258`
- `action.yml:295`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed both script-injection findings in action.yml:

1. Rule (a) - 7 occurrences of `${{ contains(inputs.xxx, '...') }}` in the 'Sanity checks on inputs' step: Added CONDA_BUILD_ARGS, CONDA_CONVERT_ARGS, and ANACONDA_UPLOAD_ARGS to the step's env: block, then replaced all ${{ contains(...) }} shell conditions with bash [[ "$VAR" == *"substring"* ]] pattern matching. No ${{ }} expressions remain in any run: block.

2. Rule (b) - 3 occurrences of unquoted user-controlled args expansions: Replaced $CONDA_BUILD_ARGS, $CONDA_CONVERT_ARGS, and $ANACONDA_UPLOAD_ARGS unquoted expansions with xargs-based tokenization into bash arrays (conda_build_args_array, conda_convert_args_array, anaconda_upload_args_array). Each uses the prescribed guard+xargs+NUL-delimited read pattern to safely split whitespace-separated argument lists while preserving quoted substrings and preventing shell metacharacter injection. The internally-constructed $platforms_options variable was left unquoted as it is not user-controlled.

### Iteration 2

**Fixes applied:** github-env-injection

**Notes:**

Fixed the 'Write structured producer evidence' step in action.yml by adding sanitization of the `output` and `artifact_name` variables before writing them to $GITHUB_OUTPUT. Added two intermediate variables (`safe_output` and `safe_artifact_name`) that strip newlines using `printf '%s' "$VAR" | tr -d '\n\r'`, then use those sanitized values in the `echo` statements that write to $GITHUB_OUTPUT.

