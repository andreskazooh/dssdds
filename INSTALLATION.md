# Installation Guide - Strength Shard System

This guide will walk you through installing and configuring the Strength Shard System on your Spigot/Paper Minecraft server.

---

## 📋 Pre-Installation Checklist

Before starting, make sure you have:

- [ ] A Spigot or Paper server (1.20.4+)
- [ ] Server admin/owner access
- [ ] FTP/SFTP access OR file manager access
- [ ] Ability to restart the server
- [ ] Basic knowledge of Minecraft commands

---

## Step 1: Install Required Plugins

### 1.1 Install Skript

1. **Download Skript 2.15.3 or newer**:
   - Visit: https://github.com/SkriptLang/Skript/releases
   - Download the latest `Skript-2.15.3.jar` (or newer)

2. **Upload to your server**:
   - Place `Skript-2.15.3.jar` in the `plugins/` folder
   - Example path: `/server/plugins/Skript-2.15.3.jar`

3. **Verify Installation**:
   - Restart your server
   - Check console for: `[Skript] Skript 2.15.3 has been enabled`
   - Run `/sk info` in-game to confirm

### 1.2 Install SkBee

1. **Download SkBee 3.25 or newer**:
   - Visit: https://github.com/ShaneBeee/SkBee/releases
   - Download the latest `SkBee-3.25.0.jar` (or newer)

2. **Upload to your server**:
   - Place `SkBee-3.25.0.jar` in the `plugins/` folder
   - Example path: `/server/plugins/SkBee-3.25.0.jar`

3. **Verify Installation**:
   - Restart your server
   - Check console for: `[SkBee] SkBee 3.25.0 has been enabled`
   - SkBee loads AFTER Skript

### 1.3 (Optional) Install Geyser for Bedrock Support

If you want Bedrock (Pocket Edition) players to use your server:

1. **Download Geyser-Spigot**:
   - Visit: https://geysermc.org/download
   - Download the Spigot version

2. **Upload and configure**:
   - Place in `plugins/` folder
   - Restart server
   - Configure in `plugins/Geyser-Spigot/config.yml`
   - Bedrock players can now join!

---

## Step 2: Create Script Folders

### 2.1 Locate Skript Scripts Folder

After Skript loads, it creates a scripts folder:

```
/server/plugins/Skript/scripts/
```

### 2.2 Create System Folder

Inside the scripts folder, create a new folder:

```
/server/plugins/Skript/scripts/strength-shard-system/
```

**Final structure:**
```
plugins/
├── Skript/
│   ├── scripts/
│   │   └── strength-shard-system/
│   │       ├── config.sk (you'll add this)
│   │       └── main.sk (you'll add this)
```

---

## Step 3: Upload Script Files

### 3.1 Upload config.sk

1. Download or copy the `config.sk` file from this repository
2. Upload to: `/server/plugins/Skript/scripts/strength-shard-system/config.sk`

### 3.2 Upload main.sk

1. Download or copy the `main.sk` file from this repository
2. Upload to: `/server/plugins/Skript/scripts/strength-shard-system/main.sk`

### 3.3 Verify Upload

Your folder should now look like:
```
strength-shard-system/
├── config.sk    (configuration file)
└── main.sk      (main script)
```

---

## Step 4: Configure the System

### 4.1 Open config.sk

Use your preferred text editor:
- **FTP**: Download, edit locally, re-upload
- **File Manager**: Use built-in editor
- **SSH**: Use `nano` or `vim`

### 4.2 Set Your Danger Worlds

Find this line:
```yaml
danger-worlds: world_danger
```

**Change it to your actual world name(s):**

**Single world:**
```yaml
danger-worlds: world_nether
```

**Multiple worlds:**
```yaml
danger-worlds: world_nether|world_pvp|world_end
```

**⚠️ IMPORTANT**: 
- Use exact world folder names (case-sensitive)
- Separate multiple worlds with `|`
- No spaces around `|`

