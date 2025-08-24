# Confluence Asciidoctor Publisher

This tool lets you **publish AsciiDoc or Confluence XML/XHTML content** directly into Confluence using the REST API.  
It also supports downloading pages, dumping raw XHTML, and managing attachments.  
Authentication is via **Bearer token**.

## Features

- Publish `.txt` files written in **AsciiDoc** (converted via `asciidoctor` to XHTML)
- Publish `.xml` files (Confluence storage format, pre-processed for safety)
- Download existing Confluence pages (`dump` / `download`)
- Attach images referenced in your documents
- Initialize a local `.confluence-asciidoctor/config.toml` config
- Authentication via Bearer token (`--token` or `CONFLUENCE_TOKEN` env var)

## Installation

Ensure you have:
- Python ≥3.11
- `lxml`
- `urllib3`
- `asciidoctor` command in your PATH (for `.txt` input)

Clone this repository and run the script directly:

```bash
git clone https://github.com/your-org/confluence-asciidoctor.git
cd confluence-asciidoctor
./confluence-asciidoctor --help
```

## Usage

### 1. Initialize Config

Run once inside your project root:

```bash
./confluence-asciidoctor init --url https://your-confluence/display/SPACEKEY
```

This creates `.confluence-asciidoctor/config.toml`.

### 2. Set Authentication

Provide a bearer token either via environment variable:

```bash
export CONFLUENCE_TOKEN="your-token-here"
```

or on the command line:

```bash
./confluence-asciidoctor --token "your-token-here" <command> ...
```

### 3. Publish Pages

Publish a single AsciiDoc file:

```bash
./confluence-asciidoctor publish docs/intro.txt
```

Publish under a specific parent page:

```bash
./confluence-asciidoctor publish -p "Parent Page Title" docs/intro.txt
```

Publish multiple files at once:

```bash
./confluence-asciidoctor publish docs/intro.txt docs/setup.txt docs/usage.txt
```

Rules:
- `.txt` → processed with Asciidoctor → Confluence XHTML
- `.xml` → parsed/normalized as Confluence storage format
- Directories passed as arguments are skipped

### 4. Download Pages

Dump an existing page’s storage XHTML:

```bash
./confluence-asciidoctor dump "Page Title"
```

or using the alias:

```bash
./confluence-asciidoctor download "Page Title"
```

### 5. Dump Local XHTML

Convert a local AsciiDoc file to Confluence XHTML and print it (no upload):

```bash
./confluence-asciidoctor dumpxhtml docs/intro.txt
```

## Example Workflow

```bash
# Initialize
./confluence-asciidoctor init --url https://wiki.company.com/display/DEV

# Publish
./confluence-asciidoctor --token "$CONFLUENCE_TOKEN" publish -p "Project Docs" docs/*.txt

# Download
./confluence-asciidoctor --token "$CONFLUENCE_TOKEN" dump "Project Docs"
```

## Config File

`.confluence-asciidoctor/config.toml` example:

```toml
[default]
base_url = "https://wiki.company.com/display/DEV"
space_key = "DEV"
code_theme = "Confluence"
```
