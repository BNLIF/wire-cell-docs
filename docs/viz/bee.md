[TOC]

# Introduction to BEE

Displaying 3D reconstrution results is difficult.
Often it requires installation of some platform-dependent programs and dowloading some pre-formated data.
For most physcists, it's too much effort ("Just show me the results!")

[BEE](http://www.phy.bnl.gov/wire-cell/bee/) is a web-based application intended to solve this problem, and bring the interactive 3D world to the physicists.
In particular, one can enjoy the following features:

- *No setup* : Just go to [the BEE homepage](http://www.phy.bnl.gov/wire-cell/bee/).
- *Cross-platform* : Runs everywhere with [a modern web browser that supports WebGL](http://caniuse.com/#feat=webgl), including mobile devices.
- *Interactive* : Unleashes the power of WebGL with mouse, keyboard and touch devices.
- *Optimized for LArTPC* : Tomographic mode, projections, multiple geometries, ...
- *Optimized for developers* : User uploads, multiple algorithms, MC truth, customizable overlay, ...
- *Optimized for public* : Explore the large preset samples of different neutrino interactions in LArTPC!

# Requirements

The only requirements to run BEE are:

- A modern web browser that supports WebGL. ([Will my browser work?](http://caniuse.com/#feat=webgl))
  We recommend [Google Chrome](http://www.google.com/chrome/) for the best experience.
- An internet connection.
- A relatively good graphic card. A discrete GPU greatly enhances the performance.

The following table summarizes the basic interactions with BEE on different devices:

| Action | Mouse | Keyboard | Touch |
| ------ | ----- | -------- | ----- |
| Rotate | Left button drag | | One-finger touch |
| Pan    | Right button drag | Left / Right / Up / Down | Three-finger swipe |
| Zoom in / Zoom out | Mouse wheel | Shift+Up / Shift+Down | Pinch & Zoom |

# Hotkeys

Users may find it conveninet that most actions in BEE can be achieved using keyboard shortcuts.
The full list of hotkeys can be found through "Menu -> View -> Hotkey List", and is also summarized below:

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
        <td>Center to Event</td><td>c</td>
        <td>Reset Camera</td><td>r</td>
      </tr>
      <tr>
        <td>X-U view</td><td>u</td>
        <td>X-V view</td><td>v</td>
      </tr>
      <tr>
        <td>X-Z view</td><td>z</td>
        <td>Y-Z view</td><td>c</td>
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
    </tbody>
</table>
