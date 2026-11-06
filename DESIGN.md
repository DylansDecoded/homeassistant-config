# Robson Manor dashboard design

## Context

The dashboard serves household members who need to control lighting, temperature, entrances, and media. Use Home Assistant's native Material components and MDI icons. This keeps controls familiar and avoids frontend extensions.

Design variance: 3/10. Motion: 1/10. Density: 6/10.

The organizing feature is the house itself: daily controls on Home, room-based lighting, separate upstairs and downstairs climate, and dedicated security, camera, and media views. Use real entity states. Show existing camera capabilities accurately. Keep future access policies out of the interface until enforcement exists.

## Typography

Use HA's Roboto/Noto stack for consistency with system dialogs, touch controls, and the Companion app. Native heading, body, and state styles supply the type hierarchy. The card header size is 20px. Do not add web-font dependencies.

## Color

The YAML theme exports these colors as sRGB hex values for compatibility with HA's color helpers. OKLCH equivalents define the palette below. Native state colors retain their existing meanings. Amber marks active lighting, and alerts retain HA's warning and error colors.

| Role | Light | Dark |
|---|---|---|
| Background | oklch(0.9590 0.0059 239.82) | oklch(0.2198 0.0182 255.71) |
| Surface | oklch(0.9869 0.0025 228.78) | oklch(0.2806 0.0247 251.13) |
| Text | oklch(0.2806 0.0247 251.13) | oklch(0.9425 0.0111 243.66) |
| Secondary text | oklch(0.4868 0.0310 251.91) | oklch(0.8010 0.0240 246.10) |
| Border | oklch(0.8965 0.0129 244.27) | oklch(0.3811 0.0329 252.06) |
| Primary action | oklch(0.4779 0.0775 185.39) | oklch(0.7912 0.0755 188.24) |
| Accent | oklch(0.5347 0.1300 51.83) | oklch(0.8086 0.1141 69.60) |

Body text and the primary action color exceed 4.5:1 against the card surface in both modes. Secondary text measures 6.10:1 in light mode and 7.84:1 in dark mode.

## Layout and interaction

- Use native sections with at most three section columns. Phones collapse to one section column.
- Use the native 12-column grid and 8px spacing. Six-column tiles remain two across on phones.
- Use 16px card corners and a single border. Avoid custom shadows and shadow-DOM CSS.
- Keep existing `default_view`, `lights`, and `climate` paths. Add `security`, `cameras`, `media`, and the `scenes` subview.
- Tile text opens entity details. Light and switch icons retain native toggles. Alarm and garage summary icons open details.
- Keep alarm PIN entry in the native alarm panel. Celebration actions require confirmation.
- Display laundry sensors as vibration, without inferring that a cycle is running or finished.
- Keep offline device states visible in their detailed view.
- Use native focus, keyboard, loading, unavailable, and error states. Do not add decorative motion.

## Verification

Date: 2026-11-05.

Rendered every view at 1920x1080 and 390x844 using the live HA frontend, actual states, and a temporary storage preview compiled from the same YAML. Checked for card errors, horizontal overflow, label clipping, and history graph rendering. Checked navigation, entity details, and the celebration confirmation without running device actions. Also rendered Home in dark mode on a phone viewport. Final YAML activation is recorded in the task handoff.

Recheck layout and controls after HA upgrades or when changing the theme.

The Cameras view was also checked at both viewport sizes. The existing 450x450 front-door image loads, the Home shortcut opens Cameras, and the view has no native card errors. Keep its square aspect ratio and label it as a snapshot. The snapshot refresh control was not activated during verification.
