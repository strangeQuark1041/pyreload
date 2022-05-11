# `pyreload`

_Run commands when files change_

`pyreload` is a <100 line python script: [`reload.py`](reload.py)

When run, it watches a directory for changes to files and run commands

The intended use of `pyreload` is in a git repo for a software.

For config options, `pyreload` reads a `json` file, by default [`reload.json`](reload.json) located _in the same directory as the script_:

```json
{
    "root_dir": ".",
    "commands": {
        "pre": [],
        "structure": [],
        "all": []
    },
    "delay_seconds": 0.005,
    "exit_on_error": false,
    "clear_screen": true,
    "use_gitignore": true
}
```

## Usage

The script can be run optionally with a path to a json config file

Json Options:

- `root_dir` is the working directory in which to recursively look for files, by default the current directory of the shell instance
- `commands` is an object whose keys are events and whose values are lists of commands to be run when that event occurs:
  - `pre` commands run once at the start of the script, eg `cmake -B build` or `cmake --preset=dev`
  - `structure` commands run when the file structure changes, ie on adding, deleting, or removing files, eg `cmake -B build`
  - `all` commands run on structure changes, or when existing files are modified, eg `cmake --build build` or `make`
- `delay_seconds` is a duration in seconds to wait for during every iteration, before checking files
- `exit_on_error`: if an error occurs when running a command, throw a python exception and quit, by default `false`
- `clear_screen`: clear the terminal screen before running commands, by default `true`
- `use_gitignore`: find files using `git`, requires `root_dir` to be a git repo, by default `true`

## Installation

Just copy [`reload.py`](reload.py) and [`reload.json`](reload.json) into a directory in your project.

## Examples

- CMake:

    ```json
    {
        "commands": {
            "pre": ["cmake -B ./build"],
            "structure": ["cmake -B ./build"],
            "all": ["cmake --build ./build"]
        }
    }
    ```
