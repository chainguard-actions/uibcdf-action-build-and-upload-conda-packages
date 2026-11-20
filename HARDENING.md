<!-- markdownlint-disable -->

# Hardening Report: uibcdf--action-build-and-upload-conda-packages/v2.2.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `374911c4be584df24e6da57af2d990570ef0f859`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **uibcdf--action-build-and-upload-conda-packages/v2.2.2** was hardened automatically. 5 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a): the 'Sanity checks on inputs' step puts GitHub Actions expressions straight into its run: script, for example `if ${{ contains(inputs.conda_build_args, '--no-anaconda-upload') }}; then`. The same pattern is used for inputs.conda_convert_args and inputs.anaconda_upload_args (--label, -l, --user, -u, --token, --force). contains() only ever returns true or false, but the check forbids any ${{ }} expression inside a run: block. Fix: do the substring test in shell against the env vars, e.g. `[[ "$CONDA_BUILD_ARGS" == *--no-anaconda-upload* ]]`.

Locations:

- `action.yml:155`

### script-injection (severity: high)

Rule (b): the 'Packages compilation' step expands values that come from workflow inputs without quotes, in `conda "$build_function" . --no-anaconda-upload --output-folder "$out_dir" $CONDA_BUILD_ARGS` and `conda convert "$host_package" -o "$out_dir" $CONDA_CONVERT_ARGS $platforms_options`. CONDA_BUILD_ARGS and CONDA_CONVERT_ARGS hold inputs.conda_build_args and inputs.conda_convert_args. Neither line is an array assignment with a shellcheck SC2086/SC2206 disable directive, so the exemption does not apply. Fix: split each one into an array on a line marked with that directive, e.g. `# shellcheck disable=SC2206` then `build_args=($CONDA_BUILD_ARGS)`, and pass it as "${build_args[@]}".

Locations:

- `action.yml:251`
- `action.yml:290`

### script-injection (severity: high)

Rule (b): the 'Packages uploading' step expands the input-sourced $ANACONDA_UPLOAD_ARGS (inputs.anaconda_upload_args) without quotes in `if anaconda upload --user "$ANACONDA_USER" $force --label "$label" $ANACONDA_UPLOAD_ARGS "$package_path"; then`. It is not an array assignment with a shellcheck directive, so the exemption does not apply. Fix: use the same array-assignment approach as above.

Locations:

- `action.yml:341`

### github-env-injection (severity: high)

The 'Write structured producer evidence' step writes values built from untrusted input to $GITHUB_OUTPUT without the `printf '%s' ... | tr -d '\n\r'` sanitization step. `output` and `artifact_name` both contain $artifact_index, which comes from inputs.evidence_matrix_index, and $SUBJECT_JOB_KEY, which comes from github.job. They are written with `echo "path=$output" >> "$GITHUB_OUTPUT"` and `echo "artifact_name=$artifact_name" >> "$GITHUB_OUTPUT"`. A newline in evidence_matrix_index would let a caller add arbitrary step outputs. Fix: run each value through the sanitization step before writing it.

Locations:

- `action.yml:400`
- `action.yml:401`

### prior-fix (severity: high)

Carried forward from this version's previous harden, which fixed it; fix it again unless the code no longer has the problem. The previous harden changed action.yml without recording a finding for it, and this analysis reported nothing there. Re-apply its security changes unless the code no longer needs them. The previous patch:
```diff
diff --git a/action.yml b/action.yml
index d939184..d5cd69b 100644
--- a/action.yml
+++ b/action.yml
@@ -121,10 +121,26 @@ outputs:
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
@@ -133,6 +149,9 @@ runs:
         ANACONDA_USER: ${{ inputs.user }}
         LABEL: ${{ inputs.label }}
         OVERWRITE: ${{ inputs.overwrite }}
+        CONDA_BUILD_ARGS: ${{ inputs.conda_build_args }}
+        CONDA_CONVERT_ARGS: ${{ inputs.conda_convert_args }}
+        ANACONDA_UPLOAD_ARGS: ${{ inputs.anaconda_upload_args }}
       run: |
         set -euo pipefail
         echo "::group::Sanity checks on inputs"
@@ -148,36 +167,36 @@ runs:
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
+        if ([[ "$ANACONDA_UPLOAD_ARGS" == *"--label"* ]] || [[ "$ANACONDA_UPLOAD_ARGS" == *" -l "* ]] || [[ "$ANACONDA_UPLOAD_ARGS" == "-l "* ]] || [[ "$ANACONDA_UPLOAD_ARGS" == *" -l" ]] || [ "$ANACONDA_UPLOAD_ARGS" = "-l" ]) && [ -n "$LABEL" ]; then
             echo -e "Both the 'label' input and the '--label'/'-l' option within 'anaconda_upload_args' were specified.\n"\
                 "Please only use one of them."
             exit 1
         fi
-        if (${{ contains(inputs.anaconda_upload_args, '--user') }} || ${{ contains(inputs.anaconda_upload_args, '-u') }}) && [ -n "$ANACONDA_USER" ]; then
+        if ([[ "$ANACONDA_UPLOAD_ARGS" == *"--user"* ]] || [[ "$ANACONDA_UPLOAD_ARGS" == *" -u "* ]] || [[ "$ANACONDA_UPLOAD_ARGS" == "-u "* ]] || [[ "$ANACONDA_UPLOAD_ARGS" == *" -u" ]] || [ "$ANACONDA_UPLOAD_ARGS" = "-u" ]) && [ -n "$ANACONDA_USER" ]; then
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
          
... (truncated)
```

Locations:

- `action.yml`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, github-env-injection, prior-fix

**Notes:**

Sanity checks step: I mapped conda_build_args, conda_convert_args and anaconda_upload_args into env. Every ${{ contains(...) }} is now a bash [[ == *substr* ]] test, run under 'shopt -s nocasematch' because GitHub's contains() ignores case and is a plain substring match. Compilation and upload steps: the input argument lists are split into arrays with unquoted assignments under '# shellcheck disable=SC2206' (build_args, convert_args, upload_args). This splits and globs exactly as the earlier unquoted $VAR did. The arrays are expanded as ${arr[@]+"${arr[@]}"}, which stays safe under set -u on bash 3.2. The internal platform options and the --force flag also became arrays. Producer evidence step: output and artifact_name now go through printf '%s' | tr -d '\n\r' before they are written to GITHUB_OUTPUT. This re-applies the previous harden's changes.

### Iteration 2

**Fixes applied:** github-env-injection

**Notes:**

In action.yml, the paths list is now sanitized with printf '%s' "${paths[*]}" | tr -d '\n\r' before being written to GITHUB_OUTPUT in the 'Packages uploading' step. The same fix was applied to the built_paths write in the compilation step. out_dir comes from mktemp, and the *_json outputs are made with json.dumps, which escapes newlines, so they were left as they were.

