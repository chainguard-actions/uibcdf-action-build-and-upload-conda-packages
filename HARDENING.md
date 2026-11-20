<!-- markdownlint-disable -->

# Hardening Report: uibcdf--action-build-and-upload-conda-packages/v2.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `374911c4be584df24e6da57af2d990570ef0f859`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **uibcdf--action-build-and-upload-conda-packages/v2.3.0** was hardened automatically. 5 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a): the 'Sanity checks on inputs' step puts GitHub Actions expressions straight into the run: script, for example `if ${{ contains(inputs.conda_build_args, '--no-anaconda-upload') }}; then`. The same pattern is used with inputs.conda_convert_args and inputs.anaconda_upload_args (--output-folder, --label/-l, --user/-u, --token, --force). These expressions only evaluate to true or false, but the check forbids any ${{ }} expression inside a run: block. Fix: set the contains() results as env: variables and compare the quoted variables in the script.

Locations:

- `action.yml:160`

### script-injection (severity: high)

Rule (b): the 'Packages compilation' step expands values from workflow inputs without quotes. `command conda "$build_function" . --no-anaconda-upload --output-folder "$out_dir" $CONDA_BUILD_ARGS` uses inputs.conda_build_args, and `command conda convert "$host_package" -o "$out_dir" $CONDA_CONVERT_ARGS $platforms_options` uses inputs.conda_convert_args. These are not array assignments with a shellcheck SC2086/SC2206 directive, so the exemption does not apply. Whitespace splitting and glob expansion happen on attacker-chosen input.

Locations:

- `action.yml:259`
- `action.yml:307`

### script-injection (severity: high)

Rule (b): the 'Packages uploading' step expands $ANACONDA_UPLOAD_ARGS (from inputs.anaconda_upload_args) without quotes: `anaconda upload --user "$ANACONDA_USER" $force --label "$label" $ANACONDA_UPLOAD_ARGS "$package_path"`. This is not an array assignment with a shellcheck SC2086/SC2206 directive, so the exemption does not apply.

Locations:

- `action.yml:358`

### github-env-injection (severity: high)

The 'Write structured producer evidence' step writes `echo "path=$output" >> "$GITHUB_OUTPUT"` and `echo "artifact_name=$artifact_name" >> "$GITHUB_OUTPUT"`. Both values are built from inputs.evidence_matrix_index (through SUBJECT_MATRIX_INDEX/artifact_index) and from github.* context values, and neither is passed through `printf '%s' ... | tr -d '\n\r'` first. A newline in the input could inject extra step outputs.

Locations:

- `action.yml:417`

### prior-fix (severity: high)

