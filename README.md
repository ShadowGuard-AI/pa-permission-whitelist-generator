# pa-permission-whitelist-generator
Filters lines from a behavior file the operator supplies against patterns from a config file, and writes the kept lines as a JSON allowed_paths list. It performs no observation or monitoring: there is no monitoring period, no system call capture, and no file access capture. The input file is plain text with one line per entry, for example one file path per line. Blacklist patterns take precedence over whitelist patterns, and the result is deduplicated and sorted. - Focused on Tools for analyzing and assessing file system permissions

## Install
`git clone https://github.com/ShadowGuard-AI/pa-permission-whitelist-generator`
`pip install -r requirements.txt`

## Usage
`python3 main.py -i FILE [-o FILE] [-c FILE] [-d] [-s]`

`-i` is required. All other options are optional.

The config file is JSON. `--config` defaults to `config.json` in the current directory, and that file must exist and hold valid JSON or the run exits 1. It must hold a `whitelists` key with the patterns a line must match to be kept; a `blacklists` key is optional and its matches are dropped even when they also match the whitelist. Example config, also shipped as `config.example.json`:

```json
{
    "whitelists": ["/etc/*", "/tmp/*"],
    "blacklists": ["/etc/shadow"]
}
```

An input file with no entries, or a result with no allowed paths (for example a config with no whitelist keys), generates nothing: the run prints a plain nothing generated line and exits 1, and no empty allowed_paths file is written, because an empty allowlist applied anywhere allows nothing. The output file must not be the input file or the config file; such a run refuses at exit 1. With `-s` the whitelist is printed but not written.

## Parameters
- `-h`: Show help message and exit
- `-i`, `--input` (required): Path to the input file containing one behavior line (for example a file path) per line.
- `-o`, `--output` (optional): Path to the output file to store the generated permission whitelist (default: permission_whitelist.json).
- `-c`, `--config` (optional): Path to the configuration file containing filtering rules (default: config.json).
- `-d`, `--debug` (optional): Enable debug mode for more verbose logging.
- `-s`, `--simulate` (optional): Simulate the whitelist generation without writing to the output file.

## License
MIT License. See LICENSE.
