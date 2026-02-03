# Patch Notes: Fix crash during early game initialization

## Symptoms

On game startup, the following error appears and the game may crash or fail to continue loading:

```txt
[Error  : Unity Log] NullReferenceException: Object reference not set to an instance of an object
Stack trace:
PlatformPrefs.TryGetPreferencesProvider (Splatform.IPreferencesProvider& preferencesProvider) (at <4cba033070444d388264593479692798>:0)
PlatformPrefs.GetString (System.String name, System.String defaultValue) (at <4cba033070444d388264593479692798>:0)
Localization.SetStartupLanguage () (at <f9c1e10e233e4d07bcb7e2919c8ed6a6>:0)
Localization..ctor () (at <f9c1e10e233e4d07bcb7e2919c8ed6a6>:0)
Localization.Initialize () (at <f9c1e10e233e4d07bcb7e2919c8ed6a6>:0)
Localization.get_instance () (at <f9c1e10e233e4d07bcb7e2919c8ed6a6>:0)
LocalizationManager.Localizer.Load () (at <36008b48d38d4b719fc81601d49ca8ad>:0)
Resurrection.Resurrection.Awake () (at <36008b48d38d4b719fc81601d49ca8ad>:0)
UnityEngine.GameObject:AddComponent(Type)
BepInEx.Bootstrap.Chainloader:Start() (at C:/Users/crypt/RiderProjects/BepInEx/BepInEx/Bootstrap/Chainloader.cs:434)
UnityEngine.GameObject:.cctor()
PlatformInitializer:EarlyInitialize()
```

## Cause

Localization was initialized before the platform preferences provider was ready.

## Fix

Remove the `Localizer.Load()` call from `Awake()` and invoke it in `Start()` instead.

## Changelog

- **Changed**: Move `LocalizationManager.Localizer.Load()` from `Awake()` to `Start()`
- **Fixed**: Crash caused by initializing `Localization.instance` before the platform preferences provider is ready
