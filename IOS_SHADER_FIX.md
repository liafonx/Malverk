# Malverk iOS Shader Fix

## Issue
The Malverk mod's shader code had iOS OpenGL ES compatibility issues causing this error:
```
ERROR: '>' : wrong operand types: no operation '>' exists that takes 
a left-hand operand of type 'temp highp float' and a right operand 
of type 'const int'
```

## Root Cause
iOS's OpenGL ES shader compiler is stricter than desktop OpenGL and requires explicit float literals in comparisons. Desktop OpenGL allows implicit int→float conversion, but iOS does not.

## Fix Applied
**File:** `Malverk/assets/shaders/applied.fs`

**Line 104 - Changed:**
```glsl
// Before (breaks on iOS)
if (texture_selected.g > 0 || texture_selected.g < 0)

// After (works on iOS)
if (texture_selected.g > 0.0 || texture_selected.g < 0.0)
```

## Testing
After this fix:
1. The Malverk mod should load without shader compilation errors on iOS
2. The visual effects should work correctly
3. No impact on desktop/Mac compatibility

## Technical Details
- **Desktop OpenGL:** Allows `float > int` comparisons with implicit conversion
- **OpenGL ES (iOS):** Requires both operands to be the same type
- **Solution:** Use `.0` suffix for float literals (e.g., `0.0` instead of `0`)

This is a common issue when porting desktop GLSL shaders to mobile platforms.
