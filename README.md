# Technical Report: Customizing Oh My Posh to Display a Unicorn Icon Instead of the Username

## 1. Objective

The objective of this configuration is to customize the terminal prompt using Oh My Posh by replacing the displayed username with a unicorn icon (🦄).

This customization uses the username mapping feature in Oh My Posh while preserving the existing theme colors and other prompt elements.

**Expected result:**

Default terminal prompt (illustrative):

```text
YOUR_MAC_USERNAME: ~/Documents  ❯
```

Customized terminal prompt:

```text
🦄  ~/Documents  ❯
```

*Note: The default prompt appearance depends on the existing Oh My Posh theme.*

## 2. Environment and Software

The following environment was used:

| Component | Configuration |
|---|---|
| Operating system | macOS |
| Shell | Zsh |
| Oh My Posh | Version 31.9.1 |
| Configuration format | JSON |
| Terminal editor | Pico / Nano |
| Configuration file | `~/mytheme.omp.json` |

## 3. Step-by-Step Implementation

### Step 1: Verify Oh My Posh installation

Check the installed version:

```bash
oh-my-posh version
```

Expected output:

```text
31.9.1
```

### Step 2: Identify the active shell

Execute:

```bash
echo "$SHELL"
```

Expected output:

```text
/bin/zsh
```

This confirms that Zsh is the active login shell.

### Step 3: Locate the Oh My Posh initialization

Inspect the Zsh configuration:

```bash
grep -n 'oh-my-posh init' ~/.zshrc
```

Example output:

```bash
/Users/username/.zshrc:1:eval "$(oh-my-posh init zsh)"
```

This identifies the Oh My Posh initialization command in the Zsh configuration file.

The output is illustrative; the actual path and command may differ depending on the existing configuration.

### Step 4: Export the existing theme

Create a separate configuration file:

```bash
oh-my-posh config export --output ~/mytheme.omp.json
```

This exports the current default theme into JSON format, allowing customization without directly modifying the original theme.

If an existing custom theme is already configured, specify its configuration using the `--config` option:

```bash
oh-my-posh config export --config ~/existing-theme.omp.json --output ~/mytheme.omp.json
```

Replace `~/existing-theme.omp.json` with the actual path to your existing theme.

Verify that the file exists:

```bash
ls -lh ~/mytheme.omp.json
```

### Step 5: Create a backup

Before modifying the configuration, create a backup:

```bash
cp ~/mytheme.omp.json ~/mytheme.omp.json.bak
```

This makes it possible to restore the original exported theme if necessary.

### Step 6: Open the configuration file

Open the JSON configuration using the terminal editor:

```bash
nano ~/mytheme.omp.json
```

Locate the `palette` section and identify the closing bracket of the palette object.

The new `maps` property will be added at the top level of the JSON configuration, alongside properties such as `palette` and `blocks`.

### Step 7: Add the unicorn username mapping

To replace the displayed username with a unicorn icon (🦄), add a username mapping to the Oh My Posh JSON configuration.

**1. Identify your macOS username**

Before editing the configuration, open Terminal and execute:

```bash
whoami
```

Example output:

```text
alex
```

The output represents your macOS account username. You will use this exact value in the mapping.

**2. Locate the palette section**

Open `~/mytheme.omp.json` and find the closing section of the `palette` property:

```json
"yellow": "#F3AE35"
},
```

Immediately after the closing `palette` property, add the following configuration.

**3. Add the username mapping**

```json
"maps": {
  "user_name": {
    "YOUR_MAC_USERNAME": "🦄"
  }
},
```

Replace `YOUR_MAC_USERNAME` with the username obtained from the `whoami` command.

For example, if the command returns `alex`, the mapping should be:

```json
"maps": {
  "user_name": {
    "alex": "🦄"
  }
},
```

**4. Verify the resulting JSON structure**

The relevant configuration sections should look similar to the following:

```json
"palette": {
  "black": "#262B44",
  "blue": "#4B95E9",
  "green": "#59C9A5",
  "orange": "#F07623",
  "red": "#D81E5B",
  "white": "#E0DEF4",
  "yellow": "#F3AE35"
},
"maps": {
  "user_name": {
    "YOUR_MAC_USERNAME": "🦄"
  }
},
"secondary_prompt": {
```

This is an excerpt showing the relevant properties, not a complete standalone JSON file.

