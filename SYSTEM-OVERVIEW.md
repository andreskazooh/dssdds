# System Overview - Visual Guide

A visual representation of how the Strength Shard System works.

---

## 🔄 Core Gameplay Loop

```
┌─────────────────────────────────────────────────────┐
│                   THE CYCLE                          │
└─────────────────────────────────────────────────────┘

     Enter Danger World
            ↓
     ┌──────────────┐
     │  Collect      │
     │  Shards       │ ← Kill Players (+1-2 shards)
     └──────┬───────┘
            │
            ↓
     ┌──────────────┐
     │  Get         │
     │  Stronger    │ ← +1% damage & speed per shard
     └──────┬───────┘
            │
            ↓
     ┌──────────────┐
     │  Become      │
     │  Target      │ ← Other players want your shards
     └──────┬───────┘
            │
            ↓
     ┌──────────────┐
     │  Die in      │
     │  Combat      │ → Drop all shards
     └──────┬───────┘
            │
            ↓
     Return to Start (0 shards)
```

---

## 🌍 World System

```
┌─────────────────────┐           ┌─────────────────────┐
│    SAFE WORLD       │           │   DANGER WORLD      │
│                     │           │                     │
│  ✅ Build           │◄─────────►│  ⚔️  PvP            │
│  ✅ Store items     │           │  💎 Gain shards     │
│  ✅ Prepare         │  Player   │  📈 Stats active    │
│  ❌ No progression  │  Travels  │  💀 Death = loss    │
│  ❌ No stat bonus   │           │  🎯 High risk       │
│                     │           │                     │
└─────────────────────┘           └─────────────────────┘
```

---

## 💎 Shard Mechanics

```
┌──────────────────────────────────────────────────────┐
│              STRENGTH SHARD ITEM                      │
├──────────────────────────────────────────────────────┤
│  Item: Echo Shard (renamed)                          │
│  Name: "Strength Shard" (purple/pink)                │
│  Stackable: Yes                                      │
│  Obtainable: PvP kills only                          │
│                                                      │
│  LORE:                                               │
│  • Each shard in inventory grants:                   │
│  • +1% Damage                                        │
│  • +1% Movement Speed                                │
│  • ⚠ Lost on death in danger worlds!                 │
│  • Maximum: 20 shards                                │
└──────────────────────────────────────────────────────┘
```

---

## 📊 Power Scaling

```
Shards    Power Level    Damage    Speed    Status
──────────────────────────────────────────────────────
  0       Powerless      +0%       +0%      🟢 Safe
  5       Initiate       +5%       +5%      🟡 Noticeable
 10       Warrior        +10%      +10%     🟠 Strong
 15       Veteran        +15%      +15%     🔴 Powerful
 20       Master         +20%      +20%     ⚫ Maximum
 20+      Capped         +20%      +20%     ⚫ Can't go higher
```

---

## ⚔️ PvP Encounter Flow

```
SCENARIO: Player A (10 shards) vs Player B (0 shards)

┌─────────────────────────────────────────────────────┐
│                    BEFORE FIGHT                      │
├─────────────────────────────────────────────────────┤
│  Player A:                    Player B:              │
│  • 10 shards                  • 0 shards             │
│  • +10% damage                • +0% damage           │
│  • +10% speed                 • +0% speed            │
│  • High value target          • Nothing to lose     │
└─────────────────────────────────────────────────────┘
                        ↓
              ⚔️  COMBAT  ⚔️
                        ↓
┌─────────────────────────────────────────────────────┐
│              OUTCOME 1: Player A Wins                │
├─────────────────────────────────────────────────────┤
│  Player A:                    Player B:              │
│  • 11-12 shards (gained 1-2)  • Dead                 │
│  • Now +11-12% bonuses        • Lost 0 (had none)    │
│  • Even bigger target         • Respawns with 0      │
└─────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────┐
│              OUTCOME 2: Player B Wins                │
├─────────────────────────────────────────────────────┤
│  Player A:                    Player B:              │
│  • Dead                       • Gains 1-2 shards     │
│  • Dropped 10 shards          • Can pick up A's 10   │
│  • Respawns with 0            • Total: 11-12 shards  │
└─────────────────────────────────────────────────────┘
```

---

## 🔐 Item Security System

