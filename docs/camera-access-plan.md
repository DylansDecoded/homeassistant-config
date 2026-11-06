# Camera access requirements

Status: recorded for future work. No camera access restriction is implemented by the dashboard redesign.

The owner needs permanent access to the cameras. During defined off hours, all other users must lose access. Cameras should continue recording and supplying detection events for automations.

## Proposed design

- Use separate accounts. Keep administrator access with the owner.
- Define the off hours with a Home Assistant Schedule helper.
- Enforce access at the viewing service. Dashboard visibility is not authorization.
- Cover live streams, existing sessions, snapshots, recordings, notification images, and shared links.
- Account for all access paths, including Scrypted, Apple Home, Reolink, direct camera protocols, and the NVR.
- Keep private video out of a shared HA dashboard. Forward detection events where needed.
- Recover the current access policy after a restart. A failed schedule controller should not grant access.

Scrypted NVR supports separate users and camera permissions. Scheduled permission changes and active-session revocation need a verified implementation. Do not assume a built-in scheduler exists.

## Decisions still needed

1. Which cameras and viewing apps does each person use?
2. What are the off hours, including weekdays and exceptions?
3. Should footage from off hours remain private after those hours end?
4. Should the policy also suppress detection information or only video?
5. How should an owner override work, and when should it expire?

## Acceptance checks

- The owner can view video during off hours.
- A non-owner cannot start or continue a stream during off hours.
- Non-owner access to recordings, snapshots, and notification images follows the chosen policy.
- No alternate app, local stream URL, or shared credential bypasses the policy.
- Recording and permitted detection automations continue.
- Access restores at the correct time, including after a restart and daylight-saving changes.

Previously downloaded recordings cannot be recalled.

## References

- [Scrypted users and camera permissions](https://docs.scrypted.app/maintenance/access.html)
- [HA Schedule helper](https://www.home-assistant.io/integrations/schedule/)
- [HA view visibility limitations](https://www.home-assistant.io/dashboards/views/#visible)
