# Events Commit Script

The Code -> [Github Gist](https://gist.github.com/SajagIN/c78f6813fb34f375c5786847af27d894)

This Gist contains a Python script that commits historical event data to a GitHub repository using backdated commits.

## Overview

The script performs the following tasks:
- Checks if the GitHub repository exists; if not, it creates the repository.
- Creates an initial commit with a backdated README file.
- Fetches historical events from an external API based on dates.
- Commits each event as a separate backdated commit with the event's date and description.
- Utilizes a persistent state file (`.events_state.json`) and a log file (`events.log`) to resume operations after interruptions.
- Optimizes the resume process by parsing the last committed event from the log file.

## How It Works

1. **Repository Initialization**: 
   - The script checks for the existence of the target repository using the GitHub API.
   - If the repository is empty, an initial commit is created using a temporary local git repository with custom backdated commit dates.

2. **Event Fetching & Commit Process**:
   - For each day in 1970, historical events are fetched concurrently using a `ThreadPoolExecutor`.
   - Each event is committed sequentially with its own backdated commit; the commit message includes the event date and a truncated description.

3. **Persistent State & Resuming**:
   - A persistent state is maintained in `.events_state.json` to track committed events.
   - The log file (`events.log`) records commit operations, enabling fast resume by parsing the last committed event.

4. **Diagnostics and Error Handling**:
   - A diagnostic mode (`--diagnose`) is available to inspect repository status, the latest commit details, and GitHub API rate limits.

## Usage

- **Run the Script**:
  ```bash
  python main.py
  ```

- **Run Diagnostics**:
  ```bash
  python main.py --diagnose~
  ```

## Configuration

- **GitHub Settings**: 
  The GitHub token, repository owner, and repository name are configured at the top of `main.py`.

- **Logging and State Files**:
  - `events.log`: Logs commit operations and status messages.
  - `.events_state.json`: Stores persistent state to support resuming operations.

## Dependencies

- Python 3.x
- `requests` library
- Standard libraries: `json`, `hashlib`, `datetime`, `concurrent.futures`, etc.

## Notes

- Ensure that the GitHub token is kept secure.
- This script is intended for generating historical commits and should be adapted as needed.

## License

This project is provided "as-is" and may require modifications for production use.