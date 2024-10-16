# Roadmap.sh-003
Bash script to archive/extract

### How to run
Save `archive.sh` , make the file executable and do `./archive.sh`.

or

Install as a program you can call anytime from bash.

Save the script into `$HOME/.local/bin` using:

```bash
cat << 'EOF' > "$HOME/.local/bin/archive"
#!/bin/bash

# Function to create an archive
create_archive() {
  ARCHIVE_DIR="${1:-$(pwd)}"

  # Get the absolute path and ensure the directory exists
  ARCHIVE_DIR=$(realpath "$ARCHIVE_DIR")
  if [ ! -d "$ARCHIVE_DIR" ]; then
    echo "Error: Directory '$ARCHIVE_DIR' does not exist."
    exit 1
  fi

  # Create a timestamped archive name
  TIMESTAMP=$(date +"%Y%m%d_%H%M%S")
  ARCHIVE_NAME=$(basename "$ARCHIVE_DIR")_$TIMESTAMP.tar.gz

  # Archive the directory
  tar -czf "$ARCHIVE_NAME" -C "$(dirname "$ARCHIVE_DIR")" "$(basename "$ARCHIVE_DIR")"
  echo "Created archive: $ARCHIVE_NAME"
}

# Function to extract an archive
extract_archive() {
  ARCHIVE_FILE="$1"
  EXTRACT_DIR="${2:-$(pwd)}"

  # Ensure the archive file exists
  if [ ! -f "$ARCHIVE_FILE" ]; then
    echo "Error: Archive file '$ARCHIVE_FILE' does not exist."
    exit 1
  fi

  # Ensure the extract directory exists
  EXTRACT_DIR=$(realpath "$EXTRACT_DIR")
  if [ ! -d "$EXTRACT_DIR" ]; then
    echo "Error: Extract directory does not exist. 

Tip: Omit the directory option to extract to current directory."
    exit 1
  fi

  # Extract the archive
  tar -xzf "$ARCHIVE_FILE" -C "$EXTRACT_DIR"
  echo "Extracted to: $EXTRACT_DIR"
}

# Call the script 
if [ "$1" == "extract" ]; then
    extract_archive "$2" "$3"
elif [ -z "$1" ]; then
    echo "Simple utility that archives directories and extracts them using tar.

Usage: archive <directory> | archive extract <archive_file> [<optional_extract_directory>]"
    exit 1
else
    create_archive "$1"
fi

EOF
```

Make the saved program executable

```bash
chmod +x "$HOME/.local/bin/archive"
```

Import the bin directory into PATH if not already imported 

```bash 
[[ ":$PATH:" != *":$HOME/.local/bin:"* ]] && { export PATH="$HOME/.local/bin:$PATH"; source ~/.bashrc; }
```

Run the program
```bash
archive
```

### How to uninstall 
```bash
rm $HOME/.local/bin/archive
```
