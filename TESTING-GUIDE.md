# Testing & Verification Guide

Complete testing procedures to ensure the Strength Shard System works correctly on your server.

---

## 🧪 Pre-Launch Testing Checklist

Use this checklist before releasing to players:

### Phase 1: Installation Verification

- [ ] **Skript 2.15.3+ installed**
  - Run `/sk info` and verify version
  
- [ ] **SkBee 3.25+ installed**
  - Run `/skbee version` or check console

- [ ] **Script loads successfully**
  - Console shows: `[⚔] Strength Shard System v1.0.0 LOADED`
  - No error messages in console

- [ ] **Configuration validated**
  - Danger worlds set correctly
  - All syntax correct (no missing colons/quotes)

---

### Phase 2: Basic Functionality Tests

#### Test 1: Item Creation
```
/ss give @s 1
```
**Expected:**
- ✅ Receive 1 Echo Shard
- ✅ Name: "Strength Shard" (purple/pink)
- ✅ Has custom lore
- ✅ Item flags hidden

**Pass/Fail:** _______

---

#### Test 2: Shard Counting
```
/ss give @s 10
/ss check @s
```
**Expected:**
- ✅ Message shows: "You have 10 Strength Shard(s)"
- ✅ Count matches inventory

**Pass/Fail:** _______

---

#### Test 3: World Detection
**Safe World Test:**
```
1. /tp @s ~ ~ ~ world (or your safe world)
2. Hold shards
3. Check action bar
```
**Expected:**
- ✅ Message: "You entered a safe world..."
- ✅ NO action bar stats
- ✅ NO speed boost

**Pass/Fail:** _______

**Danger World Test:**
```
1. /tp @s ~ ~ ~ world_danger (your danger world)
2. Hold shards
3. Check action bar
```
**Expected:**
- ✅ Message: "You entered a danger world..."
- ✅ Action bar shows: "Shards: X | Damage: +X% | Speed: +X%"
- ✅ Noticeable speed boost

**Pass/Fail:** _______

---

#### Test 4: Stat Application (Danger World)
**Movement Speed:**
```
1. Go to danger world
2. Drop all shards
3. Walk forward and measure time
4. Pick up 10 shards
5. Walk same distance
```
**Expected:**
- ✅ Movement noticeably faster with shards
- ✅ Action bar shows "+10% speed"

**Pass/Fail:** _______

**Damage Boost:**
```
1. In danger world with 0 shards
2. Hit a zombie, note damage
3. Get 10 shards
4. Hit same type of zombie
```
**Expected:**
- ✅ Damage increased by ~10%
- ✅ Example: 5 damage → 5.5 damage

**Pass/Fail:** _______

---

### Phase 3: PvP System Tests

#### Test 5: Death & Drops
**Setup:** Two players, both in danger world

**Test:**
```
1. Give Player A 10 shards
2. Player B kills Player A
3. Observe drops
```

**Expected:**
- ✅ Player A drops 10 shards on ground
- ✅ Player A sees: "You lost 10 Strength Shard(s)!"
- ✅ Player A respawns with 0 shards

**Pass/Fail:** _______

---

#### Test 6: Kill Rewards
**Setup:** Two players in danger world

**Test:**
```
1. Player A has 5 shards
2. Player B kills Player A
3. Check Player B's inventory
```

**Expected:**
- ✅ Player B receives 1-2 NEW shards (from system)
- ✅ Player B sees: "You gained X Strength Shard(s)!"
- ✅ Player A's 5 shards are on ground (if killer_gets_victim_shards: true)

**Pass/Fail:** _______

---

#### Test 7: Damage in PvP
**Setup:** Two players in danger world

**Test:**
```
1. Player A has 0 shards
2. Player B has 10 shards
3. They hit each other
4. Compare damage
```

**Expected:**
- ✅ Player B deals ~10% more damage than Player A
- ✅ Noticeable difference in PvP

**Pass/Fail:** _______

---

### Phase 4: Security Tests

#### Test 8: Natural Echo Shard Prevention
**Test A: Chest Loot**
```
1. Enable debug mode
2. Find/generate ancient city chest
3. Open chest
```

**Expected:**
- ✅ No natural echo shards in chest
- ✅ Debug message if any removed

**Pass/Fail:** _______

**Test B: Pickup Prevention**
```
1. /give @s echo_shard (vanilla, not custom)
2. Drop it
3. Try to pick up
```

