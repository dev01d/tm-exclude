# Time Machine exclude

This is a `launchd` plist wrapper that uses `find` and `tmutil` to recursively search for and exclude specified directories from your Time Machine backups.

> This example is for the node_modules directory but will work on any other reoccurring dev dependency.

**Usage**:

- Open with Xcode or any text editor

- Edit the command so `cd` points to your working directory

- Change hour and minute values to a time you are usually actively using your computer.

- Move plist to `~/Library/LaunchAgents/`

- Load config with this command

```bash
launchctl load ~/Library/LaunchAgents/com.user.time-machine-exclude.plist
```

- Wait for interval

> After interval to see if the plist was effective run this command.
>
> ```bash
> cd "$HOME/your-working-dir"; \
> find $(pwd) -type d -name node_modules -prune -exec tmutil isexcluded item {} \;
> ```
