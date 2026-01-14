# DazToC4D Live Character Export Guide

## Overview

The Live Character Export feature allows you to update character poses in Daz Studio and see the changes reflected in Cinema 4D **without re-importing the entire character**. This is perfect for iterative pose refinement when rendering in Octane or other Cinema 4D renderers.

## How It Works

The live export workflow consists of two main operations:

1. **Initial Export** - Full character export from Daz Studio (mesh, skeleton, materials, textures)
2. **Pose Update** - Lightweight pose-only update that modifies only the character's position and rotation data

## Workflow

### Step 1: Initial Character Export

1. In **Daz Studio**, select your character
2. Run the normal export: **File → Send To → Daz to Cinema 4D**
3. Configure your export settings (subdivision, morphs, textures, etc.)
4. Wait for the export to complete

### Step 2: Import Character in Cinema 4D

1. In **Cinema 4D**, open the DazToC4D plugin panel
2. Click **"Auto Import Figure"** button
3. The character will be imported with mesh, skeleton, materials, and the initial pose
4. Keep Cinema 4D open for live updates

### Step 3: Update Pose in Daz Studio

1. In **Daz Studio**, adjust your character's pose:
   - Rotate joints
   - Change arm/leg positions
   - Adjust facial expressions (if using morphs)
   - Modify any other pose parameters

2. Once you're happy with the new pose, run the **Update Pose** script:
   - **File → Scripts → Daz to Cinema 4D - Update Pose**
   - Or manually run: `Daz Studio/appdir_common/scripts/support/DAZ/Daz to Cinema 4D - Update Pose.dsa`

3. Select the figure export number when prompted (usually 0 for the first character)

4. Wait for the confirmation message: "Pose updated successfully!"

### Step 4: Update Character in Cinema 4D

1. In **Cinema 4D**, in the DazToC4D plugin panel
2. Click the **"Update Character"** button
3. The character's pose will update instantly to match your Daz Studio changes
4. Render your scene with the new pose

### Step 5: Iterate

Repeat steps 3-4 as many times as needed to perfect your pose:

```
Daz Studio (adjust pose) → Update Pose → Cinema 4D (click Update Character) → Render
```

## Benefits

- **Fast iteration** - No need to re-export/re-import meshes and materials
- **Time-saving** - Updates take seconds instead of minutes
- **Non-destructive** - Original mesh and materials remain unchanged
- **Perfect for rendering** - Fine-tune poses after seeing them in your C4D scene
- **Octane workflow friendly** - Ideal for pose refinement with Octane renderer

## Technical Details

### What Gets Updated

- Joint rotations (all bones in the skeleton)
- Joint positions (root position, hip offset, etc.)
- Joint scales (if modified)

### What Doesn't Get Updated

- Mesh geometry (vertices, faces)
- Materials and textures
- Morph values (planned for future update)
- Skeleton structure

### File Structure

The live export uses the DTU (Daz Transfer Utility) JSON file format:

- **Location**: `~/Documents/DAZ 3D/Bridges/Daz To Cinema 4D/Exports/FIG0/FIG0/FIG0.dtu`
- **Updated Section**: `PoseData` - Contains position, rotation, and scale for each bone
- **New Field**: `LiveUpdateTimestamp` - ISO timestamp of the last pose update

### Daz Studio Scripts

1. **Daz to Cinema 4D.dsa** - Main export script (full export)
2. **Daz to Cinema 4D - Update Pose.dsa** - Pose-only update script (live export)

### Cinema 4D Plugin

- **Update Character Button** - Located in the DazToC4D plugin panel
- **Function**: `Poses.update_live_pose()` in `DtC4DPosing.py`
- **Behavior**: Reloads DTU file and applies new pose to existing skeleton

## Troubleshooting

### "No character found in scene"

- Make sure you've imported a character using "Auto Import Figure" first
- The live update only works on already-imported characters

### "Failed to update character pose"

- Verify that you ran "Update Pose" in Daz Studio first
- Check that the DTU file exists in the export directory
- Ensure the figure export number matches (0, 1, 2, etc.)

### "DTU file not found"

- Perform a full export from Daz Studio first
- The DTU file must exist before you can update it
- Check the export path: `~/Documents/DAZ 3D/Bridges/Daz To Cinema 4D/Exports/`

### Pose doesn't update correctly

- Make sure you're updating the correct figure (if you have multiple)
- Try performing a full export again and then use live updates
- Check the Cinema 4D console for error messages

## Limitations

- Only one character can be updated at a time
- Morph values are not yet supported in live updates (planned)
- Animation frames are not supported (planned)
- Requires both programs to remain open during iteration

## Future Enhancements

- Support for morph/expression updates
- Multi-character updates
- Animation timeline support
- Auto-refresh option (watch for file changes)
- Undo/redo support for pose updates

## Tips

1. **Save your C4D scene** before starting live updates
2. **Use consistent export folders** - Don't mix figure numbers
3. **Test with simple poses first** to ensure the workflow works
4. **Keep track of figure numbers** if working with multiple characters
5. **Check the C4D console** for detailed update messages

## Questions or Issues?

If you encounter any problems with the live export feature, please:

1. Check the Console/Script Log in both Daz Studio and Cinema 4D
2. Verify you're following the workflow steps correctly
3. Report issues at: https://github.com/daz3d/DazToC4D/issues

---

**Version**: 1.4.0 (Live Export Feature)
**Last Updated**: January 2026
