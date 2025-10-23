# Publishing a Release from PR Branch

This directory contains the workflow for publishing a release directly from a PR branch.

## Workflow: publish-pr-release.yml

This workflow allows you to build and publish a release from any branch, including PR branches.

### Important Note About workflow_dispatch

⚠️ **GitHub Actions Limitation**: Workflows triggered by `workflow_dispatch` only appear in the Actions UI when they exist in the default branch (`main`). Since this workflow is in a PR branch, it won't show up in the Actions dropdown yet.

### Two Ways to Trigger the Release

#### Option 1: Push a Tag (Recommended for PR branches)

Push the release tag to trigger the workflow automatically:

```bash
git tag v0.30.3.1
git push origin v0.30.3.1
```

The workflow will automatically:
- Detect the tag
- Build the package
- Create the release
- Upload all artifacts

#### Option 2: Manual Trigger (After merging to main)

Once this PR is merged to `main`, the workflow will appear in the Actions tab:

1. Go to the **Actions** tab in the GitHub repository
2. Select **"Publish Release from PR Branch"** from the workflows list
3. Click **"Run workflow"**
4. Fill in the required inputs:
   - **tag**: The release tag (e.g., `v0.30.3.1`)
   - **prerelease**: Check this box if it's a pre-release (optional)
5. Click **"Run workflow"** to start the release process

### What it Does

The workflow will:
1. ✅ Checkout the current branch code
2. ✅ Build both wheel and source distribution
3. ✅ Verify the distributions with twine
4. ✅ Generate SHA256 checksums
5. ✅ Create a GitHub release with the specified tag
6. ✅ Upload all build artifacts:
   - Wheel file (`.whl`)
   - Source distribution (`.tar.gz`)
   - Checksums file (`checksums.txt`)

### Requirements

- The tag must match the version in `metakernel/__init__.py`
- You need write permissions to create releases
- The workflow runs on the current branch, so you can use it from PR branches

### Example for PR Branch

To publish version 0.30.3.1 from the PR branch:

```bash
# Create and push the tag
git tag v0.30.3.1
git push origin v0.30.3.1
```

The workflow will automatically run and create the release at:  
`https://github.com/jimwhite/metakernel/releases/tag/v0.30.3.1`
