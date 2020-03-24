# FortUpdater
This class will help you to find the new offsets, on every update.
It can be implemented in cheats to make them auto-updating.
The only thing you must do is add Pattern Scan for the only 4 needed offsets.
ALL PATTERNS ARE IN THE EXAMPLE ON *HOW TO USE*

Security info
---------------
In this release i don't use Return Address's spoof.
You must implement a spoofer to "GetObjectName" and "GetNameByIndex" funcs to be UNDETECTED for a long time.

HOW TO USE
---------------
Just initialize the class and then search for your needed offset!
```
uintptr_t UObjectArray = (base_address + 0x7EC0F00);
uintptr_t GetObjectName = (base_address + 0x4114470);
uintptr_t GetNameByIndex = (base_address + 0x2B713B0);
uintptr_t FnFree = (base_address + 0x2A6E690);

FortUpdater* Updater = new FortUpdater();
if (Updater->Init(UObjectArray, GetObjectName, GetNameByIndex, FnFree))
{
  Offsets.Levels = Updater->FindOffset("World", "Levels");
  Offsets.OwningGameInstance = Updater->FindOffset("World", "OwningGameInstance");
  Offsets.PlayerCameraManager = Updater->FindOffset("PlayerController", "PlayerCameraManager");
  Offsets.AcknowledgedPawn = Updater->FindOffset("PlayerController", "AcknowledgedPawn");
}
```

These are the current patterns for the 4 needed offsets.
```
UObjectArray: 49 63 C8 48 8D 14 40 48 8B 05 ? ? ? ? 48 8B 0C C8 48 8D 04 D1 (+10)
GetObjectName: 40 53 48 83 EC 20 48 8B D9 48 85 D2 75 45 33 C0 48 89 01 48 89 41 08 8D 50 05 E8 ? ? ? ? 8B 53 08 8D 42 05 89 43 08 3B 43 0C 7E 08 48 8B CB E8 ? ? ? ? 48 8B 0B 48 8D 15 ? ? ? ? 41 B8 ? ? ? ? E8 ? ? ? ? 48 8B C3 48 83 C4 20 5B C3 48 8B 42 18
GetNameByIndex: located at the 4th call inside GetObjectName, and at the moment it is at "GetObjectName + 0x64"
FnFree: 48 85 C9 74 2E 53
```

INFO
---------------
If someone is asking how much time it require for each offset:
Every offset search, for me took about 250µs (microsecond) (0,250ms)
