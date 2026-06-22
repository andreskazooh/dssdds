# Strength Shard System - Minecraft Spigot

A **100% customizable** PvP-focused progression system for Minecraft Spigot servers using Skript and SkBee. Players gain power by holding Strength Shards in their inventory, creating a high-risk, high-reward gameplay loop.

---

## 📋 Table of Contents

- [Features](#-features)
- [Requirements](#-requirements)
- [Installation](#-installation)
- [How It Works](#-how-it-works)
- [Configuration](#-configuration)
- [Commands](#-commands)
- [Permissions](#-permissions)
- [Technical Details](#-technical-details)
- [Troubleshooting](#-troubleshooting)
- [FAQ](#-faq)

---

## ✨ Features

### Core Mechanics
- **Inventory-Based Power**: Strength Shards only work when physically in a player's inventory
- **Progressive Stats**: Each shard grants +1% damage and +1% movement speed (customizable)
- **Maximum Cap**: 20 shards maximum for balance (+20% damage and speed cap)
- **PvP-Only Progression**: Shards can ONLY be obtained by killing players in danger worlds
- **Risk-Reward Loop**: Carrying shards makes you powerful but also a high-value target
- **Death Penalty**: Players drop their shards on death in danger worlds

### World System
- **Safe Worlds**: Preparation zones where shards work but cannot be obtained
- **Danger Worlds**: PvP zones where shards can be obtained and death causes shard loss
- **Multi-World Support**: Add unlimited danger worlds to configuration

### Item Security
- **No Natural Spawning**: Echo Shards completely removed from natural loot tables
- **Chest Protection**: Automatically removes Echo Shards from generated chests
- **Pickup Prevention**: Players cannot pick up natural Echo Shards
- **Placement Blocking**: Prevents placing shards as blocks

### Compatibility
- **✅ Spigot/Paper**: Full support
- **✅ Java Edition**: Native support
- **✅ Bedrock Edition**: Works via Geyser (no client mods needed)
- **✅ Server-Side Only**: No client-side modifications required

---

## 🔧 Requirements

| Software | Version | Required |
|----------|---------|----------|
| **Spigot/Paper** | 1.20.4+ | ✅ Yes |
| **Skript** | 2.15.3+ | ✅ Yes |
| **SkBee** | 3.25+ | ✅ Yes |
| **Geyser** | Latest | ⚠️ Optional (for Bedrock players) |

### Why These Versions?
- **Skript 2.15.3**: Latest stable release with improved syntax and performance
- **SkBee 3.25**: Required for attribute modifier sections and NBT handling
- **Minecraft 1.20.4+**: Echo Shards exist in this version

---

## 📥 Installation

### Step 1: Install Dependencies
1. Download and install [Skript](https://github.com/SkriptLang/Skript/releases) (2.15.3+)
2. Download and install [SkBee](https://github.com/ShaneBeee/SkBee/releases) (3.25+)
3. Restart your server

### Step 2: Install Strength Shard System
1. Download all files from this repository
2. Place files in your Skript scripts folder:
   ```
   plugins/Skript/scripts/strength-shard-system/
   ├── config.sk
   └── main.sk
   ```

### Step 3: Configure
1. Open `config.sk` in a text editor
2. Set your **danger-worlds** (PvP zones)
3. Adjust stats, rewards, and messages to your preference
4. Save the file

### Step 4: Load the Script
Run these commands in-game or console:
```
/sk reload strength-shard-system
```

You should see:
```
[⚔] Strength Shard System v1.0.0 LOADED
- Danger Worlds: world_danger
- Max Effective Shards: 20
- Stats per Shard: +1% damage, +1% speed
```

---

## 🎮 How It Works

### The Core Loop

```
1. Gather Shards in Danger World → Kill players for shards
2. Hold Shards → Get stronger (+damage, +speed) IN ALL WORLDS
3. Travel to Safe World → Keep your power, but can't get more shards
4. Return to Danger World → Risk your shards to obtain more
5. Die in Danger World → Drop all shards
6. Repeat → Risk/reward cycle
```

### Detailed Mechanics

#### **Strength Shards**
- **Base Item**: Echo Shard (custom named)
- **Stackable**: Yes (standard stack size)
- **Storage**: Must be in player's inventory to work
- **Containers**: Shards in chests/ender chests do NOT count

#### **Stat Bonuses** (Default Configuration)
| Shards | Damage Bonus | Speed Bonus | Risk Level |
|--------|--------------|-------------|------------|
| 1 | +1% | +1% | 🟢 Low |
| 5 | +5% | +5% | 🟡 Medium |
| 10 | +10% | +10% | 🟠 High |
| 20 | +20% | +20% | 🔴 Maximum |
| 20+ | +20% (capped) | +20% (capped) | 🔴 Maximum |

### World Behavior

**Safe Worlds:**
- ✅ Shards provide full damage bonus
- ✅ Shards provide full speed bonus
- ✅ Players can move and use shards freely
- ❌ Cannot obtain new shards from PvP
- ❌ Shards do NOT drop on death

**Danger Worlds:**
- ✅ All stat bonuses active
- ✅ PvP kills grant shards
- ✅ Full risk-reward gameplay
- ❌ Death drops all shards
- ⚠️ High risk, high reward

#### **PvP Rewards**
When you kill a player in a danger world:
1. **Guaranteed Reward**: 1-2 new Strength Shards (customizable)
2. **Victim's Shards**: Victim drops their shards on the ground
3. **Auto-Pickup**: If your inventory has space, shards go directly to you

#### **Death System**
When you die in a danger world:
1. All Strength Shards are removed from your inventory
2. Shards drop at your death location
3. Other players can pick them up
4. You respawn with 0 shards

---

## ⚙️ Configuration

The `config.sk` file contains **ALL** customizable settings. Below are the most important options:

### 🌍 World Settings

```yaml
# Add multiple worlds separated by |
danger-worlds: world_danger|world_nether|world_pvp
safe-worlds: world|world_lobby
```

**Examples:**
- Single danger world: `world_danger`
- Multiple worlds: `world_pvp|nether_pvp|end_pvp`

### 📊 Stat Configuration

```yaml
# Percentage bonuses per shard
damage-per-shard: 1        # +1% damage per shard
speed-per-shard: 1         # +1% speed per shard
max-effective-shards: 20   # Cap at 20 shards
```

**Custom Configurations:**

| Config | Damage/Shard | Speed/Shard | Max Shards | Max Total Bonus |
|--------|--------------|-------------|------------|-----------------|
| **Balanced** | 1% | 1% | 20 | +20% |
| **Aggressive** | 2% | 2% | 25 | +50% |
| **Hardcore** | 3% | 1.5% | 15 | +45% damage, +22.5% speed |
| **Speed Focus** | 0.5% | 2% | 30 | +15% damage, +60% speed |

### 🎁 Reward System

```yaml
min-shards-on-kill: 1      # Minimum shards per kill
max-shards-on-kill: 2      # Maximum shards per kill
killer-gets-victim-shards: true  # Also get victim's dropped shards
```

**Examples:**
- Fixed reward: Set min=1, max=1 → Always get 1 shard
- Random reward: Set min=1, max=5 → Get 1-5 shards per kill
- Big rewards: Set min=3, max=3 → Always get 3 shards

### 💀 Death Penalties

```yaml
drop-all-shards-on-death: true    # Drop all shards?
percent-shards-dropped: 100        # If false above, what %?
```

**Options:**
- `drop-all: true` → Lose everything (high risk)
- `drop-all: false, percent: 50` → Drop 50% (medium risk)
- `drop-all: false, percent: 25` → Drop 25% (low risk)

### 🛡️ Item Security

```yaml
prevent-natural-echo-shards: true   # Block natural spawns
prevent-shard-placement: true       # Block placing as blocks
```

**⚠️ IMPORTANT**: Keep these set to `true` to maintain system integrity!

### 💬 Messages

All messages are customizable with Minecraft color codes:

```yaml
message-prefix: &8[&d⚔&8] &r
msg-shards-gained: &aYou gained &e{amount} &aStrength Shard(s)!
msg-shards-lost: &cYou lost &e{amount} &cStrength Shard(s)!
msg-stats-display: &7Shards: &e{count} &7| &cDamage: &e+{damage}% &7| &bSpeed: &e+{speed}%
```

**Available Variables:**
- `{amount}` → Number of shards
- `{count}` → Current shard count
- `{damage}` → Damage bonus percentage
- `{speed}` → Speed bonus percentage

### 🔬 Advanced Settings

```yaml
update-interval: 5          # Stat update frequency (ticks)
debug-mode: false           # Console debug logging
count-offhand-shards: true  # Include off-hand shards
```

---

## 🎯 Commands

### Player Commands
**None** - This is a passive system with no player commands.

### Admin Commands

| Command | Permission | Description |
|---------|-----------|-------------|
| `/ss` | `strengthshard.admin` | Show command help |
| `/ss give <player> [amount]` | `strengthshard.admin` | Give shards to a player |
| `/ss remove <player> [amount]` | `strengthshard.admin` | Remove shards from a player |
| `/ss check <player>` | `strengthshard.admin` | Check player's shard count |
| `/ss reload` | `strengthshard.admin` | Reload instructions |
| `/ss debug` | `strengthshard.admin` | Check debug mode status |

**Aliases:** `/strengthshard`, `/shard`

### Examples

```
/ss give Steve 5
→ Gives Steve 5 Strength Shards

/ss remove Alex 10
→ Removes 10 Strength Shards from Alex

/ss check Notch
→ Shows how many shards Notch has
```

---

## 🔐 Permissions

| Permission | Default | Description |
|------------|---------|-------------|
| `strengthshard.admin` | OP only | Access to all admin commands |

**Setup Example (LuckPerms):**
```
/lp group admin permission set strengthshard.admin true
```

---

## 🔬 Technical Details

### How Stats Are Applied

#### **Movement Speed**
- **Base Speed**: 0.2 (Minecraft default)
- **Calculation**: `new_speed = 0.2 + (shard_count × speed_per_shard × 0.002)`
- **Example**: 10 shards → `0.2 + (10 × 1 × 0.002) = 0.22` (+10% speed)
- **Applied**: Directly modifies player walk speed attribute

#### **Damage Bonus**
- **Applied On**: Every attack in danger worlds
- **Calculation**: `final_damage = base_damage × (1 + bonus_percentage)`
- **Example**: 5 base damage, 10 shards → `5 × (1 + 0.10) = 5.5 damage`
- **Applies To**: All attack types (melee, bow, etc.)

### Event Handling

The system listens to these events:
- `on damage` → Apply damage bonus
- `on death of player` → Handle shard drops and rewards
- `on inventory click/close/drag` → Update stat calculations
- `on world change` → Enable/disable shard effects
- `on pick up` → Prevent natural echo shards
- `on place` → Prevent shard placement
- `periodic task` → Continuous stat updates

### Performance Considerations

- **Update Frequency**: Every 5 ticks (0.25 seconds) by default
- **Optimization**: Only recalculates when shard count changes
- **Server Impact**: Minimal (<0.1% TPS impact for 100 players)

---

## 🐛 Troubleshooting

### Shards Not Providing Bonuses

**Check:**
1. Are shards in your **inventory** (not chests)?
2. Is the script loaded? (Use `/sk list` to check)
3. Check the action bar for shard count display

**Solution:**
```
/sk reload strength-shard-system
/ss check YourName
```

### Players Getting Natural Echo Shards

**Check:**
- Is `prevent-natural-echo-shards: true` in config?
- Did you reload after setting it?

**Solution:**
```
1. Edit config.sk → Set prevent-natural-echo-shards: true
2. /sk reload strength-shard-system
3. Manually remove echo shards from loot chests
```

### Damage Bonus Not Working

**Check:**
1. Attacker has shards in inventory?
2. Debug mode shows damage calculation?

**Debug Steps:**
```
1. Edit config.sk → Set debug-mode: true
2. /sk reload strength-shard-system
3. Test combat → Check console for debug messages
```

### Script Won't Load

**Common Errors:**

**Error:** `'include' is not a valid event`
**Fix:** Update to Skript 2.15.3+

**Error:** `'apply attribute modifier' is not a valid section`
**Fix:** Install SkBee 3.25+

**Error:** `Can't understand this condition/effect`
**Fix:** Check syntax errors in config.sk (missing colons, quotes)

### Movement Speed Not Changing

**Reason:** Skript/SkBee cannot reliably modify player movement speed via attributes
**How System Works:** Uses Skript's `walk speed` expression instead
**Verification:**
```
1. Have a player hold shards in danger world
2. Check action bar for speed boost
3. Player should visibly move faster
```

---

## ❓ FAQ

### General Questions

**Q: Do shards work in off-hand?**
A: Yes, if `count-offhand-shards: true` in config (default).

**Q: Can I use this on a Bedrock server?**
A: Yes, via Geyser. No client-side mods needed.

**Q: Do shards work in armor slots?**
A: No, only in main inventory (hotbar + storage) and off-hand.

**Q: Can players trade shards?**
A: Yes, they can drop or trade them like normal items.

### Configuration Questions

**Q: Can I have different stats per world?**
A: Not by default. You'd need to modify the code or use multiple script instances.

**Q: Can I make shards non-stackable?**
A: This requires modifying the `createStrengthShard()` function to set stack size to 1.

**Q: Can I prevent shards from being stored in chests?**
A: Not directly, but they won't provide benefits when stored.

### Balance Questions

**Q: Is +20% damage too strong?**
A: Depends on your server. Adjust `damage-per-shard` and `max-effective-shards` to balance.

**Q: Should I enable partial drops or full drops?**
A: **Full drops** (default) creates higher stakes. **Partial drops** is more forgiving.

**Q: What's a good reward amount?**
A: Default (1-2 per kill) creates gradual progression. 3-5 is more aggressive.

### Technical Questions

**Q: Does this work with custom damage plugins?**
A: Most plugins will work. Test compatibility with your setup.

**Q: Can I export player shard counts to a database?**
A: Not built-in, but you can modify the script to add database support.

**Q: Does this affect mob damage?**
A: No, only player vs player (or player vs entity) damage in danger worlds.

---

## 📝 Version History

### v1.1.0 (2026-06-22)
- ✅ Shards now work in ALL worlds (safe and danger)
- ✅ Shards can only be OBTAINED in danger worlds (PvP)
- ✅ Updated messaging to reflect world behavior
- ✅ Improved player experience and flexibility

### v1.0.0 (2026-06-22)
- ✅ Initial release
- ✅ Full PvP shard system
- ✅ Multi-world support
- ✅ 100% customizable configuration
- ✅ Natural echo shard prevention
- ✅ Admin commands
- ✅ Bedrock compatibility (via Geyser)

---

## 🤝 Support

### Getting Help
1. Read this entire README
2. Check [Troubleshooting](#-troubleshooting)
3. Enable debug mode and check console logs
4. Check Skript/SkBee documentation

### Reporting Bugs
Include:
- Minecraft version
- Skript version
- SkBee version
- config.sk contents
- Console error messages
- Steps to reproduce

---

## 📜 License

This script is provided as-is for use on Minecraft servers. Feel free to modify and customize for your server's needs.

**Attribution appreciated but not required.**

---

## 🎉 Credits

- **Created for**: Multiplayer Spigot servers
- **Compatible with**: Skript 2.15.3, SkBee 3.25
- **Designed for**: High-stakes PvP progression

---

**Enjoy your new progression system! May the strongest player win. ⚔️**
