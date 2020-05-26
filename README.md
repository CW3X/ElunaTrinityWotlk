This branch contains the changes needed to send patches through the server to the WoW Client  
```diff
! NOT RECOMMENDED FOR PRODUCTION
```

Requirements:
- 3.3.5 Client  
- Patched Wow.exe to allow loading of unsigned/badly signed MPQ archives  
- Wow.exe with changed build number and (optionally) version numbers  
- MPQ with Prepatch.lst and related files  
- New entry in auth.build_info table

References:
- [Patch 1](https://pastebin.com/Vdp9wpBT)
- [Patch 2](https://pastebin.com/ESi3em3T)


I would like to thank **schlumpf** and **stoneharry** for their [guide](https://www.dropbox.com/s/fovh9mtrj9tgqd4/Implementing%20in-client%20patching%20for%20World%20of%20Warcraft.pdf?dl=0)


Diff patch: https://pastebin.com/dhjujLhb
Eluna fork with patch applied: https://github.com/luth31/ElunaTrini...e-clientupdate
Warning:
I strongly recommend NOT to use this in a production server without editing it a bit. Right now the patcher doesn't run in another thread or async so it blocks main thread. Also XferResume isn't implemented so you would have to delete the partial patch before updating (that is if you have one).
How to use :
Apply patch to your source, add PatchDir to your authserver.conf, create an MPQ and name it the client build target of your patch (example: 12340.mpq would be a patch for users that use Wow.exe with a build number of 12340)
Remove the entry 12340 from auth.build_info in your DB.
Add your desired build number to build_info with a version of 3 3 5 a (if you don't know how to do it just look at the other entries in the table)
Patch your Wow.exe to allow badly signed MPQs (or try to crack Blizzard's private key, have fun)
Edit the build number of your update Wow.exe using any hex editor (hint: it's at address 0x004C99E0; thanks Eluo@Modcraft)
Add your Wow.exe to your MPQ and your prepatch.lst (check stoneharry's thread to learn how to make one)
Put your MPQ in your PatchDir location (Warning: make sure that the PatchDir directory exists!, otherwise patching will be disabled)

Useful resources:
http://www.ac-web.org/forums/showthr...oW-Application
http://www.modcraft.io/index.php?topic=1829

Tweaks:
You can change the size of the chunk sent every tick and the wait time between ticks in Patcher.cpp
CHUNK_SIZE is the number of bytes sent at a time
Sleep(100) (line 37) controls the wait time between ticks; change to lower to send faster (recommended: run the patcher on another thread)

Credits:
Special thanks to stoneharry and schlumpf
