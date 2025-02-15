---
title: Paste keystrokes automatically with Apple Automator
layout: post
---

Want to paste a text into some place?

<img width="442" alt="image" src="https://user-images.githubusercontent.com/1079135/146840229-9cac9b82-6c59-4b9c-bcb1-69f638936bfb.png">

Create an AppleScript with this code:

```applescript
on run {input, parameters}
  delay 5

  tell application "System Events"
    repeat with charOfInput in (characters of (input as text))
      set asciiValue to ASCII number of charOfInput
      -- Check if ASCII value is in the range of uppercase letters (65-90)
      if asciiValue ≥ 65 and asciiValue ≤ 90 then
        key down shift
        keystroke charOfInput
        key up shift
      else
        keystroke charOfInput
      end if
      delay 0.1
    end repeat
  end tell

  return input
end run
```

I created an app called Paste Keystrokes, the final result can look like this:

<img width="616" alt="image" src="https://user-images.githubusercontent.com/1079135/146840130-4d4a9874-88e7-4f7e-afd5-07def3edc15d.png">

## App not allowed to send keystrokes

If you get issues like thiis:

The action “Run AppleScript” encountered an error: “System Events got an error: Paste Keystrokes is not allowed to send keystrokes.

1. Go to System Preferences > Privacy & Security > Accessibility
2. Remove your app if listed there
3. Re-add your app to the list

![2024-02-21-11h39-screenshot@2x](https://github.com/adriaandotcom/blog.adriaan.io/assets/1079135/81a7303c-23ad-49e5-98eb-b38272cade8d)