**Expected:**
- ✅ Cannot pick up
- ✅ Message: "Strength Shards can only be obtained through PvP..."
- ✅ Item disappears

**Pass/Fail:** _______

---

#### Test 9: Shard Placement
```
1. Get Strength Shard
2. Try to place it as a block
```

**Expected:**
- ✅ Placement cancelled
- ✅ Message: "You cannot place Strength Shards!"

**Pass/Fail:** _______

---

### Phase 5: Edge Cases

#### Test 10: Maximum Cap
```
1. /ss give @s 50
2. Check action bar
```

**Expected:**
- ✅ Stats cap at 20 shards
- ✅ Action bar shows: "Shards: 20" (not 50)
- ✅ Bonuses: +20% damage, +20% speed (not +50%)

**Pass/Fail:** _______

---

#### Test 11: Inventory Changes
**Test:** Move shards in inventory

```
1. Have 10 shards in inventory
2. Move shards between slots
3. Drop and pickup
4. Place in chest and remove
```

**Expected:**
- ✅ Stats update correctly after each change
- ✅ Action bar updates
- ✅ NO stats when shards in chest

**Pass/Fail:** _______

---

#### Test 12: World Switching with Shards
```
1. In danger world with 10 shards
2. /tp to safe world
3. /tp back to danger world
```

**Expected:**
- ✅ Stats disabled in safe world
- ✅ Stats re-enabled in danger world
- ✅ Messages appear on world change

**Pass/Fail:** _______

---

#### Test 13: Logout/Login
```
1. Get 10 shards
2. Check stats (should be +10%)
3. Logout
4. Login
5. Check stats again
```

**Expected:**
- ✅ Stats recalculate on login
- ✅ Shards still in inventory
- ✅ Bonuses apply correctly

**Pass/Fail:** _______

---

### Phase 6: Admin Commands

#### Test 14: Give Command
```
/ss give PlayerName 15
```

**Expected:**
- ✅ Target receives 15 shards
- ✅ Sender sees success message
- ✅ Target sees gain message

**Pass/Fail:** _______

---

#### Test 15: Remove Command
```
/ss remove PlayerName 5
```

**Expected:**
- ✅ Target loses 5 shards
- ✅ Sender sees confirmation
- ✅ Shards removed from inventory

**Pass/Fail:** _______

---

#### Test 16: Check Command
```
/ss check PlayerName
```

**Expected:**
- ✅ Shows accurate shard count
- ✅ Works for online players
- ✅ Shows cap (e.g., "capped at 20")

**Pass/Fail:** _______

---

### Phase 7: Multi-Player Tests

#### Test 17: Multiple Players in Danger World
**Setup:** 5+ players

```
1. All players get different shard amounts
2. All enter danger world simultaneously
3. Check each player's stats
```

**Expected:**
- ✅ Each player has correct individual bonuses
- ✅ No cross-contamination of stats
- ✅ Server performance stable

**Pass/Fail:** _______

---

#### Test 18: Mass PvP
**Setup:** 10+ players in danger world

```
1. Multiple players fight simultaneously
2. Multiple deaths occur
3. Check rewards and drops
```

**Expected:**
- ✅ All kills reward shards correctly
- ✅ All deaths drop shards correctly
- ✅ No duplication bugs
- ✅ No missing shards
- ✅ Server remains stable

**Pass/Fail:** _______

---

### Phase 8: Performance Tests

#### Test 19: Server Performance
**Tools needed:** Performance monitoring plugin (e.g., Spark)

**Baseline:**
```
1. Record TPS with no players
2. Record RAM usage
```

**With System Active:**
```
1. 20 players online
2. All holding shards
3. PvP occurring
4. Record TPS and RAM
```

**Expected:**
- ✅ TPS drop: <0.5
- ✅ RAM increase: <50MB
- ✅ No lag spikes

**Pass/Fail:** _______

---

#### Test 20: Long-Term Stability
```
1. Run server for 6+ hours
2. Players constantly join/leave
3. PvP occurs throughout
4. Check console for errors
```

**Expected:**
- ✅ No memory leaks
- ✅ No errors in console
- ✅ TPS remains stable
- ✅ All features work consistently

**Pass/Fail:** _______

---

## 🐛 Bug Tracking Template

If any test fails, use this template:

