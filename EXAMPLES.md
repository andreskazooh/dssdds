# Configuration Examples & Use Cases

This guide provides real-world configuration examples for different server types and gameplay styles.

---

## 📚 Table of Contents

1. [Server Type Configurations](#server-type-configurations)
2. [Balance Presets](#balance-presets)
3. [World Setup Examples](#world-setup-examples)
4. [Custom Message Examples](#custom-message-examples)
5. [Common Scenarios](#common-scenarios)

---

## Server Type Configurations

### 1. Small Survival Server (10-30 players)

**Goal:** Balanced progression, friendly competition

```yaml
# WORLD SETTINGS
danger-worlds: world_nether
safe-worlds: world

# STATS
damage-per-shard: 1
speed-per-shard: 1
max-effective-shards: 15

# REWARDS
min-shards-on-kill: 1
max-shards-on-kill: 1
killer-gets-victim-shards: true

# DEATH
drop-all-shards-on-death: true
percent-shards-dropped: 100

# ADVANCED
update-interval: 5
debug-mode: false
```

**Why these settings:**
- Lower cap (15) keeps power differences manageable
- Fixed 1 shard per kill creates predictable progression
- Nether-only danger zone encourages resource gathering risk
- Full drop penalty maintains stakes

---

### 2. Hardcore PvP Server (50+ players)

**Goal:** High stakes, maximum risk-reward

```yaml
# WORLD SETTINGS
danger-worlds: world|world_nether|world_the_end
safe-worlds: spawn_world

# STATS
damage-per-shard: 2
speed-per-shard: 1.5
max-effective-shards: 30

# REWARDS
min-shards-on-kill: 2
max-shards-on-kill: 5
killer-gets-victim-shards: true

# DEATH
drop-all-shards-on-death: true
percent-shards-dropped: 100

# MESSAGES
message-prefix: &c[&4⚔&c] &r
msg-shards-gained: &4You claimed &c{amount} &4shards from your victim!
msg-shards-lost: &c&l💀 YOU LOST {amount} SHARDS! 💀

# ADVANCED
update-interval: 5
debug-mode: false
```

**Why these settings:**
- All worlds are dangerous (except spawn)
- Higher stat bonuses (2% damage, 1.5% speed)
- Higher cap (30 shards)
- 2-5 shards per kill creates big swings
- Aggressive messaging reinforces hardcore theme

---

### 3. Casual/Fun Server (any size)

**Goal:** Fun power progression, low frustration

```yaml
# WORLD SETTINGS
danger-worlds: world_pvp
safe-worlds: world|world_resource

# STATS
damage-per-shard: 1.5
speed-per-shard: 1.5
max-effective-shards: 20

# REWARDS
min-shards-on-kill: 2
max-shards-on-kill: 3
killer-gets-victim-shards: true

# DEATH
drop-all-shards-on-death: false
percent-shards-dropped: 50

# MESSAGES
message-prefix: &a[&2⚔&a] &r
msg-shards-gained: &a+{amount} Shards! Keep fighting!
msg-shards-lost: &eYou lost {amount} shards, but kept the rest!

# ADVANCED
update-interval: 10
debug-mode: false
```

**Why these settings:**
- Dedicated PvP world only
- Only lose 50% on death (less frustrating)
- Faster progression (2-3 per kill)
- Friendly messaging
- Lower update rate (less server load)

---

### 4. Competitive/Esports Server

**Goal:** Skill-based, tournament-ready

```yaml
# WORLD SETTINGS
danger-worlds: arena_1|arena_2|arena_3
safe-worlds: lobby|spectator

# STATS
damage-per-shard: 1
speed-per-shard: 1
max-effective-shards: 10

# REWARDS
min-shards-on-kill: 1
max-shards-on-kill: 1
killer-gets-victim-shards: true

# DEATH
drop-all-shards-on-death: true
percent-shards-dropped: 100

# MESSAGES
message-prefix: &9[&1Arena&9] &r
msg-shards-gained: &9+{amount} Power
msg-shards-lost: &c-{amount} Power

# ADVANCED
update-interval: 3
debug-mode: false
```

**Why these settings:**
- Multiple arena worlds
- Lower cap (10) keeps matches skill-based
- Predictable rewards (always 1)
- Fast updates for responsiveness
- Clean, professional messaging

---

### 5. RP/Faction Server

**Goal:** Territory control, faction warfare

```yaml
# WORLD SETTINGS
danger-worlds: warzone|resource_world|faction_world
safe-worlds: spawn|market

# STATS
damage-per-shard: 1
speed-per-shard: 0.5
max-effective-shards: 25

# REWARDS
min-shards-on-kill: 1
max-shards-on-kill: 3
killer-gets-victim-shards: true

# DEATH
drop-all-shards-on-death: true
percent-shards-dropped: 100

# MESSAGES
message-prefix: &6[&eWarzone&6] &r
msg-shards-gained: &6You looted &e{amount} &6power shard(s)!
msg-shards-lost: &cYour power shards were taken!
msg-stats-display: &6Power: {count} &8| &cDamage: +{damage}% &8| &bSpeed: +{speed}%

# ADVANCED
update-interval: 5
debug-mode: false
```

**Why these settings:**
- Multiple danger zones for different activities
- Less speed bonus (0.5%) to prevent kiting
- Variable rewards (1-3) adds unpredictability
- RP-themed messaging

---

## Balance Presets

### Ultra-Conservative
**For servers worried about balance:**

```yaml
damage-per-shard: 0.5
speed-per-shard: 0.5
max-effective-shards: 20
min-shards-on-kill: 1
max-shards-on-kill: 1
drop-all-shards-on-death: true
```

**Result:** Max +10% bonuses, slow progression

---

### Moderate
**Recommended starting point:**

```yaml
damage-per-shard: 1
speed-per-shard: 1
max-effective-shards: 20
min-shards-on-kill: 1
max-shards-on-kill: 2
drop-all-shards-on-death: true
```

**Result:** Max +20% bonuses, steady progression

---

### Aggressive
**For action-packed servers:**

```yaml
damage-per-shard: 2
speed-per-shard: 2
max-effective-shards: 25
min-shards-on-kill: 2
max-shards-on-kill: 4
drop-all-shards-on-death: true
```

**Result:** Max +50% bonuses, fast progression

---

### Extreme
**High-risk, high-reward:**

```yaml
damage-per-shard: 3
speed-per-shard: 2
max-effective-shards: 30
min-shards-on-kill: 3
max-shards-on-kill: 10
drop-all-shards-on-death: true
```

**Result:** Max +90% damage, +60% speed, chaotic gameplay

---

## World Setup Examples

### Example 1: Single Danger World
**Nether-only PvP:**

```yaml
danger-worlds: world_nether
```

**Use case:** Overworld is safe for building, Nether is PvP zone

---

### Example 2: Multiple Danger Worlds
**All resource worlds dangerous:**

```yaml
danger-worlds: world_nether|world_the_end|resource_world
```

**Use case:** Multiple zones for resource gathering and PvP

---

### Example 3: Dedicated Arenas
**Tournament/arena setup:**

```yaml
danger-worlds: arena_pvp|arena_1v1|arena_team
```

**Use case:** Structured PvP in designated areas only

---

### Example 4: Everything Dangerous
**Hardcore survival:**

```yaml
danger-worlds: world|world_nether|world_the_end
safe-worlds: spawn
```

**Use case:** Only spawn is safe, everywhere else is PvP

---

### Example 5: Rotating Danger Zones
**Weekly rotation (manual config change):**

**Week 1:**
```yaml
danger-worlds: world_nether
```

**Week 2:**
```yaml
danger-worlds: world_the_end
```

**Week 3:**
```yaml
danger-worlds: world_nether|world_the_end
```

---

## Custom Message Examples

### 1. Medieval Theme

```yaml
message-prefix: &6⚔ &r
msg-shards-gained: &aYou have claimed &e{amount} &ashard(s) of power from your foe!
msg-shards-lost: &cYou have fallen! Your {amount} shard(s) have been scattered!
msg-stats-display: &6⚔ &7Power Shards: &e{count} &8| &cBattle Prowess: &e+{damage}% &8| &bAgility: &e+{speed}%
msg-shard-restricted: &cThese mystical shards can only be claimed through honorable combat!
```

---

### 2. Sci-Fi Theme

```yaml
message-prefix: &b[&3⚡&b] &r
msg-shards-gained: &b+{amount} Energy Crystal(s) absorbed!
msg-shards-lost: &cPower loss detected: {amount} crystal(s)
msg-stats-display: &3⚡ &7Crystals: &b{count} &8| &cWeapon Output: &e+{damage}% &8| &bMobility: &e+{speed}%
msg-shard-restricted: &cEnergy crystals must be extracted from fallen opponents!
```

---

### 3. Dark/Edgy Theme

```yaml
message-prefix: &4[&c☠&4] &r
msg-shards-gained: &4You ripped &c{amount} &4soul shard(s) from your victim!
msg-shards-lost: &c&l💀 {amount} SOUL(S) LOST 💀
msg-stats-display: &4☠ &cSouls: {count} &8| &4Corruption: +{damage}% &8| &cDark Speed: +{speed}%
msg-shard-restricted: &4Soul shards can only be harvested through bloodshed!
```

---

### 4. Minimal/Clean

```yaml
message-prefix: &7[&f+&7] &r
msg-shards-gained: &7+{amount}
msg-shards-lost: &7-{amount}
msg-stats-display: &7{count} &8| &7+{damage}% &8| &7+{speed}%
msg-shard-restricted: &7PvP only.
```

---

### 5. Over-the-Top

```yaml
message-prefix: &6&l✧ &e&l★ &r
msg-shards-gained: &6&l✧ &e&lEPIC! &6&lYou gained &e&l{amount} &6&lLEGENDARY SHARD(S)! &e&l★ &6&l✧
msg-shards-lost: &c&l💔 DEFEATED! &4&lYou lost &c&l{amount} &4&lshard(s)! &c&l💔
msg-stats-display: &e&l★ &6Power: {count} &8| &c&lDamage: +{damage}% &8| &b&lSpeed: +{speed}% &e&l★
msg-shard-restricted: &c&lNO CHEATING! &4These shards are earned through &c&lGLORIOUS COMBAT!
```

---

## Common Scenarios

### Scenario 1: New Player Protection

**Problem:** New players get dominated

**Solution:** Create a newbie-protected zone

```yaml
danger-worlds: world_nether|world_the_end
safe-worlds: world|newbie_world
```

Then use WorldGuard in `world` to create safe zones around spawn.

---

### Scenario 2: Event Days

**Problem:** Want special events with boosted rewards

**Manual config change for event:**

```yaml
# Normal config
min-shards-on-kill: 1
max-shards-on-kill: 2

# Weekend event config
min-shards-on-kill: 5
max-shards-on-kill: 10
```

Then reload: `/sk reload strength-shard-system`

---

### Scenario 3: Seasonal Reset

**Problem:** Some players have too many shards

**Solution:**
1. Announce reset in advance
2. On reset day, use: `/ss remove PlayerName 999` for all players
3. Or reload script (auto-clears data)
4. Optional: Give participation rewards

---

### Scenario 4: Tournament Mode

**Problem:** Want equal start for tournaments

**Setup:**
```yaml
danger-worlds: tournament_arena
max-effective-shards: 5
min-shards-on-kill: 1
max-shards-on-kill: 1
```

**Before tournament:**
- Remove all shards from participants
- Everyone starts at 0
- Quick progression (max 5)
- Skill-focused

---

### Scenario 5: Guild/Team Systems

**Problem:** Want to reward team play

**Config:**
```yaml
killer-gets-victim-shards: false
min-shards-on-kill: 3
max-shards-on-kill: 5
```

**Result:** Victim's shards stay on ground, team can share them

---

### Scenario 6: Slow Progression

**Problem:** Players max out too fast

**Solution:**
```yaml
max-effective-shards: 50
damage-per-shard: 0.5
speed-per-shard: 0.5
min-shards-on-kill: 1
max-shards-on-kill: 1
```

**Result:** Need 50 shards for max power (+25%), takes longer

---

### Scenario 7: High-Death Penalty

**Problem:** Want to punish deaths more

**Solution:**
```yaml
drop-all-shards-on-death: true
min-shards-on-kill: 1
max-shards-on-kill: 1
```

**Result:** Lose everything, slow to regain

---

### Scenario 8: Forgiving Mode

**Problem:** Players quit after losing shards

**Solution:**
```yaml
drop-all-shards-on-death: false
percent-shards-dropped: 25
min-shards-on-kill: 2
max-shards-on-kill: 3
```

**Result:** Only lose 25%, fast to recover

---

## Testing Configurations

### Quick Test Config
**For testing mechanics:**

```yaml
danger-worlds: world
max-effective-shards: 5
min-shards-on-kill: 5
max-shards-on-kill: 5
update-interval: 1
debug-mode: true
```

---

### Balance Test Config
**For finding the right balance:**

```yaml
# Test version 1
damage-per-shard: 1
speed-per-shard: 1

# Test version 2
damage-per-shard: 1.5
speed-per-shard: 1.5

# Test version 3
damage-per-shard: 2
speed-per-shard: 2

# Play for a day with each, gather feedback
```

---

## Configuration Tips

1. **Start conservative** - It's easier to increase power than decrease
2. **Test with players** - Balance on paper ≠ balance in practice
3. **Gather feedback** - Ask players what feels right
4. **Iterate** - Make small changes, test, repeat
5. **Document changes** - Keep notes on what works
6. **Watch the meta** - Adjust if one strategy dominates
7. **Seasonal resets** - Fresh starts keep things interesting

---

## Quick Reference: Config Impact

| Setting | Increase Effect | Decrease Effect |
|---------|----------------|-----------------|
| `damage-per-shard` | Faster kills, more snowball | Longer fights, less snowball |
| `speed-per-shard` | More kiting, easier escape | Harder to run, more standoffs |
| `max-effective-shards` | Higher power ceiling | Lower power ceiling |
| `min/max-shards-on-kill` | Faster progression | Slower progression |
| `drop-all-shards` | Higher stakes | More forgiving |
| `percent-shards-dropped` | Higher risk | Lower risk |
| `update-interval` | Less load, less responsive | More load, more responsive |

---

**Experiment and find what works for YOUR server! ⚔️**
