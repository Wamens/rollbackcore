# RollbackCore

RollbackCore is a modernized fork of the original plugin for fast arena rollbacks, region snapshots, watchdog-based recovery, and whole-world backup workflows on modern Paper servers.

This fork is updated for Paper `1.21.11`, cleaned up for safer task lifecycle handling, and packaged as a proper multi-module Maven project so it opens more cleanly in IntelliJ IDEA.

## Highlights

- Updated for Paper `1.21.11`
- Supports region copy and paste workflows
- Supports saved rollback regions for arena-style servers
- Includes WatchDog regions that capture block changes for later recovery
- Includes whole-world backup and rollback commands
- Improved shutdown cleanup and task cancellation behavior
- Safer watchdog import handling and path resolution fixes

## Project Layout

- Root project: [pom.xml](C:/Users/Soulj/IdeaProjects/rollbackcore/pom.xml)
- Actual plugin module: [RollbackCore](C:/Users/Soulj/IdeaProjects/rollbackcore/RollbackCore)
- Main plugin class: [RollbackCore/src/main/java/net/shadowxcraft/rollbackcore/Main.java](C:/Users/Soulj/IdeaProjects/rollbackcore/RollbackCore/src/main/java/net/shadowxcraft/rollbackcore/Main.java)
- Plugin metadata: [RollbackCore/src/main/resources/plugin.yml](C:/Users/Soulj/IdeaProjects/rollbackcore/RollbackCore/src/main/resources/plugin.yml)

If you open this repository in IntelliJ IDEA, import the Maven project from the repo root and make sure the `RollbackCore` module is loaded.

## Requirements

- Java 21
- Paper `1.21.11`
- WorldEdit recommended for region selection workflows
- Skript optional

## Building

From the repository root:

```bash
mvn clean package
```

Output jars are created in:

- `RollbackCore/target/RollbackCore-3.6.2.jar`
- `RollbackCore/target/RollbackCore-3.6.2-shaded.jar`

For most server installs, use the shaded jar.

## Installation

1. Build the project with Maven.
2. Drop the built jar into your server's `plugins` folder.
3. Start the server once to generate config files.
4. Stop the server if you want to adjust configuration before production use.
5. Edit `plugins/RollbackCore/config.yml` as needed.

## Commands

Main command:

```text
/rollback
/rb
```

Available subcommands:

- `/rollback help`
- `/rollback reload`
- `/rollback copy <output-file>`
- `/rollback paste [<x> <y> <z> [<world>]] <file> [-clearEntities -ignoreAir]`
- `/rollback addregion <name>`
- `/rollback updateregion <name>`
- `/rollback rollbackregion <name> [-clearEntities -ignoreAir]`
- `/rollback watchdog <rollback|create|remove|import|export>`
- `/rollback worldbackup <worldname> [folder]`
- `/rollback worldrollback <worldname> <to-worldname> [<to-x> <to-y> <to-z>] [folder]`

Permission:

- `rollback.admin`

## Typical Workflows

### Save a WorldEdit Selection

1. Make a WorldEdit cuboid selection.
2. Run `/rollback copy arena_spawn`
3. Paste it later with `/rollback paste arena_spawn`

### Create a Named Rollback Region

1. Make a WorldEdit cuboid selection.
2. Run `/rollback addregion myarena`
3. Roll it back later with `/rollback rollbackregion myarena`

### Use a WatchDog Region

1. Make a WorldEdit cuboid selection.
2. Run `/rollback watchdog create`
3. Let gameplay happen inside the region.
4. Run `/rollback watchdog rollback` to restore tracked block changes.

### Back Up a Whole World

```text
/rollback worldbackup world
```

### Roll Back a Whole World

```text
/rollback worldrollback world lobby
```

This teleports players out of the target world before restoring it from backup.

## Configuration

Current config file:

- [RollbackCore/src/main/resources/config.yml](C:/Users/Soulj/IdeaProjects/rollbackcore/RollbackCore/src/main/resources/config.yml)

Important settings:

- `Config.rollback.targettime`
  Controls how long rollback tasks should work per tick before yielding.
- `Config.rollback.compression`
  Supports `none` or `LZ4`.
- `Config.rollback.unload_chunks`
  Can reduce memory pressure during larger operations, depending on workload.

Recommended tuning:

- Lower `targettime` if your server is sensitive to tick spikes.
- Keep `compression` at `LZ4` or `none` depending on whether you prefer lower disk usage or simpler writes.
- Enable `unload_chunks` only after testing on your own server workload.

## What Was Modernized In This Fork

- Paper API target updated to `1.21.11`
- Plugin metadata refreshed for modern server versions
- Task shutdown and cancellation handling improved
- Watchdog import no longer stores mutable `Location` keys during import
- Safer command handling for console vs player execution
- Better path resolution for saves and watchdog backups
- Modern entity enum names fixed for current Paper APIs
- Repo root converted into a clean parent Maven build

## Notes

- The plugin is significantly safer than the old state of the repo, but large rollback operations should still be tested on a staging server before production rollout.
- Whole-world rollback is powerful and should be treated like an admin-only operation.
- WorldEdit remains the best experience for region selection workflows.

## License

The upstream project includes [LICENSE.txt](C:/Users/Soulj/IdeaProjects/rollbackcore/RollbackCore/LICENSE.txt). Review that file before redistributing modified builds.
