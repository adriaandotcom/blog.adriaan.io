---
title: Turn off Wi-Fi when macOS goes to sleep
layout: post
---

When I am on a network that's paid by the hour you want to disable your Wi-Fi when you are not using your mac. To do this I listen for when the mac goes to sleep and disable the Wi-Fi. 

1. Install [sleepwatcher](https://www.bernhard-baehr.de/) with homebrew: `brew install sleepwatcher`
2. I have my scripts living in `~/Developer/scripts` (create that folder with `mkdir -p ~/Developer/scripts`)
3. Create a script at `~/Developer/scripts/sleepscript.sh`:

   ```sh
   networksetup -setairportpower Wi-Fi off
   ```

4. Make your script excutable in your Terminal with `chmod +x ~/Developer/scripts/sleepscript.sh`
5. Test your script with `/usr/local/sbin/sleepwatcher --verbose --sleep ~/Developer/scripts/sleepscript.sh --wakeup /path/to/your/wakeupscript`
6. Create a Launch Agent at `~/Library/LaunchAgents/de.bernhard-baehr.sleepwatcher.plist`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
   <plist version="1.0">
   <dict>
     <key>Label</key>
     <string>de.bernhard-baehr.sleepwatcher</string>
     <key>ProgramArguments</key>
     <array>
       <string>/usr/local/sbin/sleepwatcher</string>
       <string>-V</string>
       <string>-s ~/Developer/scripts/sleepscript.sh</string>
     </array>
     <key>RunAtLoad</key>
     <true/>
     <key>KeepAlive</key>
     <true/>
     <key>StandardOutPath</key>
     <string>~/Developer/scripts/sleepscript-output.log</string>
     <key>StandardErrorPath</key>
     <string>~/Developer/scripts/sleepscript-errors.log</string>
   </dict>
   </plist>
   ```
    
   Just save it to `~/Library/LaunchAgents/de.bernhard-baehr.sleepwatcher.plist`
    
   > Don't change `-s` to `--sleep` because somehow that didn't work.

1. Enable and start the Launch Agent `launchctl load -w ~/Library/LaunchAgents/de.bernhard-baehr.sleepwatcher.plist`
2. Test is while putting your mac to sleep. No luck? Check your logs in `~/Developer/scripts/`
