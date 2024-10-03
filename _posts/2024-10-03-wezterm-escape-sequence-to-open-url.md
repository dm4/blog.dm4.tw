---
layout: post
title: "用 WezTerm 的 Escape Sequence 來開啟網頁"
---

之前在 [用 iTerm2 的 Escape Sequence 來開啟網頁](/iterm2-escape-sequence-to-open-url) 裡要解決的問題，在換到 [WezTerm](https://wezfurlong.org/wezterm/) 之後一樣要再處理一次， WezTerm 雖然不像 iTerm 可以用 OSC 1337 直接 OpenURL ，但可以用 OSC 1337 來 `SetUserVar` ，並且會觸發 `user-var-changed` [^1] 事件。

所以只要接下來只要自己處理好打開連結的部份就好了：

```lua
wezterm.on(
  'user-var-changed',
  function(window, pane, name, value)
    if name == 'open-url' then
      wezterm.run_child_process { 'open', value }
    end
  end
)
```

相對應的 `wezterm-open-url.sh` （可以參考 [get_user_vars](https://wezfurlong.org/wezterm/config/lua/pane/get_user_vars.html) ）：

```bash
#!/bin/bash

set -e
set -u

[ -z "${1-}" ] && echo "Usage: $0 <URL>" && exit 1

url="$1"
[[ $url != http* ]] && url="https://$url"
base64_url=$(echo -n "$url" | base64)

if [[ -z "${TMUX-}" ]] ; then
    printf "\e]1337;SetUserVar=open-url=%s\x07" "$base64_url"
else
    # <https://github.com/tmux/tmux/wiki/FAQ#what-is-the-passthrough-escape-sequence-and-how-do-i-use-it>
    # Note that you ALSO need to add "set -g allow-passthrough on" to your tmux.conf
    printf "\ePtmux;\e\e]1337;SetUserVar=open-url=%s\x07\e\\" "$base64_url"
fi
```

[^1]: <https://wezfurlong.org/wezterm/config/lua/window-events/user-var-changed.html>