```
BUG REPORT #___

Test Failed: [Test name/number]
Date: [Date]
Server Version: [Spigot/Paper version]
Skript Version: [Version]
SkBee Version: [Version]

Description:
[What happened vs what should happen]

Steps to Reproduce:
1. [Step 1]
2. [Step 2]
3. [Step 3]

Expected Behavior:
[What should happen]

Actual Behavior:
[What actually happened]

Console Errors:
[Paste any errors]

Screenshots:
[If applicable]

Configuration:
[Relevant config settings]
```

---

## 🔍 Debug Testing

### Enable Debug Mode

**In config.sk:**
```yaml
debug-mode: true
```

**Reload:**
```
/sk reload strength-shard-system
```

### What to Look For

**Console should show:**
```
[DEBUG] Updated stats for PlayerName: 10 shards, +10% damage, +10% speed
[DEBUG] PlayerName killed OtherPlayer and gained 2 shards. Victim dropped 5 shards.
[DEBUG] Removed natural echo shard from chest
```

---

## ✅ Final Verification Checklist

Before going live:

### Functionality
- [ ] All Phase 1-6 tests passed
- [ ] All admin commands work
- [ ] PvP rewards work correctly
- [ ] Death penalties work correctly
- [ ] World detection works

### Security
- [ ] Natural echo shards blocked
- [ ] Placement prevention works
- [ ] No duplication exploits found

### Performance
- [ ] TPS stable with max players
- [ ] No memory leaks
- [ ] No console errors

### Configuration
- [ ] All worlds configured
- [ ] Stats balanced
- [ ] Messages customized
- [ ] Permissions set

### Documentation
- [ ] Player rules written
- [ ] Admin guide ready
- [ ] Tutorial created
- [ ] FAQ prepared

---

## 🎮 Beta Testing Period

### Week 1: Limited Release
- Release to 5-10 trusted players
- Monitor closely for bugs
- Gather initial feedback
- Make balance adjustments

### Week 2: Expanded Beta
- Release to 20-30 players
- Test under load
- Watch for exploits
- Fine-tune rewards

### Week 3: Open Beta
- Release to all players
- Monitor leaderboards
- Check for balance issues
- Prepare for full launch

### Week 4: Full Launch
- System stable
- Players understand mechanics
- Balance confirmed
- Ready for long-term operation

---

## 📊 Metrics to Track

### Daily
- [ ] Total shards in circulation
- [ ] Top shard holders
- [ ] PvP kill count
- [ ] Average shards per player

### Weekly
- [ ] Shard distribution curve
- [ ] Player engagement
- [ ] Balance feedback
- [ ] Bug reports

### Monthly
- [ ] Long-term balance
- [ ] Player retention
- [ ] System impact on gameplay
- [ ] Necessary adjustments

---

## 🔄 Post-Launch Testing

### Ongoing Verification

**Weekly spot checks:**
```
/ss check TopPlayer
/ss top 10
[Check for exploits]
[Verify drops working]
[Test new features after updates]
```

**Monthly full audit:**
- Complete Phase 1-6 tests again
- Test with max player load
- Verify performance
- Check for any degradation

---

## 💡 Testing Tips

1. **Always test on a separate test server first**
2. **Use debug mode during testing**
3. **Document everything**
4. **Test with real players when possible**
5. **Monitor console during tests**
6. **Back up before testing major changes**
7. **Test during peak hours for real conditions**

---

## 🎯 Testing Automation Ideas

### Automated Test Script (Skript)
```skript
command /test-shards:
    permission: op
    trigger:
        send "&eRunning automated tests..." to player
        
        # Test 1: Give shards
        execute player command "/ss give %player% 10"
        wait 1 tick
        
        # Test 2: Check count
        execute player command "/ss check %player%"
        wait 1 tick
        
        # Test 3: Remove shards
        execute player command "/ss remove %player% 10"
        
        send "&aTests complete! Check console for results." to player
```

---

## 📞 Support During Testing

If you encounter issues during testing:

1. **Check this guide first**
2. **Enable debug mode**
3. **Check console for errors**
4. **Review configuration**
5. **Test on clean server**
6. **Ask for help with full details:**
   - Minecraft version
   - Skript version
   - SkBee version
   - Test that failed
   - Console errors
   - Configuration settings

---

**Complete all tests before launch! 🚀**

**Good luck with your testing! ⚔️**
