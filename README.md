# spy (beta)

See what a program really does on your machine.

```
spy <executable> [args...]
```

## Download

Get the latest version for macOS, Linux, or Windows from [Releases](https://github.com/naviko/spy/releases/latest).

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

## spyview

`spyview` is the terminal viewer for spy logs.

- Tabs for web requests, connections, files, processes, and all events
- Content shown decoded, raw, or hex
- Filter, search, group, mark, label, and export
- Follow a log live
- Keyboard and mouse

## License

Free to use. No modification or reverse engineering. See [LICENSE](LICENSE).