Carried forward from this version's previous harden, which fixed it; fix it again unless the code no longer has the problem. The previous harden changed action.yml without recording a finding for it, and this analysis reported nothing there. Re-apply its security changes unless the code no longer needs them. The previous patch:
```diff
diff --git a/action.yml b/action.yml
index 896f0bb..a26274c 100644
--- a/action.yml
+++ b/action.yml
@@ -123,10 +123,26 @@ outputs:
   evidence_artifact_name:
     description: Attempt-qualified artifact name reserved for uploading evidence_path.
     value: ${{steps.producer-evidence.outputs.artifact_name}}
-
 runs:
   using: "composite"
   steps:
     - name: Sanity checks on inputs
       shell: bash -l {0}
       env:
@@ -135,6 +151,9 @@ runs:
         ANACONDA_USER: ${{ inputs.user }}
         LABEL: ${{ inputs.label }}
         OVERWRITE: ${{ inputs.overwrite }}
+        CONDA_BUILD_ARGS: ${{ inputs.conda_build_args }}
+        CONDA_CONVERT_ARGS: ${{ inputs.conda_convert_args }}
+        ANACONDA_UPLOAD_ARGS: ${{ inputs.anaconda_upload_args }}
       run: |
         set -euo pipefail
         echo "::group::Sanity checks on inputs"
@@ -150,36 +169,36 @@ runs:
             exit 1
           fi
         fi
-        if ${{ contains(inputs.conda_build_args, '--no-anaconda-upload') }}; then
+        if [[ "$CONDA_BUILD_ARGS" == *"--no-anaconda-upload"* ]]; then
             echo -e "The argument '--no-anaconda-upload' is already used within this action's 'conda build' command and is not allowed in 'conda_build_args'."
             exit 1
         fi
-        if ${{ contains(inputs.conda_build_args, '--output-folder') }}; then
+        if [[ "$CONDA_BUILD_ARGS" == *"--output-folder"* ]]; then
             echo -e "The destination folder for the built packages is set internally and the '--output-folder' option is not allowed in 'conda_build_args'.\n"\
                 "To retrieve the paths of the built packages, please refer to this action's 'paths' output."
             exit 1
         fi
-        if ${{ contains(inputs.conda_convert_args, '--output-folder') }}; then
+        if [[ "$CONDA_CONVERT_ARGS" == *"--output-folder"* ]]; then
             echo -e "The destination folder for the converted packages is set internally and the '--output-folder' option is not allowed in 'conda_convert_args'.\n"\
                 "To retrieve the paths of the built packages, please refer to this action's 'paths' output."
             exit 1
         fi
-        if (${{ contains(inputs.anaconda_upload_args, '--label') }} || ${{ contains(inputs.anaconda_upload_args, '-l') }}) && [ -n "$LABEL" ]; then
+        if ([[ "$ANACONDA_UPLOAD_ARGS" == *"--label"* ]] || [[ "$ANACONDA_UPLOAD_ARGS" == *"-l"* ]]) && [ -n "$LABEL" ]; then
             echo -e "Both the 'label' input and the '--label'/'-l' option within 'anaconda_upload_args' were specified.\n"\
                 "Please only use one of them."
             exit 1
         fi
-        if (${{ contains(inputs.anaconda_upload_args, '--user') }} || ${{ contains(inputs.anaconda_upload_args, '-u') }}) && [ -n "$ANACONDA_USER" ]; then
+        if ([[ "$ANACONDA_UPLOAD_ARGS" == *"--user"* ]] || [[ "$ANACONDA_UPLOAD_ARGS" == *"-u"* ]]) && [ -n "$ANACONDA_USER" ]; then
             echo -e "Both the 'user' input and the '--user'/'-u' option within 'anaconda_upload_args' were specified.\n"\
                 "Please only use one of them."
             exit 1
         fi
-        if ${{ contains(inputs.anaconda_upload_args, '--token') }} && [ -n "$TOKEN" ]; then
+        if [[ "$ANACONDA_UPLOAD_ARGS" == *"--token"* ]] && [ -n "$TOKEN" ]; then
             echo -e "Both the 'token' input and the '--token' option within 'anaconda_upload_args' were specified.\n"\
                 "Please only use one of them."
             exit 1
         fi
-        if ${{ contains(inputs.anaconda_upload_args, '--force') }} && [ -n "$OVERWRITE" ]; then
+        if [[ "$ANACONDA_UPLOAD_ARGS" == *"--force"* ]] && [ -n "$OVERWRITE" ]; then
             echo -e "Both the 'overwrite' input and the '--force' option within 'anaconda_upload_args' were specified.\n"\
                 "Please only use one of them."
             exit 1
@@ -241,14 +260,16 @@ runs:
         else
           build_function="build"
         fi
-        # $CONDA_BUILD_ARGS and friends are intentionally unquoted so that several
-        # arguments s
... (truncated)
```

Locations:

- `action.yml`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, github-env-injection, prior-fix

**Notes:**

Sanity checks: moved all nine contains() expressions into env vars (BUILD_HAS_*, CONVERT_HAS_OUTPUT_FOLDER, UPLOAD_HAS_*) and compare with [ "$VAR" = "true" ]. GitHub still does the matching, so it stays case-insensitive like upstream. Compilation/upload: CONDA_BUILD_ARGS, CONDA_CONVERT_ARGS and ANACONDA_UPLOAD_ARGS are read into arrays with an unquoted assignment plus a shellcheck SC2206 directive, which keeps upstream's whitespace split and glob. The arrays are passed as ${arr[@]+"${arr[@]}"} so set -u does not fail on old bash when an input is empty. The platform -p options and --force are now arrays instead of unquoted strings. Evidence step: path and artifact_name go through printf | tr -d '\n\r' before being written to GITHUB_OUTPUT. This re-applies the previous harden's changes. Nothing was run or tested.

