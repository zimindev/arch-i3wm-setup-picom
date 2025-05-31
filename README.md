# 🌫️ Arch + i3wm + Picom + Setup (Blur, Rounded Corners, macOS-like Animations)

---

## ✅ What is Picom?

**Picom** is a lightweight compositor for X11 that adds effects like transparency, blur, shadows, rounded corners, and animations for window management, improving the visual experience in window managers like i3.

---

## 🖥️ Step 1: Install Proper GPU Drivers

Make sure you have the correct GPU drivers installed for smooth compositing and VSync support:

### For Intel/AMD GPUs (Integrated graphics)

```bash
sudo pacman -S mesa xf86-video-intel xf86-video-amdgpu
```

*(Install the one matching your GPU)*

### For NVIDIA GPUs (Proprietary drivers)

```bash
sudo pacman -S nvidia nvidia-utils nvidia-settings
```

---

## 🛠️ Step 2: Verify OpenGL Support

Install `mesa-demos` and check OpenGL info:

```bash
sudo pacman -S mesa-demos
glxinfo | grep OpenGL
```

You should see your GPU and driver info (e.g. "Mesa Intel(R) UHD Graphics..."). If errors appear, your drivers may be misconfigured.

---

## 📦 Step 3: Install Picom (ibhagwan fork with advanced features)

Clone and build from AUR for the latest features like blur and animations:

```bash
cd ~/Downloads
git clone https://aur.archlinux.org/picom-ibhagwan-git.git
cd picom-ibhagwan-git
makepkg -si
```

---

## ⚙️ Step 4: Picom Configuration File

Create or edit your picom config at `~/.config/picom/picom.conf`:

```ini
# Use GLX backend for blur and vsync support
backend = "glx";

# Enable vertical sync to reduce tearing (if supported by driver)
vsync = true;

# Blur settings (Gaussian blur)
blur {
  method = "gaussian";
  size = 12;
  deviation = 8.0;
};

# Rounded window corners
rounded-corners = true;
corner-radius = 12;

# Fade animations on window open/close
fading = true;
fade-in-step = 0.03;
fade-out-step = 0.03;

# Enable shadows for windows
shadow = true;
shadow-radius = 7;
shadow-opacity = 0.5;

# Exclude blur for panels/docks/desktops
blur-background-exclude = [
  "window_type = 'dock'",
  "window_type = 'desktop'"
];

# Set transparency for Alacritty terminal (90% opacity)
opacity-rule = [
  "90:class_g = 'Alacritty'"
];
```

---

## 🚀 Step 5: Start Picom Automatically with i3wm

Edit your i3 config (`~/.config/i3/config`) and add:

```bash
exec --no-startup-id picom --config ~/.config/picom/picom.conf --experimental-backends
```

---

## 🔄 Step 6: Reload i3wm

Reload i3 (`Mod+Shift+R`) or log out and back in to apply changes.

---

## ✅ Step 7: Test It Out

* Open **Alacritty** terminal — you should see transparency and blur effect.
* Move, open, and close windows — smooth fade animations and rounded corners should be visible.
* If tearing or glitches appear, try disabling `vsync` or check driver installation.

---

## 🛠️ Step 8: Troubleshooting and Debugging

Run picom manually with detailed logging:

```bash
picom --config ~/.config/picom/picom.conf --experimental-backends --vsync --log-level=trace
```

Look for errors or warnings to fix configuration or driver issues.

---

## Optional: macOS-like Animation Tweaks

If you want advanced easing and window scaling animations like macOS, picom supports **animation rules** — I can provide examples if interested.

---

# Summary

* Install GPU drivers → verify OpenGL
* Install `picom-ibhagwan-git` from AUR
* Configure `picom.conf` with blur, rounded corners, fade animations, vsync
* Start picom via i3 config with experimental backends
* Reload i3 and enjoy smooth, modern compositing effects on Arch + i3wm