```
┌─────────────────────────────────────────────────────┐
│           PREVENTING NATURAL ECHO SHARDS             │
├─────────────────────────────────────────────────────┤
│                                                      │
│  Layer 1: Chest Generation                          │
│  ├─ Check chests on open                            │
│  ├─ Identify natural echo shards                    │
│  └─ Remove automatically                            │
│                                                      │
│  Layer 2: Pickup Prevention                         │
│  ├─ Detect echo shard pickup                        │
│  ├─ Validate if custom Strength Shard               │
│  └─ Cancel if natural, delete item                  │
│                                                      │
│  Layer 3: Placement Blocking                        │
│  ├─ Detect echo shard placement                     │
│  └─ Cancel and notify player                        │
│                                                      │
│  Result: ONLY way to get shards = PvP               │
└─────────────────────────────────────────────────────┘
```

---

## 📈 Stat Application System

```
┌─────────────────────────────────────────────────────┐
│              HOW STATS ARE CALCULATED                │
└─────────────────────────────────────────────────────┘

Every 5 ticks (0.25 seconds):
    │
    ├─► Count shards in player inventory
    │   └─► Include main inventory + off-hand
    │
    ├─► Check if count changed
    │   └─► If same as last check, skip
    │
    ├─► Check if player in danger world
    │   └─► If in safe world, set stats to 0
    │
    ├─► Calculate bonuses
    │   ├─► Damage: (shard_count × damage_per_shard)%
    │   └─► Speed: (shard_count × speed_per_shard)%
    │
    ├─► Cap at maximum
    │   └─► Max effective: 20 shards
    │
    └─► Apply stats
        ├─► Speed: Modify walk speed
        ├─► Damage: Apply on attack event
        └─► Display: Update action bar
```

---

## 💀 Death & Reward System

```
┌─────────────────────────────────────────────────────┐
│              PLAYER DEATH IN DANGER WORLD            │
└─────────────────────────────────────────────────────┘

Player Dies
    │
    ├─► Count victim's shards
    │   └─► Example: 15 shards
    │
    ├─► Calculate drops
    │   └─► Default: 100% (all 15)
    │
    ├─► Remove from inventory
    │   └─► Delete all Strength Shards
    │
    ├─► Drop at death location
    │   └─► Spawn 15 Strength Shards on ground
    │
    ├─► Notify victim
    │   └─► "You lost 15 Strength Shard(s)!"
    │
    └─► If killed by player:
        │
        ├─► Generate reward (1-2 shards)
        │
        ├─► Give to killer
        │   └─► Add to inventory or drop if full
        │
        └─► Notify killer
            └─► "You gained 2 Strength Shard(s)!"
```

---

## 🎮 Player Decision Tree

```
┌─────────────────────────────────────────────────────┐
│            PLAYER STRATEGIC DECISIONS                │
└─────────────────────────────────────────────────────┘

You have 15 shards. What do you do?

    Option A: Keep Fighting
        ├─► Pro: Gain more shards
        ├─► Pro: Become very powerful
        ├─► Con: Big target
        └─► Con: Lose everything if you die

    Option B: Store in Safe World
        ├─► Pro: Shards are safe
        ├─► Pro: Can't lose them
        ├─► Con: No stat bonuses in safe world
        └─► Con: No progression

    Option C: Moderate Approach
        ├─► Pro: Some power, moderate risk
        ├─► Keep 5-10 shards
        ├─► Store rest in safe world
        └─► Balance risk/reward

    Option D: Go All-In
        ├─► Collect maximum (20 shards)
        ├─► Maximum power (+20%)
        ├─► Huge target
        └─► Epic fights

Result: EMERGENT GAMEPLAY!
```

---

## 🔄 Information Flow

```
┌─────────────────────────────────────────────────────┐
│              SYSTEM INFORMATION FLOW                 │
└─────────────────────────────────────────────────────┘

Player Actions → Events → Script Response → Visual Feedback
     │              │           │                │
     │              │           │                └─► Action Bar
     │              │           │                └─► Chat Messages
     │              │           │                └─► Visual Effects
     │              │           │
     │              │           └─► Update Stats
     │              │           └─► Apply Bonuses
     │              │           └─► Check Conditions
     │              │
     │              └─► Inventory Click
     │              └─► Player Death
     │              └─► World Change
     │              └─► Item Pickup
     │
     └─► Hold Shards
     └─► Kill Player
     └─► Die
     └─► Change Worlds
```

---

## 🎯 Risk/Reward Matrix