### 4.3 Verify World Names

Not sure of your world names? Run this command in-game:
```
/skript eval send "%all worlds%" to player
```

Or check your server folder for world folders:
```
/server/world
/server/world_nether
/server/world_the_end
```

### 4.4 Adjust Stats (Optional)

**Default settings (balanced):**
```yaml
damage-per-shard: 1
speed-per-shard: 1
max-effective-shards: 20
```

**Aggressive server:**
```yaml
damage-per-shard: 2
speed-per-shard: 2
max-effective-shards: 25
```

**Hardcore server:**
```yaml
damage-per-shard: 3
speed-per-shard: 1.5
max-effective-shards: 15
```

### 4.5 Configure Rewards

**Default (1-2 shards per kill):**
```yaml
min-shards-on-kill: 1
max-shards-on-kill: 2
```

**Fixed reward (always 1):**
```yaml
min-shards-on-kill: 1
max-shards-on-kill: 1
```

**Big rewards (3-5 per kill):**
```yaml
min-shards-on-kill: 3
max-shards-on-kill: 5
```

### 4.6 Save Changes

Save the `config.sk` file after making changes.

---

## Step 5: Load the Script

### 5.1 Load via Command

In-game or in console, run:
```
/sk reload strength-shard-system
```

Or reload all scripts:
```
/sk reload all
```

### 5.2 Verify Successful Load

**Check console output:**
```
[⚔] Strength Shard System v1.0.0 LOADED
- Danger Worlds: world_danger
- Max Effective Shards: 20
- Stats per Shard: +1% damage, +1% speed
```

