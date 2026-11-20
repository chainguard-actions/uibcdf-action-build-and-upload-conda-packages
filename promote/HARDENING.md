<!-- markdownlint-disable -->

# Hardening Report: uibcdf--action-build-and-upload-conda-packages--promote/v2.2.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `374911c4be584df24e6da57af2d990570ef0f859`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **uibcdf--action-build-and-upload-conda-packages--promote/v2.2.2** was hardened automatically. 6 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a): the composite step 'Promote and verify the exact package' puts input expressions straight into its run: command: --package-spec "${{ inputs.package-spec }}", --expected-sha256 "${{ inputs.expected-sha256 }}", --from-label "${{ inputs.from-label }}", --to-label "${{ inputs.to-label }}". These values are substituted into the script before bash runs it, so a value containing a double quote or $(...) can inject shell commands. Fix: pass each input through env: (for example PACKAGE_SPEC: ${{ inputs.package-spec }}) and reference it in the script as "$PACKAGE_SPEC".

Locations:

- `action.yml:45`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.package-spec }}" appears directly in run: block of step "Promote and verify the exact package"; move to env: map

Locations:

- `action.yml:44`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.expected-sha256 }}" appears directly in run: block of step "Promote and verify the exact package"; move to env: map

Locations:

- `action.yml:44`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.from-label }}" appears directly in run: block of step "Promote and verify the exact package"; move to env: map

Locations:

- `action.yml:44`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.to-label }}" appears directly in run: block of step "Promote and verify the exact package"; move to env: map

Locations:

- `action.yml:44`

### prior-fix (severity: high)

Carried forward from this version's previous harden, which fixed it; fix it again unless the code no longer has the problem. The previous harden changed action.yml without recording a finding for it, and this analysis reported nothing there. Re-apply its security changes unless the code no longer needs them. The previous patch:
```diff
diff --git a/promote/action.yml b/promote/action.yml
index d53ec10..16e5a6e 100644
--- a/promote/action.yml
+++ b/promote/action.yml
@@ -35,15 +35,31 @@ outputs:
 runs:
   using: 'composite'
   steps:
     - name: Promote and verify the exact package
       id: promote
       shell: bash -l {0}
       env:
         ANACONDA_API_TOKEN: ${{ inputs.token }}
+        PACKAGE_SPEC: ${{ inputs.package-spec }}
+        EXPECTED_SHA256: ${{ inputs.expected-sha256 }}
+        FROM_LABEL: ${{ inputs.from-label }}
+        TO_LABEL: ${{ inputs.to-label }}
       run: >-
-        python "$GITHUB_ACTION_PATH/../scripts/promote_conda_package.py"
-        --package-spec "${{ inputs.package-spec }}"
-        --expected-sha256 "${{ inputs.expected-sha256 }}"
-        --from-label "${{ inputs.from-label }}"
-        --to-label "${{ inputs.to-label }}"
-        --output "$RUNNER_TEMP/conda-promotion-receipt.json"
+        python "$GITHUB_ACTION_PATH/../scripts/promote_conda_package.py" --package-spec "$PACKAGE_SPEC" --expected-sha256 "$EXPECTED_SHA256" --from-label "$FROM_LABEL" --to-label "$TO_LABEL" --output "$RUNNER_TEMP/conda-promotion-receipt.json"

```

Locations:

- `action.yml`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection, prior-fix

**Notes:**

Moved inputs.package-spec, expected-sha256, from-label and to-label into the step's env: block as PACKAGE_SPEC, EXPECTED_SHA256, FROM_LABEL and TO_LABEL. The script now references them as double-quoted shell variables. The arguments and the way the command is called are unchanged.

