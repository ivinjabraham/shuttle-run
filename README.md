# Shuttle Deploy Workflow

This initially started out as a replacement to the official [shuttle-hq/deploy-action](https://github.com/shuttle-hq/deploy-action) as `cargo shuttle deploy` used to fail if the deployment was already running.

Now, it is meant to be used on PRs to ensure it can be deployed on shuttle by testing it with a local run i.e `cargo shuttle run`.
 
## Inputs

### `cargo-shuttle-version`
- **Description**: Specify a version of cargo-shuttle to install.
- **Required**: No
- **Default**: (Latest version if not specified)
- **Example**: `0.46.0`

### `secrets`
- **Description**: Contents of the `Secrets.toml` file used for deployment.
- **Required**: No
- **Example**: `token = "mysecrettoken"`

## Workflow Steps

1. **Install Dependencies**: Installs necessary tools (`cargo binstall`) for managing Cargo binaries.

2. **Install cargo-shuttle**:
   - Installs the latest version of `cargo-shuttle` if `cargo-shuttle-version` is not specified.
   - Installs a specific version of `cargo-shuttle` if `cargo-shuttle-version` is provided.

3. **Create `Secrets.toml`** (if `secrets` provided):
   - Creates a `Secrets.toml` file with the provided contents.

4. **Run Project**:
   - Runs `cargo shuttle run`

## Example Usage

```yaml
uses: username/repo/path/to/shuttle-deploy
with:
  cargo-shuttle-version: '0.5.1'
  secrets: ${{ secrets.SECRETS_FILE_CONTENT }}
```