**If you see errors:**
- Check syntax in `config.sk` (missing colons, quotes)
- Verify Skript and SkBee are installed correctly
- Check the [Troubleshooting](#troubleshooting) section below

---

## Step 6: Test the System

### 6.1 Give Yourself Shards

Run this command:
```
/ss give YourName 10
```

You should receive 10 Strength Shards.

### 6.2 Check Shard Count

```
/ss check YourName
```

Should display:
```
[⚔] YourName has 10 Strength Shard(s) (capped at 20)
```

### 6.3 Test in Danger World

1. **Go to your danger world:**
   ```
   /tp @s ~ ~ ~ world_danger
   ```
   Or use a world portal

2. **Check your stats:**
   - Look at your action bar
   - Should show: `Shards: 10 | Damage: +10% | Speed: +10%`

3. **Test speed:**
   - Walk around
   - You should move noticeably faster

4. **Test damage:**
   - Attack a mob or player
   - Damage should be increased

### 6.4 Test Safe World

1. **Return to safe world:**
   ```
   /tp @s ~ ~ ~ world
   ```

2. **Verify stats are disabled:**
   - Action bar should disappear
   - Speed should return to normal
   - Damage bonus should not apply

### 6.5 Test PvP Rewards

1. **Have two players enter danger world**
2. **One player kills the other**
3. **Killer should receive:**
   - 1-2 new Strength Shards (based on config)
   - Message: "You gained X Strength Shard(s)!"
4. **Victim should:**
   - Drop all their shards
   - See message: "You lost X Strength Shard(s)!"

---

## Step 7: Configure Permissions

### 7.1 Admin Permission

Give admins access to commands:

**Using LuckPerms:**
```
/lp group admin permission set strengthshard.admin true
```

**Using PermissionsEx:**
```
/pex group admin add strengthshard.admin
```

**Using GroupManager:**
```
/mangaddp admin strengthshard.admin
```

### 7.2 Test Permissions

Log in as a non-admin and try:
```
/ss give PlayerName 5
```

Should see: "You don't have permission to use this command"

---

## Step 8: Final Configuration

### 8.1 Remove Natural Echo Shards (Important!)

Since the system is based on Echo Shards, we need to prevent them from spawning naturally:

**This is AUTOMATICALLY handled by:**
```yaml
prevent-natural-echo-shards: true
```

**But you should also:**

1. **Remove existing Echo Shards from world chests:**
   - Use a plugin like WorldEdit
   - Or manually clear ancient city chests

2. **Disable Echo Shard loot:**
   - Create a datapack that removes echo shards from loot tables
   - Or use the built-in prevention system

### 8.2 Configure Server Rules

Let your players know:
1. Strength Shards are PvP-only currency
2. Carrying shards makes you powerful but a target
3. Death in danger worlds = lose all shards
4. Safe worlds don't allow progression

### 8.3 Create Spawn Protection (Recommended)

In your danger world, create a protected spawn area:
- Use WorldGuard or similar
- Disable PvP in spawn zone
- Players can prepare safely before leaving spawn

---

## Troubleshooting

### Script Won't Load

**Error: `include is not a valid event`**
- **Cause**: Skript version too old
- **Fix**: Update to Skript 2.15.3+

**Error: `attribute modifier is not a section`**
- **Cause**: SkBee not installed or outdated
- **Fix**: Install SkBee 3.25+

**Error: `Can't understand this condition`**
- **Cause**: Syntax error in config.sk
- **Fix**: Check for missing colons, quotes, or typos

### Shards Not Working

**Shards don't provide bonuses:**
1. Verify you're in a danger world
2. Check config for correct world name
3. Reload script: `/sk reload strength-shard-system`

**Shards not dropping on death:**
1. Verify death occurred in danger world
2. Check `drop-all-shards-on-death: true` in config
3. Enable debug mode and check console

### Permission Issues

**Can't use /ss commands:**
1. Verify permission: `strengthshard.admin`
2. Check you're OP: `/op YourName`
3. Reload permissions plugin

### Performance Issues

**Server lagging:**
1. Increase `update-interval` in config (try 10 or 20 ticks)
2. Reduce `max-effective-shards` to lower cap
3. Disable debug mode

---

## Post-Installation Checklist

After installation, verify:

- [ ] Script loads without errors
- [ ] Danger worlds are correctly configured
- [ ] Shards provide bonuses in danger worlds
- [ ] Shards do NOT provide bonuses in safe worlds
- [ ] PvP kills grant shards
- [ ] Deaths drop shards
- [ ] Natural echo shards are prevented
- [ ] Admin commands work
- [ ] Permissions are set up
- [ ] Players can understand the system

---

## Next Steps

### 1. Announce to Players

Let your players know about the new system:
```
📢 NEW SYSTEM: Strength Shards
🗡️ Kill players in danger worlds to gain power!
⚔️ Each shard = +1% damage and speed
💀 Die = lose all shards
🎯 High risk, high reward!
```

### 2. Create Tutorials

- In-game signs explaining the system
- Video or text tutorial on your website
- Discord announcements

### 3. Monitor Balance

After a few days:
- Check if anyone has too many shards (adjust max cap)
- See if rewards are too high/low (adjust kill rewards)
- Verify death penalty isn't too harsh (consider partial drops)

### 4. Customize Further

See `CUSTOMIZATION.md` for:
- Custom stat formulas
- Per-world configurations
- Integration with other plugins
- Advanced modifications

---

## Getting Help

If you encounter issues:

1. **Read this guide completely**
2. **Check README.md for details**
3. **Enable debug mode:**
   ```yaml
   debug-mode: true
   ```
   Then reload and check console

4. **Verify versions:**
   ```
   /sk info
   /skbee version
   ```

5. **Check Skript documentation:**
   - https://docs.skriptlang.org/
   - https://github.com/ShaneBeee/SkBee/wiki

---

## Congratulations! 🎉

Your Strength Shard System is now installed and ready to use!

Players can now engage in high-stakes PvP combat with a risk-reward progression system that creates natural gameplay loops and emergent interactions.

**Good luck, and may the best warrior win! ⚔️**
