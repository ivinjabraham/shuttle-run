# Shuttle Run GitHub Action

A GitHub Action for testing [Shuttle](https://shuttle.dev/) Rust applications locally during CI workflows.

## Overview

This action runs your Shuttle project locally and verifies that it starts up correctly. It's ideal for:

- Validating your Shuttle project during CI/CD pipelines
- Ensuring your server responds properly before deploying

## Usage

Add this action to your workflow file:

```yaml
steps:
  - uses: actions/checkout@v4
  
  - name: Set up Rust
    uses: dtolnay/rust-toolchain@stable
    
  - name: Test Shuttle Application
    uses: ivinjabraham/shuttle-run-action@v2
    with:
      # Optional parameters
      timeout: 240
      secrets: |
        API_KEY="your-api-key"
        DATABASE_URL="postgres://user:password@localhost:5432/db"
      cargo-shuttle-version: "0.28.0"
```

## Inputs

| Parameter | Description | Required | Default |
|-----------|-------------|----------|---------|
| `cargo-shuttle-version` | Specific version of cargo-shuttle to install. If empty, uses the latest version. | No | `""` |
| `secrets` | Contents for the `Secrets.toml` file. Will be written to disk before running the project. | No | `""` |
| `timeout` | Maximum time (in seconds) to wait for the server to start before failing. | No | `180` |
| `port` | Port on which the server will run. | No | `8000` |
| `health-path` | Path to check for server health (e.g., /health). | No | `/` |

## How It Works

1. Installs `cargo-binstall` for efficient Rust binary installation
2. Installs `cargo-shuttle` (specific version or latest)
3. Creates a `Secrets.toml` file if secrets are provided
4. Runs your Shuttle project locally with `cargo shuttle run`
5. Checks if the server starts up properly on localhost:8000 or the specified port.
6. Fails if the server doesn't respond within the specified timeout

## Example: Complete Workflow

Here's a complete example showing how to integrate this action into a workflow:

```yaml
name: Shuttle Test

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Set up Rust
        uses: dtolnay/rust-toolchain@stable
        
      - name: Install Dependencies
        run: |
          sudo apt-get update
          sudo apt-get install -y libssl-dev pkg-config
      
      - name: Cargo Build
        run: cargo build
      
      - name: Test with cargo
        run: cargo test
      
      - name: Test Shuttle Application
        uses: ivinjabraham/shuttle-run-action@v2
        with:
          timeout: 120
          secrets: |
            TEST_MODE="true"
            DATABASE_URL="sqlite::memory:"
```

## Troubleshooting

### Common Issues

- **Timeout errors**: If your application takes longer than the default timeout (180 seconds) to start, increase the `timeout` parameter.
- **Port conflicts**: Ensure port 8000 is available in your CI environment.
- **Missing secrets**: Double-check that you've provided all required secrets for your application.

## License

MIT

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.
