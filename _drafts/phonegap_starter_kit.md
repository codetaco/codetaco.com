# Prerequisites

- [Xcode](https://itunes.apple.com/us/app/xcode/id497799835?mt=12)
  - enable Command Line Tools from within Xcode


# Create Project

[Cordova CLI](http://docs.phonegap.com/en/4.0.0/guide_cli_index.md.html#The%20Command-Line%20Interface)

```bash
# IOS
npm install -g cordova
npm install -g ios-sim
cordova create hello com.example.hello HelloWorld
cd hello
cordova platform add ios
cordova build ios
cordova emulate ios
```
