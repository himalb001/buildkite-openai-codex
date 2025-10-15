# Buildkite Pipeline with OpenAI Codex

A Buildkite pipeline that uses the OpenAI Codex CLI to interact with Buildkite's hosted AI models.

## How It Works

The pipeline uses a pre-command hook to set up the Codex CLI:

1. Installs Node.js and npm (if not available)
2. Installs the `@openai/codex` CLI
3. Creates a `~/.codex/config.toml` that points to Buildkite's hosted models endpoint

The config uses `BUILDKITE_AGENT_ACCESS_TOKEN` for authentication and `BUILDKITE_AGENT_ENDPOINT/ai/openai/v1` as the base URL.

## Repository Structure

```
.
├── .buildkite/
│   ├── hooks/
│   │   └── pre-command          # Sets up Codex CLI and configuration
│   └── pipeline.yml              # Pipeline steps
└── README.md
```

## Usage

Upload the pipeline:

```bash
buildkite-agent pipeline upload
```

The example pipeline sends a prompt to generate a Python hello world function using Codex.

## Adding More Steps

```yaml
steps:
  - label: ":rust: Generate Rust Code"
    command: |
      echo "Write a function to calculate fibonacci in Rust" | codex exec

  - label: ":test_tube: Generate Tests"
    command: |
      echo "Write unit tests for a REST API" | codex exec
```

## Configuration

Edit `.buildkite/hooks/pre-command` to change the model or reasoning level:

```bash
cat > ~/.codex/config.toml <<EOF
model_provider = "hosted-models"
name = "gpt-5-codex"
reasoning_level = "high"
...
EOF
```