**Important:** The `maps` property must be placed at the top level of the JSON configuration, alongside `palette`, `blocks`, and other configuration properties. Do not place it inside the `palette` object.

**5. Understand how the mapping works**

- `maps`: Defines replacement values for specific template properties.
- `user_name`: Identifies the username mapping property.
- `YOUR_MAC_USERNAME`: Represents your actual macOS account username.
- `🦄`: The replacement symbol displayed instead of the original username.

For the mapping to work, the session segment must use the mapped username property.

For example:

```json
{
  "type": "session",
  "style": "plain",
  "template": "{{ .UserName }}"
}
```

The `{{ .UserName }}` template retrieves the username value, allowing Oh My Posh to apply the configured mapping.

**Additional note:** The mapping key must match the username returned by `whoami` exactly. If the session segment uses a different template property or does not display the mapped username, the unicorn replacement may not appear.

After completing this configuration, proceed to Step 8 to save the JSON file.

### Step 8: Save the configuration

In Pico or Nano:

1. Press `Ctrl + O` to save.
2. Press `Enter` to confirm the filename.
3. Press `Ctrl + X` to exit.

### Step 9: Validate the JSON configuration

Execute:

```bash
python3 -m json.tool ~/mytheme.omp.json >/dev/null && echo "JSON is valid"
```

Expected output:

```text
JSON is valid
```

This verifies that the JSON syntax is correct.

### Step 10: Configure Zsh to load the customized theme

Open the Zsh configuration file:

```bash
nano ~/.zshrc
```

Locate the existing Oh My Posh initialization command.

Replace the existing initialization command with:

```bash
eval "$(oh-my-posh init zsh --config ~/mytheme.omp.json)"
```

If no existing Oh My Posh initialization command is present, add the command at the end of the file.

This instructs Zsh to load the customized theme instead of the default theme.

**Important:** Keep only one active Oh My Posh initialization command in `~/.zshrc`. Do not add a second initialization command if one already exists.

### Step 11: Reload the terminal configuration

Execute:

```bash
source ~/.zshrc
```

This reloads the Zsh configuration and applies the customized Oh My Posh theme.

## 4. Verification

Check the terminal prompt after reloading.

Expected appearance:

```text
🦄  ~/Documents  ❯
```

If the original username still appears, inspect the theme's `blocks` and `segments` sections.

The username segment must use the mapped property, for example:

```json
{
  "type": "session",
  "style": "plain",
  "template": "{{ .UserName }}"
}
```

The mapping and the session template must work together for the replacement to appear.

Also verify that the mapping key matches the output of the `whoami` command.

## 5. Troubleshooting

| Problem | Recommended solution |
|---|---|
| JSON validation fails | Check commas, quotation marks, and matching braces. |
| Username still appears | Verify the `maps.user_name` configuration and the session template `{{ .UserName }}`. |
| Unicorn appears as a square | Check terminal emoji support and configure a font that supports the unicorn emoji. |
| Theme does not load | Check the Oh My Posh initialization command in `~/.zshrc`. |
| Wrong theme appears | Verify that the `--config` path points to the correct JSON file. |
| Prompt displays duplicate information | Check for duplicate session segments or multiple Oh My Posh initialization commands. |

**Font compatibility note:**

A Nerd Font can help display Powerline symbols and other terminal glyphs. However, emoji rendering also depends on the terminal application and its emoji-supporting font.

To restore the exported backup:

```bash
cp ~/mytheme.omp.json.bak ~/mytheme.omp.json
```

If you have already modified `~/.zshrc`, also check that its initialization command points to the intended configuration file.

Reload the configuration after restoring the backup:

```bash
source ~/.zshrc
```

## 6. Conclusion

The customization process demonstrates how Oh My Posh can be adapted to create a personalized terminal interface.

The main steps include:

1. Identifying the shell and Oh My Posh version.
2. Exporting the existing theme.
3. Creating a backup.
4. Editing the JSON configuration.
5. Adding a username mapping.
6. Validating the JSON file.
7. Configuring Zsh to load the custom theme.
8. Reloading and verifying the terminal prompt.

The final goal is to replace the traditional username display with a unicorn icon (🦄), while preserving the existing theme colors and other prompt elements.

**Result:** A personalized Oh My Posh terminal prompt on macOS using Zsh.
