# 🦄 Oh My Posh – Unicorn Username Customization

Give your macOS terminal a little personality! ✨

This project demonstrates how to customize **Oh My Posh** on macOS using Zsh by replacing the displayed username with a unicorn emoji (🦄), while preserving the existing theme colors and prompt elements.

## 🎯 Preview

**Before:**
```text
YOUR_MAC_USERNAME: ~/Documents ❯
```

**After:**
```text
🦄 ~/Documents ❯
```

## 🛠️ Environment

| Component | Configuration |
|---|---|
| Operating System | macOS |
| Shell | Zsh |
| Prompt Framework | Oh My Posh 31.9.1 |
| Configuration | JSON |

## ⚙️ How It Works

The customization uses the Oh My Posh JSON configuration:

- **Username mapping:** Maps the macOS username to 🦄.
- **Session segment:** Uses `{{ .UserName }}` to display the mapped value.
- **Zsh initialization:** Loads the customized theme through `.zshrc`.

The configuration is stored separately in `~/mytheme.omp.json`, keeping the original theme available for backup.

## 📚 Full Technical Documentation

The complete step-by-step guide covers:

- Checking the environment and exporting the theme.
- Backing up and editing the JSON configuration.
- Adding the unicorn username mapping.
- Configuring Zsh and validating JSON.
- Testing the result and troubleshooting common issues.

👉 **[Read the Full Technical Guide](https://gagaphone.github.io/UnicornCustomization/)**

## 🦄 Final Result

A small customization, a more personal terminal, and a little extra magic in everyday development!

---

*macOS · Zsh · Oh My Posh · Terminal Customization*
