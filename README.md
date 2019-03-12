# ERC Hardware (Trezor) Wallet Recovery via [WallΞTH](https://raw.githubusercontent.com/walleth/walleth/) [![WallΞTH](https://raw.githubusercontent.com/walleth/walleth/master/app/src/main/res/mipmap-xhdpi/ic_launcher.png)](https://walleth.org)

Forked WallΞTH from main v37 only as a clear line of deprecation for Trezor firmware 1.6.1 ETH transaction signing support. 

[TL;DR](#too-long-didnt-read) quick fix towards the bottom. 

Backstory 
=======

I recently bailed out a bunch of coin from a Trezor with the ill-fated firmware version 1.6.1. Best I can tell, Trezor forced a firmware update due to some fundamental changes in the cryptocurrency ecosystem and demands for support from their users, rendering the older firmwares incapable of properly signing Ethereum transactions. At some point between June 2018 and March 2019,

- [MyEtherWallet](https://MyEtherWallet.com)
- [Trezor Wallet](https://wallet.trezor.io)
- [MetaMask](https://metamask.io/)

and other common bridge tools were rendered useless for the purpose of extracting financial value from their trezor, had Trezor owners not performed their firmware update.

A friend of mine found themself stuck in the middle of this, a carpenter contractor type whom often found himself away from home on business. Quietly HODLing away all he could, like any of our fellows desirous of the death of the centralized banking systems he decided a Trezor store was the best place for them. He needed to extract the coins to update the firmware, but so it seemed he also needed to update the firmware to extract the coin. Trezor support and gilded redditors offered some potential solutions

- [Vintage MyEtherWallet](https://vintage.myetherwallet.com), which also now fails to properly sign ETH transactions with the old Trezor firmware 1.6.1   
- [MyEtherWallet](https://github.com/MyEtherWallet/MyEtherWallet) doing an even more vintage version local build/install, this seemed like a much more extensive project than I was willing to commit to
- [python-trezor (trezorctl)](https://github.com/trezor/python-trezor) which is CLI-only and didn't seem to properly communicate with 1.6.1, perhaps an older build would but python has always been a [Dependency-Hell for me](https://xkcd.com/1987/)![/relevantXKCD](https://imgs.xkcd.com/comics/python_environment.png "The Python environmental protection agency wants to seal it in a cement chamber, with pictorial messages to future civilizations warning them about the danger of using sudo to install random Python packages")
- [WallΞTH](https://github.com/walleth/walleth/) Fearing the worst with bugs in older versions potentially manhandling the wallets to oblivion, and also finding the latest version was no longer compatible with firmware 1.6.1, we started digging through the commit changelog hoping for clues, and fixed efforts around the release dates of the Trezor model 1 firmware 1.6.1 release. 
 

# Working Solution: [WallEth version 0.37](https://github.com/walleth/walleth/releases/tag/0.37) ❤ Trezor 1.6.1 
- Released 2018 Jun 13 (build flavor no-GEth No-Analytics -For-FDroid -OnlineRelease)

To save anyone else out there with ERC* token wallets hopelessly stuck on an old Trezor firmware, I present to you... what worked for us. We gratefully stand on the shoulders of the Trezor and reddit communities and are happy to let you know there is a (probably time-sensitive) escape from your financial woes. Again, [TL;DR](#too-long-didnt-read) pre-built WallΞTH is below. 

I urge you to build yourself to cut out me as a middleman between you and your coin. This walkthrough's target audience is intended for pure beginners. On we go:

Build Environment 
=======

- Android Studio 3.3.2 Build #AI-182.5107.16.33.5314842, built on February 15, 2019
- JRE: 1.8.0_152-release-1248-b01 amd64
- JVM: OpenJDK 64-Bit Server VM by JetBrains s.r.o
- Windows 10 10.0


### Install
- [Android Studio](https://developer.android.com/studio/)
- [Java RunTime Environment](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) (JRE, traditional end-user Java)
- [Java SE](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) (JDK)

You can automate the above installs on Windows by leveraging the lovely [Chocolatey](https://chocolatey.org/install)[![Chocolatey Logo](https://cdn.rawgit.com/chocolatey/choco/14a627932c78c8baaba6bef5f749ebfa1957d28d/docs/logo/chocolateyicon.gif "Chocolatey")](https://chocolatey.org/install) platform. They have their simple installation methods posted there. This walkthrough describes Windows building. Should work for whatever base platform, Windows/Linux/Mac, but YMMV.

Once choco is a thing on your machine, complete the above installations with a one-liner:

```choco install -y androidstudio jdk8 jre8``` 

When you're done with the project, I'd recommend you remove all this as well.

```choco uninstall androidstudio jdk8 #chocolatey #jre8``` 

- probably worth a reboot at this time just to start fresh, as environment variables get changed. 

### Prepare
- Open android studio and follow the generic instruction for setup, where it will start pulling a bunch of base frameworks for building things in the latest version of android.
- back to our task at hand - WallΞTH v0.37 is built on Oreo 8.0 (SDK 26) so we'll need to pull in that framework as well.

`File` > `Settings` > `System Settings` > `Android SDK` > ✔ `Android 8.0` and then `Apply`  

This gits the framework for building this Internet-ancient WallΞTH 37.

### Import & Configure
- Download/git/ssh the version that worked for us.  (WallΞTH 0.37 from master branch worked for Trezor 1.6.1)
[WallΞTH tag 0.37](https://github.com/walleth/walleth/releases/tag/0.37)

- if you grabbed the zip/tar file, unzip/tar the archive somewhere that can host a couple Gig's worth of disk space - the build dependencies that get auto-downloaded by Studio in during compiling are sprawling. 

- Back in android studio, 

`File` > `New` > `Import Project (gradle...)` 

Direct it towards the walleth-0.37 directory unzipped in the last step. Studio will start setting itself up to work with the codebase. 

- Android Studio will recommend upgrding the Gradle install. I didn't, and since we're compiling legacy code, it's probably a bad idea. `Don't Remind me Again`

- Wait for the initial Gradle Build to complete. 
- on the  left-hand border of Studio there's a tab 'Build Versions' that presents a dropdown menu.
- Select `NoGEth - NoFirebase - ForFDroid - Online -Release`

#### Why this build?
 - v0.37 was found to work. Tested others, but no luck for 1.6.1. Can't vouch for what changed in the WallΞTH code, or if any other version might work, but this one did for us. 
 - [Geth](https://github.com/ethereum/go-ethereum/wiki/Mining) is an extension for WallΞTH that lets us run a full ETH node - awesome, but unncessary for this purpose. 
 - [FireBase](https://developers.google.com/training/firebase/) is Google's analytics platform, again unnecessary, and possibly sketchy. 
 - `Online`  because I'm pretty sure `offline` won't connect to the ETH mainnet, where we need our blockchain ledger updates written so our real coins go to real addresses
 - `Release` : We don't need the Debug version, we're trying to run lean for this Trezor recovery purpose alone.
 - [F-Droid](https://f-droid.org) is the only static variable in all the build flavors because at this stage of WallΞTH development, it was not ready for play store, and we shouldn't/wouldn't be able to sign the code for Play Store inclusion anyways. (FOSS android app store, be sure to send [Richard Stallman](https://my.fsf.org/donate) some coin once you recover yours! litecoin:LPttYC3GoXNrBqGfLT7tTbNHm8SiUpBwYz bitcoin:1PC9aZC4hNX2rmmrt7uHTfYAS3hRbph4UN)


### Build Time!

We're about halfway out of the woods.

- `Build` > `Generate Signed Bundle APK` > 🔘`APK`   
- Now create a code signing signature for yourself. Save the keystore to a path you can find it again, and set/remember the passwords (so you can use the same code sig if you have to repeat this.)
- Select the build variant `noGethNoFirebaseForFDdroidOnlineRelease`
- Sign with the v2 method only, v1 is ancient and not required. If you're running an android version that requires this for app installs, you shouldn't be messing with valuable data like crypto on that device and the WallΞTH app probably won't work.

### It's building...
- Cross your fingers. 
- Android Studio might try to convince you to install their 'latest APK bundler' - `no`. Don't need it for this project, we're not publishing to Play Store.

If today's a good day, after a few minutes you'll get a toast message 

```
Generate Signed APK
APK(s) generated successfully:
Module 'app': locate or analyze the APK.
```

in the bottom-right that build completed. Click `LOCATE` and your File Browser should open to the walleth-0.37/app directory. There will be a directory name `noGethNoFirebaseForFDroidOnline` there matching the build flavor you'd selected to build. Inside is an APK. 

### Install
Send that APK to your Android device, preferably one running Oreo 8.0 or 8.1. Use bluetooth, Google drive, USB cable, whatever. Open your File Manager of choice and find the APK, click to install. If prompted that 'for security, this app is prevented from installing other apps' then you'll need to allow that.

If you have a WallΞTH version installed already, you're going to have to uninstall that first. If you've got a bunch of precious WallΞTH configuration data, perhaps leverage [TitaniumBackup](https://www.titaniumtrack.com/titanium-backup.html) if you're running rooted Android. Otherwise, move your coins/wallets around to get them away from your existing WallΞTH install, because the current instance must be destroyed to get this v37 version running.

If the installation fails, check
- Did you tell `Android Settings` to allow your `File Manager` app to install other apps?
- Did you back up your wallets and uninstall any current WallΞTH install?
- Did you Build a SIGNED APK? It needs to be signed. Had enough? Head to the [TL;DR](#too-long-didnt-read)

### Connect
Once you're set with v37 WallΞTH installed, connect your Trezor to your android with USB ([OTG](https://shop.trezor.io/product/android-phone-cable) cable, or leverage an [OTG adapter](https://www.monoprice.com/Product?p_id=9724). (adapter's male goes in the Android's port, the generic cable goes between the adapter and the trezor.)

WallΞTH should recognize your Trezor, allow pin unlock, view wallets, and the value stored on them. If it's not reading the known value of the wallets, you might have to try a different WallΞTH version. This walkthrough only describes how v37 worked with firmware 1.6.1 


Bail out your coins!
=====

- Send your coin to an address that's explicitly meant for that coin, and one you control! Binance, BitStamp, CoinBase are all decent intermediaries until you've got your Trezor updated to the latest firmware and ready to receive the transfers back onto it. 
- I'd recommend sending VERY SMALL amounts of coin at first as a few test cases. Wait for the nodes validation to fully complete, and move forward from there. Even if all the software you're running is perfectly tuned, after all's said and done the Internets can corrupt or drop data transfers sometimes.
- **BE SURE to uninstall version 37** or whatever other ancient alpha build you're working with once you've recovered your gear. It's strongly urged to keep up with the latest version for compatibility and security purposes. 


Too long; didn't read
=======

If you're not paranoid about where your personal banking software executables are from, or just don't want to fuss with build environments, here's the exact APK build that worked for us. It's the one we compiled straight from v37 walleth/walleth/main branch with the process described above. The code is verifiable, but painful. Only difference is the code signature certificate it's signed with, which is **not** publicly verifiable. [F-Droid](https://f-droid.org/en/packages/org.walleth/) repo doesn't go back into WallEth's alpha days, and I don't feel like bothering the WallΞTH devs with this as I'm sure they have enough on their hands. Here you go. 

[PreBuilt WallΞTH-v37 APK](https://github.com/mfsen10/walleth/raw/37-trezorbailout/assets/walleth-0.37-release.zip) (right click download, or there's a download button on the linked page.)

SHA256: abdf17f0f07f474bca1b74486ede1ee7194bc863e0be9d9c1778ca5773ad181a *WALLETH-0.37-noGeth-noFirebase-forFDroid-online-release.apk

[VirusTotal 3rd party validation - not malware](https://www.virustotal.com/#/file/abdf17f0f07f474bca1b74486ede1ee7194bc863e0be9d9c1778ca5773ad181a/detection)


Panhandling 
=======

If I saved you some hairpulling and/or time, I'd very much appreciate some coin in lieu of cash/check/money order/cigarette cartons. My Ethereum ETH-ONLY address, thanks in advance: 

**0x4Ea515dDfc03D833fDC202393621A89AB77F1D87**

![0x4Ea515dDfc03D833fDC202393621A89AB77F1D87](https://github.com/mfsen10/walleth/blob/37-trezorbailout/assets/eth-addr-qr.png)

**Be sure to tip the WallEth devs as well**, their addresses are listed within the app. 



Standard WallΞTH README follows:
=======

[![on Google Play](https://ligi.de/img/play_badge.png)](https://play.google.com/store/apps/details?id=org.walleth)
[![on FDroid](https://ligi.de/img/fdroid_badge.png)](https://f-droid.org/repository/browse/?fdid=org.walleth)
![](https://github.com/ligi/walleth/blob/master/assets/1024x500.png)

WALLΞTH
=======

Native Android Ethereum wallet.

Features
========

 - Networks main(1), ropsten(3), rinkeby(4)
 - Tokens(ERC-20)
 - ERC-67 / ERC-681 URLS
 - ERC-55 Checksums
 - Offline signing (compatible to Parity signing flow)
 - JSON UTC key import/export
 - display value in fiat like EUR, NZD, USD, .. or MakerDAO DAI 
 - display function 
 - [TREZOR](https://trezor.io/?a=walleth.org) Support via USB-OTG - model 1 and model T are supported
 - watch only accounts 
 - one flavor contains go-ethereum light client

find more information on https://walleth.org

References
==========

* [ERC67](https://github.com/ethereum/EIPs/issues/67)
* [ERC681](https://eips.ethereum.org/EIPS/eip-681)
* [ERC20](https://eips.ethereum.org/EIPS/eip-20)
* [Import Geth - a Devcon2 talk](https://ethereum.karalabe.com/talks/2016-devcon.html#1)
* [go Mobile:-Account-management](https://github.com/ethereum/go-ethereum/wiki/Mobile:-Account-management)
* [Ethereum visual reference](https://www.ethereum.org/images/logos/Ethereum_Visual_Identity_1.0.0.pdf)

License
=======

GPL
