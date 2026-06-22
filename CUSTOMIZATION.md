# Advanced Customization Guide

This guide covers advanced modifications and customizations for the Strength Shard System. These changes require editing the script files directly.

---

## Table of Contents

1. [Custom Stat Formulas](#1-custom-stat-formulas)
2. [Per-World Configurations](#2-per-world-configurations)
3. [Alternative Progression Systems](#3-alternative-progression-systems)
4. [Custom Visual Effects](#4-custom-visual-effects)
5. [Integration with Other Plugins](#5-integration-with-other-plugins)
6. [Database Storage](#6-database-storage)
7. [Leaderboards](#7-leaderboards)
8. [Custom Rewards](#8-custom-rewards)
9. [Time-Based Mechanics](#9-time-based-mechanics)
10. [Advanced Security](#10-advanced-security)

---

## 1. Custom Stat Formulas

### Exponential Growth

Instead of linear growth (+1% per shard), use exponential scaling:

**Edit `main.sk`, find the `updatePlayerStats` function:**

```skript
function updatePlayerStats(player: player):
    # ... existing code ...
    
    # REPLACE THIS:
    # set {_damage-bonus} to {_count} * {@damage-per-shard}
    # set {_speed-bonus} to {_count} * {@speed-per-shard}
    
    # WITH THIS (exponential):
    set {_damage-bonus} to ({_count} ^ 1.2) * 0.8
    set {_speed-bonus} to ({_count} ^ 1.2) * 0.8
    
    # Results:
    # 1 shard = +0.8%
    # 5 shards = +4.3%
    # 10 shards = +12.7%
    # 20 shards = +33.1%
```

### Diminishing Returns

Early shards are powerful, later shards give less:

```skript
function updatePlayerStats(player: player):
    # ... existing code ...
    
    # Calculate with diminishing returns
    set {_damage-bonus} to 100 * (1 - (0.95 ^ {_count}))
    set {_speed-bonus} to 100 * (1 - (0.95 ^ {_count}))
    
    # Results:
    # 1 shard = +5%
    # 5 shards = +22.6%
    # 10 shards = +40.1%
    # 20 shards = +64.2%
```

### Threshold Bonuses

Bonus "jumps" at certain thresholds:

```skript
function updatePlayerStats(player: player):
    # ... existing code ...
    
    # Base linear bonus
    set {_damage-bonus} to {_count} * {@damage-per-shard}
    set {_speed-bonus} to {_count} * {@speed-per-shard}
    
    # Add threshold bonuses
    if {_count} >= 5:
        add 5 to {_damage-bonus}
    if {_count} >= 10:
        add 10 to {_damage-bonus}
    if {_count} >= 15:
        add 15 to {_damage-bonus}
    if {_count} >= 20:
        add 25 to {_damage-bonus}
    
    # Results:
    # 4 shards = +4%
    # 5 shards = +10% (threshold bonus!)
    # 10 shards = +20%
    # 20 shards = +75%
```

---

## 2. Per-World Configurations

### Different Stats per World

**Add to `main.sk`:**

```skript
function getWorldMultiplier(world: world) :: number:
    if name of {_world} is "world_nether":
        return 1.5  # 50% more powerful in nether
    else if name of {_world} is "world_end":
        return 2.0  # 2x more powerful in end
    else:
        return 1.0  # Normal in other worlds

function updatePlayerStats(player: player):
    # ... existing code ...
    
    # Calculate base bonuses
    set {_damage-bonus} to {_count} * {@damage-per-shard}
    set {_speed-bonus} to {_count} * {@speed-per-shard}
    
    # Apply world multiplier
    set {_multiplier} to getWorldMultiplier(world of {_player})
    set {_damage-bonus} to {_damage-bonus} * {_multiplier}
    set {_speed-bonus} to {_speed-bonus} * {_multiplier}
```

### Different Drop Rates per World

**Add to `on death of player` event:**

```skript
on death of player:
    if isDangerWorld(world of victim) is true:
        # Calculate drops based on world
        if name of world of victim is "world_nether":
            set {_drop-percent} to 50  # Only drop 50% in nether
        else if name of world of victim is "world_end":
            set {_drop-percent} to 100  # Drop all in end
        else:
            set {_drop-percent} to {@percent-shards-dropped}
        
        set {_drop-amount} to round({_victim-shards} * {_drop-percent} / 100)
        # ... rest of code ...
```

---

## 3. Alternative Progression Systems

### Class-Based System

Different benefits based on "class":

**Add to `config.sk`:**

```yaml
# Player classes (set with /ss setclass <player> <class>)
warrior-damage-bonus: 2
warrior-speed-bonus: 0.5

archer-damage-bonus: 1.5
archer-speed-bonus: 1.5

tank-damage-bonus: 0.5
tank-speed-bonus: 0.5
```

**Add to `main.sk`:**

```skript
function updatePlayerStats(player: player):
    # Get player class
    set {_class} to {player-class::%{_player}%} ? "default"
    
    # Calculate bonuses based on class
    if {_class} is "warrior":
        set {_damage-bonus} to {_count} * 2
        set {_speed-bonus} to {_count} * 0.5
    else if {_class} is "archer":
        set {_damage-bonus} to {_count} * 1.5
        set {_speed-bonus} to {_count} * 1.5
    else if {_class} is "tank":
        set {_damage-bonus} to {_count} * 0.5
        set {_speed-bonus} to {_count} * 0.5
    else:
        set {_damage-bonus} to {_count} * {@damage-per-shard}
        set {_speed-bonus} to {_count} * {@speed-per-shard}

# Add command to set class
command /ss setclass <player> <text>:
    permission: strengthshard.admin
    trigger:
        if arg-2 is "warrior" or "archer" or "tank":
            set {player-class::%arg-1%} to arg-2
            send "&aSet %arg-1%'s class to %arg-2%!" to sender
        else:
            send "&cInvalid class! Use: warrior, archer, or tank" to sender
```

### Prestige System

Reset shards for permanent bonuses:

**Add to `main.sk`:**

```skript
# Prestige variables
# {prestige::%player%} = prestige level
# {prestige-bonus::%player%} = permanent bonus

command /ss prestige:
    trigger:
        if countShardsInInventory(player) >= 100:
            # Remove 100 shards
            # (implementation left as exercise)
            
            # Increase prestige
            add 1 to {prestige::%player%}
            add 5 to {prestige-bonus::%player%}  # +5% permanent bonus
            
            send "&6⭐ PRESTIGE LEVEL %{prestige::%player%}% ⭐" to player
            send "&eYou gained +5%% permanent bonus!" to player
        else:
            send "&cYou need 100 shards to prestige!" to player

function updatePlayerStats(player: player):
    # ... existing code ...
    
    # Add prestige bonus
    if {prestige-bonus::%{_player}%} is set:
        add {prestige-bonus::%{_player}%} to {_damage-bonus}
        add {prestige-bonus::%{_player}%} to {_speed-bonus}
```

---

## 4. Custom Visual Effects

### Particle Effects

Add particles based on shard count:

**Add to `main.sk`:**

```skript
every 1 second:
    loop all players:
        if isDangerWorld(world of loop-player) is true:
            set {_count} to {shard-count::%loop-player%}
            
            if {_count} >= 20:
                # Maximum power - purple particles
                show 3 of end_rod particle at loop-player offset by 0.5, 1, 0.5
            else if {_count} >= 10:
                # High power - red particles
                show 2 of flame particle at loop-player offset by 0.5, 1, 0.5
            else if {_count} >= 5:
                # Medium power - orange particles
                show 1 of soul particle at loop-player offset by 0.5, 1, 0.5
```

### Glowing Effect

Make powerful players glow:

```skript
every 5 seconds:
    loop all players:
        if isDangerWorld(world of loop-player) is true:
            set {_count} to {shard-count::%loop-player%}
            
            if {_count} >= 15:
                apply glowing to loop-player for 6 seconds
```

### Title Messages

Show dramatic titles on kills:

```skript
on death of player:
    # ... existing code ...
    
    if attacker is a player:
        if {_reward} >= 5:
            send title "&6MEGA KILL!" with subtitle "&e+%{_reward}% Shards!" to attacker for 2 seconds
        else:
            send title "&aKill!" with subtitle "&e+%{_reward}% Shards" to attacker for 1 second
```

---

## 5. Integration with Other Plugins

### PlaceholderAPI Support

Create placeholders for other plugins:

**Install PlaceholderAPI, then add:**

```skript
# Requires: Skript-PlaceholderAPI addon

placeholder shards of %player%:
    result: countShardsInInventory(arg-player)

placeholder shards_damage of %player%:
    set {_count} to countShardsInInventory(arg-player)
    result: {_count} * {@damage-per-shard}

placeholder shards_speed of %player%:
    set {_count} to countShardsInInventory(arg-player)
    result: {_count} * {@speed-per-shard}

placeholder shards_rank of %player%:
    # Shows player rank by shard count
    set {_count} to countShardsInInventory(arg-player)
    if {_count} >= 20:
        result: "Master"
    else if {_count} >= 15:
        result: "Expert"
    else if {_count} >= 10:
        result: "Veteran"
    else if {_count} >= 5:
        result: "Initiate"
    else:
        result: "Novice"
```

**Use in other plugins:**
- Chat: `%shards_shards of player%`
- Tab: `[%shards_rank of player%] %player%`

### Vault Economy Integration

Reward money for kills:

```skript
# Requires: Vault plugin

on death of player:
    if isDangerWorld(world of victim) is true:
        if attacker is a player:
            # ... existing shard code ...
            
            # Also give money
            set {_money} to {_reward} * 100
            add {_money} to attacker's money
            send "&7You earned &a$%{_money}%" to attacker
```

### WorldGuard Region Protection

Disable shards in specific regions:

```skript
# Requires: sk-worldguard addon

function isInSafeRegion(player: player) :: boolean:
    # Check if player is in a "safezone" region
    if player is in region "safezone":
        return true
    return false

function updatePlayerStats(player: player):
    # Check for safe region
    if isInSafeRegion({_player}) is true:
        set {_player}'s walk speed to 0.2
        stop
    
    # ... rest of normal code ...
```

---

## 6. Database Storage

### MySQL Storage

Store shard counts in database for persistence:

**Requires: Skript-DB addon**

```skript
# On script load, create table
on script load:
    execute "CREATE TABLE IF NOT EXISTS strength_shards (uuid VARCHAR(36) PRIMARY KEY, shards INT, last_updated TIMESTAMP)" in {sql}

# Save on quit
on quit:
    set {_uuid} to uuid of player
    set {_shards} to countShardsInInventory(player)
    execute "INSERT INTO strength_shards (uuid, shards, last_updated) VALUES ('%{_uuid}%', %{_shards}%, NOW()) ON DUPLICATE KEY UPDATE shards=%{_shards}%, last_updated=NOW()" in {sql}

# Load on join
on join:
    set {_uuid} to uuid of player
    execute "SELECT shards FROM strength_shards WHERE uuid='%{_uuid}%'" in {sql} and store result in {_result}
    if {_result} is set:
        # Restore shards
        # (implementation depends on your needs)
```

---

## 7. Leaderboards

### Top Shards Leaderboard

**Add to `main.sk`:**

```skript
command /ss top [<integer>]:
    trigger:
        set {_limit} to arg-1 ? 10
        send "&8[&d⚔&8] &dTop %{_limit}% Shard Holders"
        send "&7━━━━━━━━━━━━━━━━━━━━━━━"
        
        # Create temporary list
        delete {_leaderboard::*}
        loop all players:
            set {_count} to countShardsInInventory(loop-player)
            if {_count} is greater than 0:
                set {_leaderboard::%loop-player%} to {_count}
        
        # Sort and display
        set {_rank} to 0
        loop {_limit} times:
            set {_highest} to 0
            loop {_leaderboard::*}:
                if loop-value is greater than {_highest}:
                    set {_highest} to loop-value
                    set {_top-player} to loop-index
            
            if {_highest} is 0:
                stop
            
            add 1 to {_rank}
            send "&e%{_rank}%. &f%{_top-player}% &7- &e%{_highest}% shards" to player
            delete {_leaderboard::%{_top-player}%}

### Kill Streak Tracking

```skript
# Variables: {kill-streak::%player%}

on death of player:
    if attacker is a player:
        # Increase attacker's streak
        add 1 to {kill-streak::%attacker%}
        
        # Broadcast if high streak
        if {kill-streak::%attacker%} is 5:
            broadcast "&e%attacker% is on a 5 kill streak!"
        else if {kill-streak::%attacker%} is 10:
            broadcast "&6%attacker% is on a 10 kill streak! &c⚔"
        
        # Reset victim's streak
        if {kill-streak::%victim%} >= 5:
            broadcast "&c%victim%'s %{kill-streak::%victim%}% kill streak was ended by %attacker%!"
        delete {kill-streak::%victim%}
```

---

## 8. Custom Rewards

### Bonus Loot on High Kills

```skript
on death of player:
    if attacker is a player:
        # ... existing code ...
        
        # Bonus rewards for high-shard victims
        if {_victim-shards} >= 20:
            give diamond sword of sharpness 5 to attacker
            send "&6⭐ BONUS: Diamond Sword for killing a max-power player!" to attacker
        else if {_victim-shards} >= 10:
            give 5 golden apples to attacker
            send "&eBonus: 5 Golden Apples!" to attacker
```

### Random Events

```skript
every 30 minutes:
    if a random number between 1 and 100 <= 30:  # 30% chance
        broadcast "&6⚡ DOUBLE SHARD EVENT ACTIVE FOR 10 MINUTES! ⚡"
        set {double-shard-event} to true
        wait 10 minutes
        set {double-shard-event} to false
        broadcast "&6⚡ Double Shard Event ENDED ⚡"

on death of player:
    if attacker is a player:
        # ... calculate reward ...
        
        if {double-shard-event} is true:
            set {_reward} to {_reward} * 2
            send "&6⚡ DOUBLED!" to attacker
```

---

## 9. Time-Based Mechanics

### Shard Decay

Shards decay over time if not used:

```skript
every 1 hour:
    loop all players:
        set {_count} to countShardsInInventory(loop-player)
        if {_count} > 0:
            # Remove 5% of shards (rounded down)
            set {_decay} to round({_count} * 0.05)
            if {_decay} > 0:
                # Remove shards from inventory
                # (implementation as exercise)
                send "&cYour shards decayed by %{_decay}%!" to loop-player
```

### Peak Hours Bonus

```skript
function isPeakHours() :: boolean:
    set {_hour} to hour of now
    if {_hour} >= 18 and {_hour} <= 23:  # 6 PM to 11 PM
        return true
    return false

on death of player:
    if attacker is a player:
        # ... calculate reward ...
        
        if isPeakHours() is true:
            add 1 to {_reward}
            send "&ePeak Hours Bonus: +1 shard!" to attacker
```

---

## 10. Advanced Security

### Anti-Farming Protection

Prevent players from repeatedly killing the same person:

```skript
on death of player:
    if attacker is a player:
        # Check recent kills
        if {recent-kills::%attacker%::%victim%} is set:
            if difference between now and {recent-kills::%attacker%::%victim%} < 5 minutes:
                send "&cYou recently killed %victim%! No reward." to attacker
                stop
        
        # ... normal reward code ...
        
        # Record kill
        set {recent-kills::%attacker%::%victim%} to now
```

### Logging System

Track all shard transactions:

```skript
function logShardEvent(player: player, event: text, amount: number):
    set {_timestamp} to now
    set {_log} to "[%{_timestamp}%] %{_player}%: %{_event}% (%{_amount}% shards)"
    send {_log} to console
    
    # Optional: save to file
    # write {_log} to file "plugins/Skript/logs/shards.log" with append mode

# Use in events
on death of player:
    if attacker is a player:
        logShardEvent(attacker, "KILL_REWARD", {_reward})
        logShardEvent(victim, "DEATH_LOSS", {_drop-amount})
```

---

## Tips for Custom Modifications

1. **Always test on a test server first**
2. **Back up your scripts before making changes**
3. **Use debug mode to troubleshoot**
4. **Comment your custom code**
5. **Keep performance in mind (avoid loops)**
6. **Check Skript documentation for syntax**
7. **Use functions for reusable code**

---

## Performance Optimization

### Reduce Update Frequency

```yaml
# In config.sk
update-interval: 20  # From 5 to 20 (1x per second instead of 4x)
```

### Cache Calculations

```skript
function updatePlayerStats(player: player):
    # Only recalculate if needed
    set {_count} to countShardsInInventory({_player})
    
    if {last-shard-count::%{_player}%} is {_count}:
        stop  # No change, skip calculation
    
    # ... rest of code ...
```

---

## Support for Custom Modifications

Custom modifications are **not officially supported**, but:

1. Check Skript documentation: https://docs.skriptlang.org/
2. Check SkBee wiki: https://github.com/ShaneBeee/SkBee/wiki
3. Ask on SkUnity forums: https://forums.skunity.com/
4. Enable debug mode for troubleshooting

---

**Happy customizing! ⚔️**
