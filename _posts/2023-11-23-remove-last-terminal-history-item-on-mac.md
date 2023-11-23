---
title: How to block last history item in terminal on macOS 
layout: post
---

Sometimes I enter something in my terminal that could be painful when compromised. To fix that, GPT, the internet, and I created a function to delete the last item in my history.

Add this function to your `.zshrc` file:

```bash
function forget() {
  # Set the target line to the last command in history
  local target_line=-1

  # Prevent the last history line from being saved
  local HISTORY_IGNORE="${(b)$(fc -ln $target_line $target_line)}"

  # Write out history to file, excluding lines that match `$HISTORY_IGNORE`
  fc -W

  # Dispose of the current history and read the new history from file
  fc -p $HISTFILE $HISTSIZE $SAVEHIST

  # Confirmation message
  print -r "Deleted '$HISTORY_IGNORE' from history."
}
```

Here is how it works:

```
$ history
cd passwords
echo "password" | ssh
$ forget
Deleted 'echo "password" | ssh' from history.
$ history
cd passwords
```

It deleted the last line of your history and saved it.
