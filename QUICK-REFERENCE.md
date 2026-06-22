# Quick Reference Guide

A one-page reference for server admins managing the Strength Shard System.

---

## 📋 Essential Commands

| Command | Description | Example |
|---------|-------------|---------|
| `/ss` | Show help menu | `/ss` |
| `/ss give <player> [amount]` | Give shards | `/ss give Steve 10` |
| `/ss remove <player> [amount]` | Remove shards | `/ss remove Alex 5` |
| `/ss check <player>` | Check shard count | `/ss check Notch` |
| `/ss reload` | Reload instructions | `/ss reload` |
| `/sk reload strength-shard-system` | Actually reload script | `/sk reload strength-shard-system` |

---

## ⚙️ Key Configuration Values

### Most Common Settings (config.sk)

```yaml
# Worlds
danger-worlds: world_danger|world_nether|world_pvp

# Stats
damage-per-shard: 1          # +1% per shard
speed-per-shard: 1           # +1% per shard
max-effective-shards: 20     # Cap at 20

# Rewards
min-shards-on-kill: 1        # Min per kill
max-shards-on-kill: 2        # Max per kill

# Death
drop-all-shards-on-death: true    # Drop everything?
percent-shards-dropped: 100       # If false, what %?
```

---

## 🎯 Stat Calculation Reference

### Default Formula

```
Damage Bonus = (Shard Count × 1%)
Speed Bonus = (Shard Count × 1%)
Max = 20 shards = +20% damage, +20% speed
```

### Quick Table

| Shards | Damage | Speed | Power Level |
|--------|--------|-------|-------------|
| 0 | +0% | +0% | Weak |
| 5 | +5% | +5% | Below Average |
| 10 | +10% | +10% | Average |
| 15 | +15% | +15% | Above Average |
| 20 | +20% | +20% | Maximum |

---

## 🚨 Troubleshooting Quick Fixes

### Script Won't Load

```bash
# Check versions
/sk info
/skbee version

# Try full reload
/sk reload all

# Check console for errors
```

### Shards Not Working

```bash
# 1. Check world name
/ss check PlayerName

# 2. Verify in danger world
/skript eval send "%world of player%" to player

# 3. Reload script
/sk reload strength-shard-system
```

### Natural Echo Shards Appearing

```yaml
# Ensure this is set to true in config.sk:
prevent-natural-echo-shards: true
```

---

## 🔐 Permission

**Single permission node:**
```
strengthshard.admin
```

**Grant with LuckPerms:**
```
/lp group admin permission set strengthshard.admin true
```

---

## 📊 Balance Presets

### Conservative (Low Power)
```yaml
damage-per-shard: 0.5
speed-per-shard: 0.5
max-effective-shards: 20
min-shards-on-kill: 1
max-shards-on-kill: 1
```
**Result:** Max +10% damage/speed, 1 shard per kill

### Balanced (Default)
```yaml
damage-per-shard: 1
speed-per-shard: 1
max-effective-shards: 20
min-shards-on-kill: 1
max-shards-on-kill: 2
```
**Result:** Max +20% damage/speed, 1-2 shards per kill

### Aggressive (High Power)
```yaml
damage-per-shard: 2
speed-per-shard: 2
max-effective-shards: 25
min-shards-on-kill: 2
max-shards-on-kill: 5
```
**Result:** Max +50% damage/speed, 2-5 shards per kill

### Hardcore (Extreme)
```yaml
damage-per-shard: 3
speed-per-shard: 1.5
max-effective-shards: 30
min-shards-on-kill: 3
max-shards-on-kill: 10
drop-all-shards-on-death: true
percent-shards-dropped: 100
```
**Result:** Max +90% damage, +45% speed, 3-10 shards per kill

---

## 🌍 World Setup Checklist

**For each danger world:**
- [ ] Add world name to `danger-worlds` in config.sk
- [ ] Test PvP is enabled
- [ ] Test shards provide bonuses
- [ ] Test shards drop on death
- [ ] Test natural echo shards are removed

**For each safe world:**
- [ ] Verify world NOT in `danger-worlds`
- [ ] Test shards provide NO bonuses
- [ ] Test shards do NOT drop on death

---

## 🐛 Debug Mode

**Enable debug logging:**
```yaml
# In config.sk
debug-mode: true
```

**What it logs:**
- Stat updates
- Shard counts
- Damage calculations
- Natural echo shard removals
- PvP kill rewards

**Check console after enabling and testing**

---

## 📈 Monitoring Tips