```
┌─────────────────────────────────────────────────────┐
│              RISK VS REWARD ANALYSIS                 │
└─────────────────────────────────────────────────────┘

                    HIGH REWARD
                        ▲
                        │
                        │
     [HIGH SHARDS]      │      [LOW SHARDS]
     • 15-20 shards     │      • 1-5 shards
     • +15-20% power    │      • +1-5% power
     • HUGE target      │      • Small risk
     • Big loss         │      • Small loss
                        │
    ────────────────────┼────────────────────►
    HIGH RISK           │           LOW RISK
                        │
     [DANGER WORLD]     │      [SAFE WORLD]
     • Stats active     │      • No stats
     • Can gain         │      • No gains
     • Can lose         │      • No losses
                        │
                    LOW REWARD
```

---

## 🏆 Progression Timeline

```
┌─────────────────────────────────────────────────────┐
│           TYPICAL PLAYER PROGRESSION                 │
└─────────────────────────────────────────────────────┘

Day 1:
├─► Enter danger world
├─► Get first kill
├─► 1-2 shards
└─► "I'm slightly stronger!"

Day 2-3:
├─► Multiple kills
├─► 5-10 shards
├─► Noticeable power
└─► "I can win most fights"

Day 4-5:
├─► Frequent PvP
├─► 10-15 shards
├─► Strong player
└─► "People are hunting me"

Day 6-7:
├─► High-stakes combat
├─► 15-20 shards
├─► Maximum power
└─► "I'm a walking target"

Eventually:
├─► Die to skilled player
├─► Lose all shards
├─► Return to Day 1
└─► Cycle repeats

Result: Natural progression and regression loop!
```

---

## 🎨 Visual Representation

```
┌─────────────────────────────────────────────────────┐
│              WHAT PLAYERS SEE                        │
└─────────────────────────────────────────────────────┘

In Danger World with 10 shards:

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
                   [PLAYER VIEW]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Inventory:
┌─┬─┬─┬─┬─┬─┬─┬─┬─┐
│💎│💎│💎│💎│💎│💎│💎│💎│💎│  ← 10 Strength Shards
└─┴─┴─┴─┴─┴─┴─┴─┴─┘
│⚔│🛡│🥾│🍖│ │ │ │ │ │
└─┴─┴─┴─┴─┴─┴─┴─┴─┘

Action Bar:
[⚔] Shards: 10 | Damage: +10% | Speed: +10%

Chat:
[⚔] You entered a danger world! Your Strength Shards are now active.

Movement:
Running speed: FASTER than normal

Combat:
Attack damage: HIGHER than normal

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## 🔧 Configuration Impact Visualization

```
┌─────────────────────────────────────────────────────┐
│          CONFIGURATION → GAMEPLAY EFFECT             │
└─────────────────────────────────────────────────────┘

damage-per-shard: 1
    ↓
10 shards = +10% damage
    ↓
5 damage → 5.5 damage
    ↓
Zombie dies in 4 hits instead of 5

─────────────────────────────────────────────────

speed-per-shard: 1
    ↓
10 shards = +10% speed
    ↓
Base 0.2 → 0.22 walk speed
    ↓
Player moves visibly faster

─────────────────────────────────────────────────

max-effective-shards: 20
    ↓
30 shards in inventory
    ↓
Only 20 count
    ↓
+20% bonuses (capped)
```

---

## 📊 Server Architecture

```
┌─────────────────────────────────────────────────────┐
│              PLUGIN STRUCTURE                        │
└─────────────────────────────────────────────────────┘

Spigot/Paper Server
    │
    ├─► Skript 2.15.3
    │   └─► Core scripting engine
    │
    ├─► SkBee 3.25
    │   └─► Extended functionality
    │
    └─► Strength Shard System
        │
        ├─► config.sk
        │   └─► All configuration options
        │
        └─► main.sk
            ├─► Functions
            │   ├─► createStrengthShard()
            │   ├─► isStrengthShard()
            │   ├─► isDangerWorld()
            │   ├─► countShardsInInventory()
            │   └─► updatePlayerStats()
            │
            ├─► Events
            │   ├─► on damage
            │   ├─► on death
            │   ├─► on join/quit
            │   ├─► on inventory change
            │   ├─► on world change
            │   └─► periodic updates
            │
            └─► Commands
                └─► /ss (admin commands)
```

---

**This visual guide provides a complete overview of the system's operation! 📊**
