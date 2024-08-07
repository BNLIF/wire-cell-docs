[TOC]

# Introduction to Bee

Displaying 3D reconstruction results is difficult.
Often it requires installation of some platform-dependent programs and downloading some pre-formated data.
For most physicists, it's too much effort ("Just show me the results!")

[Bee](https://www.phy.bnl.gov/twister/bee/) is a web-based application intended to solve this problem, and brings the interactive 3D world to the physicists. Some of the features include:

- **No installation** : Just go to [the Bee homepage](https://www.phy.bnl.gov/twister/bee/).
- **Cross-platform** : Runs everywhere with [a modern web browser that supports WebGL](http://caniuse.com/#feat=webgl), including mobile devices.
- **Easy share**: Each event has its own url that can be shared with anyone in the world.
- **Interactive** : Unleashes the power of WebGL with mouse and keyboard.
- **Cross experiments**: Supports most LArTPC detector geometries.
- **Ready for analysis** : User uploads, multiple algorithms, MC truth, customizable overlay, etc.


# Requirements

The only requirements to run Bee are:

- A modern web browser that supports WebGL. ([Will my browser work?](http://caniuse.com/#feat=webgl))
  We recommend [Google Chrome](http://www.google.com/chrome/) for the best experience.
- An internet connection.
- A relatively good graphic card. A discrete GPU greatly enhances the performance.

# Basic Interactions

The following table summarizes the basic interactions with Bee on different devices:

| Action | Mouse | Keyboard | Touch |
| ------ | ----- | -------- | ----- |
| Rotate | Left button drag | | One-finger touch |
| Pan    | Right button drag | Left / Right / Up / Down | Three-finger swipe |
| Zoom in / Zoom out | Mouse wheel | Shift+Up / Shift+Down | Pinch & Zoom |

# Advanced Interactions

## Show and Overlay results from different algorithms

Bee supports any algorithms that produce a set of 3D space points (with or without charge). In the right control panel, the "Recon" folder shows the list of algorithms that Bee can load. By default, only the first one is loaded and its space points are displayed on screen. To show others:

- Click on the name of the algorithm. If it is not loaded yet, Bee will download its space points from the server. 
- Notice that the left panel now shows the name, size, opacity and color of the current actively selected algorithm.
- Increase the opacity until it is displayed on screen.
- You can now switch to other algorithms to adjust their opacity so that they can be overlayed.
- You may need to uncheck the "Show Charge" checkbox (on the right panel) so that the algorithm can be represented by a single color.

# Hotkeys

Users may find it convenient that most actions in BEE can be achieved using keyboard shortcuts.
The full list of hot-keys can be found through "Menu -> View -> Hotkey List", and is also summarized below:

<table class='table'>
    <thead>
      <tr>
        <th>Command</th><th>Hotkey</th>
        <th>Command</th><th>Hotkey</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Toggle MC</td><td>m</td>
        <td>Toggle Charge</td><td>q</td>
      </tr>
      <tr>
        <td>Next Event</td><td>Shift+n</td>
        <td>Prev Event</td><td>Shift+p</td>
      </tr>
      <tr>
        <td>Next Recon</td><td>]</td>
        <td>Prev Recon</td><td>[</td>
      </tr>
      <tr>
        <td>Next Slice</td><td>k</td>
        <td>Prev Slice</td><td>j</td>
      </tr>
      <tr>
        <td>Next Flash</td><td>></td>
        <td>Prev Flash</td><td><</td>
      </tr>
      <tr>
        <td>Center to Event</td><td>c</td>
        <td>Reset Camera</td><td>r</td>
      </tr>
      <tr>
        <td>X-U view</td><td>u</td>
        <td>X-V view</td><td>v</td>
      </tr>
      <tr>
        <td>X-Z view</td><td>z</td>
        <td>X-Y view</td><td>x</td>
      </tr>
      <tr>
        <td>Zoom in</td><td>Shift+Up</td>
        <td>Zoom out</td><td>Shift+Down</td>
      </tr>
      <tr>
        <td>Select Recon</td><td>1 - 9</td>
        <td>Unselect All</td><td>Esc</td>
      </tr>
      <tr>
        <td>Increase Opacity</td><td>=</td>
        <td>Decrease Opacity</td><td>-</td>
      </tr>
      <tr>
        <td>Increase Point Size</td><td>+</td>
        <td>Decrease Point Size</td><td>_</td>
      </tr>
      <tr>
        <td>Redraw All Points</td><td>o</td>
        <td>Show FPS</td><td>s</td>
      </tr>
    </tbody>
</table>
