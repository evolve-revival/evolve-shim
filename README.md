# evolve-shim

A Windows DLL (`evolve_shim.dll`) that intercepts three Steam API calls Evolve makes at startup to verify ownership, and forwards everything else to the real Steam API.

## Intercepted functions

| Export | Normal behavior | Shim behavior |
|--------|----------------|---------------|
| `SteamAPI_ISteamApps_BIsDlcInstalled` | Queries Steam for DLC ownership | Always returns `true` |
| `SteamAPI_ISteamApps_BIsSubscribedApp` | Queries Steam for app ownership | Always returns `true` |
| `SteamAPI_ISteamUtils_GetAppID` | Returns current Steam app ID | Always returns `273350` (Evolve) |

All other exports are forwarded to `steam_api64_real.dll`, which must be present alongside the shim in `bin64_SteamRetail/`.

## Build

Requires MinGW-w64 cross-compiler.

Using CMake:

```bash
cmake -B build -DCMAKE_TOOLCHAIN_FILE=mingw-w64-x86_64.cmake
cmake --build build
```

Or directly:

```bash
x86_64-w64-mingw32-g++ -O2 -Wall -shared -o evolve_shim.dll shim.cpp shim.def
```

Place the output `evolve_shim.dll` in `bin64_SteamRetail/`.
