# Technical Report: Customizing Oh My Posh to Display a Unicorn Icon Instead of the Username

## 1. Objective

The objective of this configuration is to customize the terminal prompt using Oh My Posh by replacing the displayed username with a unicorn icon (🦄).

**Expected result:**

Default terminal prompt:
```yaml
inititalusername: ~/Documents  ❯
```

Customized terminal prompt:
```
🦄  ~/Documents  ❯
```

## 2. Environment and Software

The following environment was used:

| Component            | Configuration        |
| -------------------- | -------------------- |
| Operating system     | macOS                |
| Shell                | Zsh                  |
| Oh My Posh           | Version 31.9.1       |
| Configuration format | JSON                 |
| Terminal editor      | Pico / Nano          |
| Configuration file   | `~/mytheme.omp.json` |

## 3. Step-by-Step Implementation

### Step 1: Verify Oh My Posh installation

Check the installed version:
```
oh-my-posh version
```

Expected output:
```
31.9.1
```

### Step 2: Identify the active shell

Execute:
```bash
echo "$SHELL"
```

Expected output:
```
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

This identifies the Oh My Posh initialization command.

### Step 4: Export the existing theme

Create a separate configuration file:
```arduino
oh-my-posh config export --output ~/mytheme.omp.json
```

This exports the current theme into JSON format, allowing customization without directly editing the original configuration.

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
```
nano ~/mytheme.omp.json
```

Locate the `palette` section and the following configuration sections.

### Step 7: Add the unicorn username mapping

Find the closing section of the palette:
```json
"yellow": "#F3AE35"
},
```

Immediately after it, add:
```json
"maps": {
  "user_name": {
    "inititalusername": "🦄"
  }
},
```

The resulting structure should look like this:
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
    "inititalusername": "🦄"
  }
},
"secondary_prompt": {
```

**Explanation:**

- `maps`: Defines replacement values for specific template properties.
- `user_name`: Identifies the username property.
- `inititalusername`: The original macOS username.
- `🦄`: The replacement symbol.

This mapping allows Oh My Posh templates to display the unicorn instead of the original username when they use the mapped username value.

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
```
JSON is valid
```

This verifies that the JSON syntax is correct.

### Step 10: Configure Zsh to load the customized theme

Add the Oh My Posh initialization command to `.zshrc`:
```bash
echo 'eval "$(oh-my-posh init zsh --config ~/mytheme.omp.json)"' >> ~/.zshrc
```

This instructs Zsh to load the customized theme.

**Note:** If an existing Oh My Posh initialization command is already present, replace it rather than keeping two initialization commands.

### Step 11: Reload the terminal configuration

Execute:
```bash
source ~/.zshrc
```

This reloads the Zsh configuration and applies the customized Oh My Posh theme.

## 4. Verification

Check the terminal prompt after reloading.

Expected appearance:
```
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

The mapping and the template must work together for the replacement to appear.

## 5. Troubleshooting

| Problem                     | Recommended solution                                              |
| --------------------------- | ----------------------------------------------------------------- |
| JSON validation fails       | Check commas, quotation marks and braces.                         |
| Username still appears      | Verify the session segment uses `{{ .UserName }}`.                |
| Unicorn appears as a square | Install and configure a Nerd Font or a font supporting the emoji. |
| Theme does not load         | Check the initialization command in `~/.zshrc`.                   |
| Wrong theme appears         | Verify the `--config` path.                                       |

To restore the exported backup:
```bash
cp ~/mytheme.omp.json.bak ~/mytheme.omp.json
```

## 6. Conclusion

The customization process demonstrates how Oh My Posh can be adapted to create a personalized terminal interface.

The main steps include:

1. Identifying the shell and Oh My Posh version.
2. Exporting the current theme.
3. Creating a backup.
4. Editing the JSON configuration.
5. Adding a username mapping.
6. Validating the JSON file.
7. Configuring Zsh to load the custom theme.
8. Reloading and verifying the terminal prompt.

The final goal is to replace the traditional username display with a unicorn icon (🦄), while preserving the existing theme colors and other prompt elements.

**Result:** A personalized Oh My Posh terminal prompt on macOS using Zsh.
