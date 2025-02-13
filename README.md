# dwl-patches
Patches that I've made for dwl
 - [btrtile](https://codeberg.org/dwl/dwl-patches/src/branch/main/patches/btrtile)
 - [simple_scratchpad](https://codeberg.org/dwl/dwl-patches/src/branch/main/patches/simple_scratchpad)
 - [fullscreenadaptivesync](https://codeberg.org/dwl/dwl-patches/src/branch/main/patches/fullscreenadaptivesync)


# btrtile — A Focus-Driven Tiling Layout for dwl

btrtile is a dynamic tiling layout patch designed for the [dwl](https://codeberg.org/dwl/dwl) window manager. It provides a focus-driven, mouse- and keyboard-friendly tiling layout that grants you granular control over how clients are placed and resized.

![btrtile in action](./demos/btrtiledemo.gif)

---

### Why btrtile

While dwl’s patches folder is full of different layouts, I couldn't find suitable layout that would work well with my workflow and single ultrawide monitor setup. Btrtile aims to solve that by introducing a layout strategy that splits clients according to user focus and pointer position.

---

### Features

- **Combined Tiling and Management**  
  Combines tiling layout and management of clients under one patchset.

- **Focus-Driven Splits**  
  When you add a new client, btrtile checks where your pointer is relative to the focused client’s geometry.  
  - If the pointer is on the left half (for a horizontally large client), the new client spawns on the left side, and vice versa.  
  - By default, new splits are 50/50.

- **Adaptive Splitting**  
  - If the area to be split is wider than its height, btrtile does a vertical split.  
  - Otherwise, it does a horizontal split.

- **Keyboard and Mouse Driven**  
  - Supports keyboard-based commands for quick ratio adjustments and client swapping.  
  - Mouse-based resizing and moving are integrated for more intuitive manipulation.

---

### How It Works

btrtile organizes clients using a binary tree data structure that represents splits either vertically or horizontally.

When a new client appears:
1. **Focused Client Detection**  
   btrtile checks your pointer location to find which client (if any) you’re interacting with.
2. **Split Creation**  
   - If there’s a focused client, btrtile creates a split node around it, placing the new client on the side where your pointer is.  

3. **Ratio Management**  
   Each split node has a `split_ratio` (defaulting to 0.5). This ratio defines how much space is allocated to each child node. You can adjust this ratio using keyboard or mouse actions.

---

### What It Doesn’t Handle

- **Suckless philosophy**
  - Yea, it's a bloat. I tried to hide the suck inside a single file as much I could. While this approach is not ideal, it's how it's at least for now.

---

### Configuring btrtile

btrtile adds couple variables to config.h to fine tune the mouse resizing of tiled clients.

1. **resize_factor**
	- A multiplier to transfer pointer movement to client weight ratio. Depends heavily on mouse sensivity.  
      Defaults to 0.0002f.

2. **resize_interval_ms**
	- A time based resize call limiter. Depends on framerate and screen refresh rate.   
      Defaults to 16ms. (~60 resize updates per second)

Fine tune these values to find the best values for your setup, smoother resizing can significally increase cpu overhead.  
If mouse resizing feels sluggish, you can try compiling dwl with more aggressive optimization flags like -O2/-O3. 

---

### Patch recommendations

1. **Patches that I use with my btrtile**
     
   - [focusdir](https://codeberg.org/dwl/dwl-patches/src/branch/main/patches/focusdir)  
     Great patch to move focus between clients.

   - [rotatetags](https://codeberg.org/dwl/dwl-patches/src/branch/main/patches/rotatetags)  
     Good patch to rotate the view or shift clients between tags.

   - [warpcursor](https://codeberg.org/dwl/dwl-patches/src/branch/main/patches/warpcursor)  
     Moves cursor location to focused client.

   - [pertag](https://codeberg.org/dwl/dwl-patches/src/branch/main/patches/pertag)  
     Allows each tag to have individual layout setups.

   - [gaps](https://codeberg.org/dwl/dwl-patches/src/branch/main/patches/gaps)  
     Add gaps between clients.

---


# simple_scratchpad — A very simple scratchpad utility.

   Adds functions to add, toggle and remove clients to/from scratchpad client list.
![scratchpad in action](./demos/simple_scratchpad_demo.gif)
  
---

1. **addscratchpad**
  - Floats client and adds it to scratchpad clients list.

2. **togglescratchpad**
  - Shows or hides the clients in scratchpad client list.

3. **removescratchpad**
  - Removes client from scratchpad client list.
  
---

# fullscreenadaptivesync — Enables adaptive sync/VRR when a client is fullscreen.

Adds a function that automatically enables adaptive sync/VRR when a fullscreen client is detected, and disables it once the client exits fullscreen.
  
---

1. **Why**
  - Some VRR implementations can introduce distracting flickering when the display’s refresh rate is synced with the application’s framerate. While VRR is useful for some applications (especially fullscreen games), this patch automates the toggling of VRR whenever a client enters or exits fullscreen.

2. **Requirements**
  - A FreeSync/G-Sync capable monitor
  - GPU/driver support for adaptive sync

3. **How it works**
  - When a client enters fullscreen, adaptive sync is automatically enabled.
  - When the client exits fullscreen, adaptive sync is disabled again.

4. **togglefullscreenadaptivesync**
  - Adds a switch to enable or disable the fullscreenadaptivesync behavior.  
  - Enabled by default.
  
---

# Applying the patches

   - Patches are built agains release version 0.7. Tho they should work agains the developement branch with non or minor modifications.
   - git am -3 patchname.patch
   or
   - patch -p1 < patchname.patch inside your local dwl directory.




