---
layout: post
title: "WezTerm Configuration: Making Tab Width Fill the Tab Bar"
lang: en
locale: en
---

I've finally taken the time to switch my terminal to [WezTerm](https://wezfurlong.org/wezterm/) these past few days. However, I couldn't find an option to make the tab width fill the entire window width. The closest I found was an issue: [Add "stretch tabs to fill tab bar" option](https://github.com/wez/wezterm/issues/1914), but it seems to be discussing a fancy style tab bar?

I decided to handle it myself. The method is roughly:

- Store the tab columns into `wezterm.GLOBAL`[^1] when launching or resizing
- Calculate the required padding on both sides in `format-tab-title`[^2]

Here's a summary of the configuration:

{% gist ecdc7edefa0746488292f1b6e6378714 %}

The final result looks something like this:

{% include image.md name="result.gif" %}

[^1]: <https://wezfurlong.org/wezterm/config/lua/wezterm/GLOBAL.html>
[^2]: <https://wezfurlong.org/wezterm/config/lua/window-events/format-tab-title.html>
