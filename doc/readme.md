## Original intention
It used to be more convenient to download Steam resources, but after Steam changed the list mechanism, you must own the game to get the Steam list.
Then a big guy wxy1343 collected some shared offline numbers to extract the collection list.
Through these lists, you can directly download the genuine diversion from Steam's CDN, which is at least faster than hanging seeds and network disks (should...)
But what if you can't play the genuine diversion? It's up to the members to figure it out~
For example, tools, little green men, simulators, less, etc...

## ManifestDownload 【Steam Manifest Downloader】
### Tool operation diagram  
![](m1.png)
![](m2.png)

### Tool Description  
List data source：**[ManifestAutoUpdate](https://github.com/wxy1343/ManifestAutoUpdate)**  
Get the list of apps corresponding to Steam's appid (latest, if it is included in the warehouse normally)
Current collection [Online Documentation](https://docs.google.com/spreadsheets/d/1tS-Tar11TAqnlaeh4c7kHJq-vHF8QiQ-EtcEy5NO8a8)  
How to check Steam's appid?
For example, Resident Evil Village, you can see the link through the Steam store or Steamdb
```
https://store.steampowered.com/app/1196590/Resident_Evil_Village/  
https://steamdb.info/app/1196590/  
```
The 1196590 is the appid

More details can be studied on your own
Because it accesses GitHub's API to obtain data, but GitHub has a frequency limit on API requests.
No token: 60 times/hour
With token: 5000 times/hour
Therefore, it is necessary to generate a Person Token with empty permissions on GitHub and put it in the configuration file.

In addition, although we have not been blasted during use, we cannot rule out the possibility that...
If necessary, please study and use magic on your own...

**Ps. The software itself does not have much technical content, but in order to avoid it becoming a profit-making tool for Xbao Xyu after the copywriting is modified, a simple shell will be added. **

### Tool Parameters
```dos
::Download resource list according to appid
ManifestDownload.exe app [appid1 appid2]
ManifestDownload.exe app 1000
ManifestDownload.exe app 1000 2000

:: Download the specified list according to the manifest (not used by ordinary people)
ManifestDownload.exe manifest [manifestId1 manifestId2]
ManifestDownload.exe manifest 4004_2300048559188578691

:: Extract all decryption keys in the downloaded manifest (automatically extracted when the app is downloaded, DepotDownloader may be useful
ManifestDownload.exe dumpkey
```

## DepotDownloader (magic modification) Steam resource downloader
### Tool operation diagram
![](d1.png)
![](d2.png)
![](d3.png)
![](d4.png)
![](d5.png)
### Tool Description
This tool is modified from **[SteamRE/DepotDownloader](https://github.com/SteamRE/DepotDownloader)**
Added some convenient features, such as
* Support for downloading without login
* Cache Steam original list
* Cache decrypted list
* Automatically identify local existing lists
* Support for specifying ssfn
* Add additional cdn servers
* Fix the problem of domestic cdn request restrictions, etc.

For detailed parameters and usage, please refer to the original project ReadMe.
For security reasons, although this project supports obtaining lists and downloading resources through account passwords, it is **not recommended to use your own account**
If it is necessary to use the account mode, it is recommended to use **offline account** or **share account** to download.

**Ps. Although this software is a magic modification and the upstream open source agreement is GPL2, in order to avoid being modified into a profit-making tool for Xbao Xyu, a simple shell will be added. **

### Tool parameters (including new features after magic modification)
```cmd
:: Login-free mode
:: Download all game content to the default directory (by default, directories will be built and stored according to depot)
DepotDownloader.exe -app 904320
:: Download the game's specified depot content to the default directory (by default, directories will be built and stored according to the depot)
DepotDownloader.exe -app 904320 -depot 994710 904321

:: [New feature] Automatically store resources according to appid, and all resources will be placed in the apps/appid directory
DepotDownloader.exe -useAppDir -app 338930

::Download all the game content to a directory
DepotDownloader.exe -app 904320 -dir E:/GameName
:: Download the game's specified depot content to a certain directory
DepotDownloader.exe -app 904320 -depot 994710 904321 -dir E:/GameName

:: Download the game by specifying the manifest (the manifest file should be placed in the depotcache of the current program) depotId and manifestId should correspond one to one
DepotDownloader.exe -app 534380 -depot 534381 -manifest 3462017367423786531 -useAppDir
DepotDownloader.exe -app 338930 -depot 338931 338932 347970 359000 359001 -manifest 1547401116504413409 3145636846994837120 7699221664155927697 2868566071607167109 761387674804236811 -useAppDir

::Login mode
::[New feature] Log in and download games using account and password and ssfn (ssfn files should be placed in the current program directory)
DepotDownloader.exe -user {account} -pass {password} -ssfn ssfnxxxxxx -app 904320 -depot 994710 904321 -useAppDir

:: Login but only get the download key and manifest
DepotDownloader.exe -user {account} -pass {password} -ssfn ssfnxxxxx -app 904320 -manifest-only

::[New feature] Get only game manifest information and try to generate a decrypted manifest (located in the depotcache_decrypt directory) (the decrypted manifest should be able to support keyless downloading of games)
DepotDownloader.exe -user {account} -pass {password} -ssfn ssfnxxxxxxxxxxxxxx -app 904320 -manifest-only -dm

::[New feature] Additional specified CDN
DepotDownloader.exe -app 904320 -cdn steampipe.akamaized.net:80,trts.baishancdnx.cn:80

::[New feature] Force download via depotid (some game apps have request restrictions that make it impossible to automatically obtain the depot list)
DepotDownloader.exe -app 338930 -depot 338931 338932 347970 359000 359001 -forceDepot
```
