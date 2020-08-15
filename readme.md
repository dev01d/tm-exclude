# Time Machine exclude [![version badge](https://img.shields.io/github/release/dev01d/tm-exclude.svg?style=flat)](https://github.com/dev01d/tm-exclude/releases)

This is a `launchd` plist wrapper that uses `find` and `tmutil` to recursively search for and exclude specified directories from your Time Machine backups.

> This example is for the node_modules directory but will work on any other reoccurring dev dependency.

This plist watches each directory specified for state change, runs and then sleeps for 30 seconds so the system doesn't throttle it.

## Usage

- Open with Xcode or any text editor

- Edit the command so `cd` points to your working directory

- Edit the string under `<key>WatchPaths</key>` and be sure to include trailing slash

  > Multiple `<string>~/directory/to/monitor/</string>` entries supported

- Move plist to `~/Library/LaunchAgents/`

- Load config with this command

```bash
launchctl load ~/Library/LaunchAgents/com.user.time-machine-exclude.plist
```

- Done.

Whenever the directory's state changes the script will run removing old exclusions then adding new ones

> To see if the plist was effective run this command.
>
> ```bash
> cd "$HOME/your-working-dir"; \
> find $(pwd) -type d -name node_modules -prune -exec tmutil isexcluded {} \;
> ```

**Note:** I haven't noticed any absurd resource usage but for those concerned please see [v1.0.0](https://github.com/dev01d/tm-exclude/releases/tag/1.0.0).
