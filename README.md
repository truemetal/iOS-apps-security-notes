## iOS apps security notes

The purpose of this repo is to serve as dump of notable and useful things about iOS apps security. 
Originally borned from a question "how safe it is to store some password / api key as text in app's source codes". 
**Pull requests are welcome.**

### App package

Apps are stored as `.ipa` file. When you export app archive to submit to Apple's iTunesConnect it's being exported as .ipa file. When you backup your device, all apps are being saved as `ipa`s in `Mobile Applications` folder on your drive. 

This file is basically a `zip` archive. After you rename `.ipa` to `.zip` and unpack you can see it's structure. Example: 

![screen shot 2016-04-12 at 22 08 28](https://cloud.githubusercontent.com/assets/2472186/14472289/27a2edaa-00fb-11e6-86b5-b7504fb98128.png)

In `Payload` folder is most interesting part - app package; right-click and `show package contents` would reveal this:

![screen shot 2016-04-12 at 22 10 46](https://cloud.githubusercontent.com/assets/2472186/14472344/702a8876-00fb-11e6-9d45-8dd116c8c78e.png)

In this case the app's binary file is `ObfuscationTestApp`. This folder would also contain images, fonts, other files included. 

### Peeking into app binary

There are several tools I know you can use to peek into app binary: `strings`, `otool -ov`, [class-dump](http://stevenygard.com/projects/class-dump/) and disassemblers like `hopper` 

Here's an example of `strings`, `otool -ov` and `class-dump` outputs respectively for one of my apps I've downloaded from app store, and backed up via itunes: 

![screen shot 2016-04-12 at 22 20 10](https://cloud.githubusercontent.com/assets/2472186/14472646/c3c57f44-00fc-11e6-89da-eb7855962d79.png)

![screen shot 2016-04-12 at 14 27 12](https://cloud.githubusercontent.com/assets/2472186/14458931/9941ec4c-00bd-11e6-9ffd-448f6678dd39.png)

![caterhamclinic_ _-bash_ _202x60_and_screenshots_and_applications](https://cloud.githubusercontent.com/assets/2472186/14458926/8d27d9da-00bd-11e6-9ebb-546a493ac58b.png)

The apps users get from app store looks encrypted. And here comes an interesting point: **if the plaintext password is stored in source code it is protected by the fact that app binary is encrypted**. `Info.plist` is not encrypted, so that's a big no-no for a place to store passwords. 

### Peeking into unencrypted app binary

If you build & archive app, then export it for app store deployment it would not be encrypted. And here're the outputs from `strings`, `otool -ov` and `class-dump` respectively: 

![screen shot 2016-04-12 at 22 29 40](https://cloud.githubusercontent.com/assets/2472186/14472900/160bd5a4-00fe-11e6-8360-3117af921022.png)

![screen shot 2016-04-12 at 22 30 46](https://cloud.githubusercontent.com/assets/2472186/14472917/3b7f8f7e-00fe-11e6-82e9-dd8f8d8cc8b6.png)

![screen shot 2016-04-12 at 15 11 36](https://cloud.githubusercontent.com/assets/2472186/14459500/eea8c388-00c0-11e6-8607-b41b6e5a0c58.png)

And you can get even more interesting things with `hopper`. Here's for example contents of `-[AppDelegate application:didFinishLaunchingWithOptions:]`. With this tool you can pretty much see the func calls and logic that's going on in the function. 

![screen shot 2016-04-12 at 22 33 07](https://cloud.githubusercontent.com/assets/2472186/14473012/b0e47bb2-00fe-11e6-8a92-876b0a9f1189.png)

## TODO: 

- Find a way to decrypt an app you have downloaded on your iphone from app store. What a hacker needs to do that? 
- For investigation purposes find out a way to decrypt your own app for which you have provisioning profiles and that is submitted with your apple developer's account  

**Pull requests are appreciated if you can share anything on the matter**