### Daily Checks
- Check for exploits (players farming shards)
- Monitor top shard holders
- Verify balance (no one too powerful)

### Weekly Reviews
- Adjust reward amounts if needed
- Review death penalty severity
- Check player feedback

### Monthly Balance Updates
- Analyze average shard counts
- Adjust max cap if needed
- Fine-tune stat bonuses

---

## 🎮 Player FAQ Answers

**Q: "How do I get shards?"**
A: Kill players in danger worlds

**Q: "Do shards work in chests?"**
A: No, only in your inventory

**Q: "What happens if I die?"**
A: You drop all shards in danger worlds

**Q: "What's the maximum?"**
A: 20 shards = +20% damage and speed

**Q: "Can I trade shards?"**
A: Yes, they're regular items

**Q: "Do shards work everywhere?"**
A: No, only in danger worlds

---

## 🔧 Emergency Commands

### Disable System Temporarily
```bash
/sk disable strength-shard-system
```

### Re-enable
```bash
/sk enable strength-shard-system
```

### Full Reset (removes all data)
```bash
/sk reload strength-shard-system
# All player data is cleared on reload
```

---

## 📝 Quick Config Edit

**Via FTP/SFTP:**
1. Navigate to `plugins/Skript/scripts/strength-shard-system/`
2. Download `config.sk`
3. Edit with text editor
4. Upload back
5. Run `/sk reload strength-shard-system`

**Via Console/SSH:**
```bash
nano plugins/Skript/scripts/strength-shard-system/config.sk
# Edit and save (Ctrl+O, Ctrl+X)
# Then reload in-game
```

---

## 🎯 Testing Procedure

### After ANY config change:

1. **Reload script:**
   ```
   /sk reload strength-shard-system
   ```

2. **Give test shards:**
   ```
   /ss give YourName 10
   ```

3. **Go to danger world:**
   ```
   /tp @s ~ ~ ~ world_danger
   ```

4. **Verify:**
   - Action bar shows stats
   - Movement speed increased
   - Attack damage increased

5. **Test death:**
   - Kill yourself or have someone kill you
   - Verify shards dropped
   - Check killer received reward

---

## 📱 Player Announcement Template

```
┌─────────────────────────────────┐
│   🗡️ NEW: STRENGTH SHARD SYSTEM   │
└─────────────────────────────────┘

⚔️ OBTAIN POWER BY KILLING PLAYERS
📊 Each shard = +1% damage & speed
💎 Maximum: 20 shards (+20% boost)
💀 Die in danger worlds = lose shards
🌍 Only works in: [your danger worlds]
🛡️ Safe in: [your safe worlds]

⚠️ HIGH RISK, HIGH REWARD! ⚠️
```

---

## 🔗 Useful Links

- **Skript Docs**: https://docs.skriptlang.org/
- **SkBee Wiki**: https://github.com/ShaneBeee/SkBee/wiki
- **SkUnity Forums**: https://forums.skunity.com/
- **Skript Discord**: https://discord.gg/skript

---

## 💾 Backup Reminder

**Before making ANY changes:**
```bash
# Backup entire scripts folder
cp -r plugins/Skript/scripts/ plugins/Skript/scripts-backup/

# Or just backup this system
cp -r plugins/Skript/scripts/strength-shard-system/ \
     plugins/Skript/scripts/strength-shard-system-backup/
```

---

## 📊 Performance Metrics

**Recommended server specs:**
- **Small server (1-20 players)**: 2GB RAM minimum
- **Medium server (20-50 players)**: 4GB RAM minimum
- **Large server (50+ players)**: 6GB+ RAM

**System impact:**
- CPU usage: <0.5% per 20 players
- RAM usage: <10MB total
- TPS impact: <0.1 with default settings

**Optimization:**
- Increase `update-interval` if experiencing lag
- Disable `debug-mode` in production
- Consider caching for 100+ players

---

## ✅ Launch Checklist

Before releasing to players:

- [ ] Script loads without errors
- [ ] Danger worlds configured correctly
- [ ] Stats apply in danger worlds
- [ ] Stats DON'T apply in safe worlds
- [ ] PvP kills grant shards
- [ ] Deaths drop shards
- [ ] Natural echo shards prevented
- [ ] Admin commands work
- [ ] Permissions set up
- [ ] Tested with multiple players
- [ ] Server performance stable
- [ ] Player announcement ready
- [ ] Tutorial/rules written

---

**Keep this page bookmarked for quick reference! 📌**
