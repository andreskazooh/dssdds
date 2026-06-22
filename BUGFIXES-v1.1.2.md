# Bug Fixes for Version 1.1.2

## Critical Bugs Fixed

### 1. **Line 126: Periodic Event Syntax Error**
**Issue:** Cannot use options syntax `{@update-interval}` in event headers
```skript
# BEFORE (BROKEN):
every {@update-interval} ticks:

# AFTER (FIXED):
every 5 ticks:
```
**Reason:** Skript 2.15.3 does not support option variables in event headers. The value must be hardcoded.

---

### 2. **Line 267: Give Item Command Syntax Error**
**Issue:** The `give` effect syntax is incorrect for Skript 2.15.3
```skript
# BEFORE (BROKEN):
give {_shard-item} to attacker

# AFTER (FIXED):
add {_shard-item} to inventory of attacker
```
**Reason:** In Skript 2.15.3, the proper syntax for adding items to player inventory is `add <item> to inventory of <player>`, not `give <item> to <player>`.

---

### 3. **Line 342: Admin Command Give Syntax Error**
**Issue:** Same as #2 - incorrect give syntax in admin command
```skript
# BEFORE (BROKEN):
give {_shard} to arg-2

# AFTER (FIXED):
add {_shard} to inventory of arg-2
```

---

### 4. **Line 339 & 355: Ternary Operator Syntax Error**
**Issue:** Skript does not support JavaScript-style ternary operator `? :`
```skript
# BEFORE (BROKEN):
set {_amount} to arg-3 ? 1

# AFTER (FIXED):
# Option 1: Set default in command declaration
command /strengthshard [<text>] [<player>] [<integer=1>]:

# Option 2: Use 'otherwise' syntax (if needed elsewhere)
set {_amount} to arg-3 otherwise 1
```
**Reason:** Skript uses `otherwise` for default values, or defaults can be set directly in command argument declarations using `=` syntax.

---

## Impact of Bugs

### **Before Fixes:**
- ❌ Script would not load due to syntax errors
- ❌ `/ss give` command would fail completely
- ❌ Killing players would not award shards
- ❌ Periodic stat updates would not run
- ❌ Players could not receive shards at all

### **After Fixes:**
- ✅ Script loads without errors
- ✅ `/ss give` command works correctly
- ✅ Killing players properly awards shards
- ✅ Stats update every 5 ticks (4 times per second)
- ✅ All shard distribution mechanics function as intended

---

## Testing Checklist

After applying these fixes, test the following:

1. **Script Loading**
   - [ ] Script loads without errors in console
   - [ ] No warnings during load
   - [ ] "LOADED" message appears in console

2. **Admin Commands**
   - [ ] `/ss give <player>` gives 1 shard (default)
   - [ ] `/ss give <player> 5` gives 5 shards
   - [ ] `/ss check <player>` shows correct count
   - [ ] `/ss remove <player>` removes shards

3. **PvP Mechanics**
   - [ ] Killing a player in danger world awards shards
   - [ ] Victim drops their shards on death
   - [ ] Killer receives the dropped shards

4. **Stat Updates**
   - [ ] Movement speed increases when holding shards
   - [ ] Damage increases when attacking with shards
   - [ ] Action bar shows current stats
   - [ ] Stats update when moving shards in inventory

5. **Cross-World Functionality**
   - [ ] Shards work in ALL worlds (safe and danger)
   - [ ] Can only obtain shards in danger worlds
   - [ ] World change message appears correctly

---

## Additional Notes

- The `update-interval` option in `config.sk` is now ignored for the periodic event
- If you need to change the update frequency, modify line 126 in `main.sk` directly
- All other options from `config.sk` work correctly
- Consider adding a config reload feature in future version to handle the hardcoded tick value

---

## Version History

- **v1.1.0** - Initial release with shard system working in all worlds
- **v1.1.1** - Documentation updates
- **v1.1.2** - Critical bug fixes for syntax errors (this version)
