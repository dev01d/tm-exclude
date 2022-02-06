# Time Machine exclude [![version badge](https://img.shields.io/github/release/dev01d/tm-exclude.svg?style=flat)](https://github.com/dev01d/tm-exclude/releases)

This is a `launchd` plist wrapper that uses `find` and `tmutil` to recursively search for and exclude specified directories from your Time Machine backups.

> This example is for the node_modules directory but will work on any other reoccurring dev dependency.

This plist watches each directory specified for state change, runs and then sleeps for 30 seconds so the system doesn't throttle it.

## Usage

- Open with Xcode or any text editor

1. Edit the variable `<string>WORKINGDIR=$HOME/your-working-dir</string>` so it points to the parent folder of your working directory

2. Edit the string under `<key>WatchPaths</key>` and be sure to include trailing slash

   - Multiple `<string>~/directory/to/monitor/</string>` entries supported

3. Move plist to `~/Library/LaunchAgents/`

4. Load config with this command

```bash
launchctl load ~/Library/LaunchAgents/com.user.time-machine-exclude.plist
```

- Done.

Whenever a monitored directory's state changes the script will run removing old exclusions then adding new ones

## Example

```shell
~/Code
  ├── Go
  ├── JavaScript
  ├── K8s
  ├── Python
  └── Websites
```

- `<string>WORKINGDIR=$HOME/Code</string>` for the above location

Then dirs to watch (WatchPaths) would be:

- `<string>~/Code/JavaScript/</string>`
- `<string>~/Code/Websites/</string>`

## Test

To see if the plist was effective run this command.

```bash
WORKINGDIR=$HOME/your-working-dir \
&& find $WORKINGDIR -type d -name node_modules -prune -exec tmutil isexcluded {} +
```

**Note:** I haven't noticed any absurd resource usage but for those concerned please see [v1.0.0](https://github.com/dev01d/tm-exclude/releases/tag/1.0.0).
