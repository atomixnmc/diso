# GitHub Actions Workflows

## Build Windows Wheels

The `build-windows-wheels.yml` workflow automatically builds Windows AMD64 wheels for the DISO package.

### Features

- **Platform**: Windows (AMD64 architecture)
- **Python Versions**: 3.10, 3.11
- **CUDA Versions**: 11.8, 12.1
- **Total Builds**: 4 wheel variants (2 Python versions × 2 CUDA versions)

### Trigger Events

The workflow runs on:
- Push to `main` or `develop` branches
- Pull requests targeting `main` or `develop` branches
- Manual trigger via GitHub Actions UI (`workflow_dispatch`)

### Build Matrix

Each build job installs:
1. Python (specified version)
2. CUDA Toolkit (specified version)
3. PyTorch with matching CUDA version
4. Required dependencies (trimesh, wheel, setuptools)

### Build Process

1. **Checkout**: Clones the repository
2. **Setup Python**: Installs the specified Python version
3. **Install CUDA**: Installs CUDA Toolkit with necessary components
4. **Verify CUDA**: Checks CUDA installation
5. **Install PyTorch**: Installs PyTorch with CUDA support
6. **Install Dependencies**: Installs build tools and package dependencies
7. **Build Wheel**: Compiles the package with CUDA extensions
8. **Upload Artifacts**: Uploads built wheels with unique names
9. **Test Installation**: Verifies the wheel can be installed and imported

### Artifacts

Built wheels are uploaded as artifacts with names following the pattern:
```
diso-windows-amd64-py{python_version}-cuda{cuda_version}-wheel
```

Examples:
- `diso-windows-amd64-py3.10-cuda11.8-wheel`
- `diso-windows-amd64-py3.11-cuda12.1-wheel`

Artifacts are retained for 30 days.

### Manual Workflow Execution

To manually trigger the workflow:
1. Go to the Actions tab in the GitHub repository
2. Select "Build Windows Wheels" workflow
3. Click "Run workflow"
4. Select the branch to build from
5. Click "Run workflow" button

### Notes

- The workflow uses `FORCE_CUDA=1` environment variable to ensure CUDA extensions are built even if CUDA is not automatically detected
- Each build job runs independently (`fail-fast: false`), so failures in one configuration won't stop others
- The workflow uses network installation method for CUDA to optimize installation time
