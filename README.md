# spy (beta)

See what a program really does on your machine.

```
spy <executable> [args...]
```

## Download

With Homebrew:

```
brew install naviko/tap/spy        # the scanner
brew install naviko/tap/spyview    # the log viewer
```

Or get the latest version for macOS, Linux, or Windows from [Releases](https://github.com/naviko/spy/releases/latest).  Linux and Windows need additional testing. 

## Features

- **Works with any program.** Put `spy` in front of the command. The program runs as usual, fully interactive.
- **Decrypted web traffic.** Every HTTP and HTTPS request and response, with full bodies, streams, and WebSocket messages.
- **Network.** Every connection and DNS lookup, for any protocol.
- **Disk.** Every file opened, read, written, created, moved, copied, or deleted.
- **Processes.** Every program launched, with its arguments and exit code.
- **Follows child processes.**
- **Live JSON Lines log.** Stream it to a file or straight into your own tools.
- **Credentials hidden by default.**
- **Never blocks your program.**
- **macOS, Linux, and Windows.**

## Examples

```
spy curl -s https://example.com          # record a command
spy npm install                          # see what an installer touches and contacts
spy my-agent                             # watch an AI agent's requests, files, and commands
spy --events run.jsonl npm install       # choose the log file
spy --events - npm install | jq .        # stream events live into another tool
spy --no-redact curl -s https://example.com   # keep credentials in the log
```

```
spyview                                  # open the newest log
spyview run.jsonl                        # open a specific log
spyview --tail run.jsonl                 # follow a log while spy is writing it
```

## Options

```
spy [options] <executable> [args...]
```

| Option | Use |
|---|---|
| `--events <path>` | Write the log to this file. `--events -` streams it to standard output. Default: `./spy-<timestamp>-<id>.jsonl`. |
| `--env` | Use when a program's web traffic is not decrypted by default: routes its API endpoints through spy. |
| `--cert` | The other fallback: trusts spy's certificate system-wide for the length of the run. |
| `--config <path>` | Use this config file instead of the one for the executable's name. |
| `--no-redact` | Record credentials exactly as sent. |
| `--version` | Print the version. |

```
spy --events run.jsonl my-tool
spy --env my-tool
spy --cert my-tool
spy --config ./my-tool.toml my-tool
```

## Config file

Optional. Settings for one program, so you do not repeat options. spy loads the file named after the executable:

- macOS and Linux: `~/.config/spy/<executable>.toml`
- Windows: `%AppData%\spy\<executable>.toml`

Every setting is optional. This example shows all of them with their defaults:

```toml
# ~/.config/spy/my-tool.toml  (used for: spy my-tool)

args = []                      # extra arguments added after the executable name,
                               # for example ["--verbose"]

capture = "intercept"          # web traffic: "intercept" (default), "env" (same as --env),
                               # "cert" (same as --cert), or "off"

redact = true                  # false is the same as --no-redact
redact_headers = []            # more header names to hide, for example ["x-my-secret"]

disk            = true         # record file activity
network         = true         # record connections and DNS
network_content = true         # record what is sent and received
syscalls        = true         # record raw system calls (Linux only)

[set_env]                      # environment variables to set for the program
# MY_VAR = "value"

[endpoints]                    # for capture = "env": the variable a program reads its
                               # API address from, and the real address behind it
# MY_TOOL_API_URL = "https://api.example.com/v1"
```

Options on the command line override the file.

## spyview

`spyview` is the terminal viewer for spy logs.

View a log:

```
spyview                     # the newest log in this folder
spyview run.jsonl           # a specific log
```

Tail a log while spy is still writing it, from a second terminal:

```
spy --events run.jsonl my-tool      # terminal 1
spyview --tail run.jsonl            # terminal 2: new events appear as they happen
```

Press `?` inside spyview for the keys.

- Tabs for web requests, connections, files, processes, and all events
- Content shown decoded, raw, or hex
- Filter, search, group, mark, label, and export
- Follow a log live
- Keyboard and mouse

## License

Free to use. No modification or reverse engineering. See [LICENSE](LICENSE).
