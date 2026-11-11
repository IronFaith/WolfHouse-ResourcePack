# WolfHouse Resource Pack Builder

This public repository combines Java resource files from BuddyPack, WolfTeams, WolfSabers, WolfPhone, WolfCraft branding, and WolfCars into one server-ready WolfHouse ZIP. Plugin code and asset sources live in the private `WolfHouse-Minecraft-Code` repository. This repository retains the existing pack release history and player downloads.

Keep the two checkouts beside each other: `WolfHouse-Minecraft-Code/` and `WolfHouse-ResourcePack/`. Work on plugin code and artwork under `WolfHouse-Minecraft-Code/plugins/<PluginName>`. This builder reads those asset folders through `pack-sources.json`.

The public repository was renamed from `minecraft-`. Keep the permanent player URL below unchanged, and never reuse the old repository name: the old URL relies on GitHub's rename redirect. No resource pack was republished as part of the repository organization.

## Build the pack

Open PowerShell in this folder and run:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File .\Build-WolfHouseResourcePack.ps1
```

The result is written to `dist`. A changed source creates the next numbered release. Running the command again without changing any resources keeps the current revision.

Every release includes:

- `WolfHouse-ResourcePack-rN.zip`: the file to upload
- `.sha1`: the value Minecraft requires in `server.properties`
- `.sha256`: an independent integrity check
- `-build-report.txt`: included plugins, file counts, and hashes
- `-server.properties.txt`: safe copy-and-paste settings containing the permanent GitHub URL

The builder does not upload anything, edit the live server, or restart it.

## Normal future updates

For a new BuddyPack outfit or color:

1. Add or change the asset in `../WolfHouse-Minecraft-Code/plugins/BuddyPack/resource-pack/assets`.
2. Run the WolfHouse build command.
3. Publish a new GitHub Release (`r2`, `r3`, and so on) and upload the ZIP with the constant asset name `WolfHouse-ResourcePack.zip`.
4. Keep the existing permanent `resource-pack` URL and update `resource-pack-sha1` from the generated server template.
5. Restart normally.
6. Join with a clean client resource-pack cache and visually check Buddy models and WolfTeams menus.

The permanent download URL is:

```text
https://github.com/IronFaith/minecraft-/releases/latest/download/WolfHouse-ResourcePack.zip
```

WolfTeams updates follow the same steps using `../WolfHouse-Minecraft-Code/plugins/WolfTeams/resourcepack/assets`.

## Add a future Java plugin

The plugin needs a resource-pack source directory containing `pack.mcmeta` and `assets`. Give it a unique namespace such as `assets/newplugin` and target pack format 88.

Add one entry to `pack-sources.json`:

```json
{
  "name": "NewPlugin",
  "path": "../WolfHouse-Minecraft-Code/plugins/NewPlugin/resource-pack"
}
```

Then run the builder. From that point forward, the plugin is collected automatically.

Java and Bedrock packs are different formats. Do not register Geyser or Bedrock packs here.

## Collision protection

If two plugins provide the same destination file, the builder stops and names both plugins. Resolve the ownership deliberately; do not delete a file or let one plugin overwrite another just to make the build pass.

Files under `assets/minecraft` are global and deserve extra attention. If a future plugin needs to share a global JSON file such as `font/default.json`, add an intentional merge rule and a focused test before registering it.

## Run the checks

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File .\tests\PackBuilder.Tests.ps1
```

The three checks cover source inventory and pack-format handling, collision rejection, and root-level ZIP/revision behavior.

## WolfCars release source

WolfCars assets come from `../WolfHouse-Minecraft-Code/plugins/WolfCars/resource-pack`.
The consolidated source was imported from accepted WolfCars `v1.0.22`, original
commit `99b90ec7ecf8e61f26995a6b69a3a0adb9d8935e`. The original repository and
unfinished vehicle worktrees remain available separately. Review all source changes before
preparing a master pack. The pack includes the Corvette,
Toyota-style Trail 4x4, Countach, Challenger, Ducati, runabout, drill buggy, and yacht models.

The source repository is private; its rendered resource-pack assets become downloadable
with the public master pack when that release is approved and published. Plugin JARs are
released separately from the pack. Pair these assets with the matching WolfCars plugin;
the Java-only master pack does not provide a Bedrock model conversion.

Master pack r26 preserves all 546 r25 files and adds six WolfCars assets for the truck
floodlight lenses and original starting, idle and rev audio. Its contents match the
accepted local equipment pack r29; local trial numbers and master release numbers
are independent. Pair r26 with WolfCars 1.0.17 for the accepted acceleration, public
truck cargo, floodlights, engine audio and pedestrian boundaries.

Master pack r27 preserves all 552 r26 files and adds eleven WolfCars drill buggy
assets. Its 563 files exactly match the accepted local drill pack r32. Pair r27
with WolfCars 1.0.22 for the enlarged spinning drill, shared truck-style headlights,
uneven-ground gravity fixes, parked climbing surfaces, and moving player impacts.
The accepted plugin JAR is promoted unchanged after local gameplay approval.
