## To Do

- [x] Register the sheet as a child of the original 5e sheet
- [x] Fill out the sheet stuff for svelte mode
- [x] Implement
  - [x] Full NPC
    - [x] Header
      - [x] HTML
        - [x] Profile
        - [x] Header Stats / INfo
      - [x] SCSS
        - [x] Exhaustion Rounded
        - [x] Exhaustion Squared
        - [x] Rest Rounded
        - [x] Rest Squared
        - [x] Hit Dice Rounded
        - [x] Hit Dice Squared
        - [x] Name row
        - [x] size / origin row
        - [x] speed row
        - [x] Abilities row
      - [x] Behaviors
    - [x] Implement tabs and contents
      - [x] Abilities
        - [x] Side bar
        - [x] Footer (money)
        - [x] Main content area
          - [x] Legendary actions
            - [x] HTML
            - [x] SCSS
            - [x] Behaviors (lock/unlock/show/hide)
          - [x] Features(?): Return to tidy5e-features.html to harvest the setup specifically tailored for NPC
            - [x] HTML
            - [x] SCSS
            - [x] Behaviors
              - [x] Edit on Locked
              - [x] Edit / Dupe / Delete on Unlocked
              - [x] Hide empty section on locked
              - [x] Show Add button on unlocked
        - [x] Settings
          - [x] hideSpellbookTabNpc
            - [x] when on: include equivalent of tidy5e-npc-spellbook template
              - [x] HTML : whole spellbook, unlocked, columns (Name, Components, Target, Usage, Classic Controls), another spell footer with less stuff (spell dc, caster level, spellcasting ability)
                - [x] The footer
                - [x] The rest
              - [x] SCSS
                - [x] The footer
                - [x] The rest
              - [x] Behaviors : context menu, add button, No-spell-levels view, dots,
              - [x] Configure No Spellcaster levels notice while locked
                - [x] While unlocked, also include Create Button itemtablefooter
                - [x] Ensure this same applies to both List and Grid in their own ways.
            - [x] note: give this one a look before completely committing to the currency footer I made
            - [x] Support Grid layout and add Layout Toggle to the Spellbook Title header
          - [x] alt trait position : just below legendary actions
      - [x] Spellbook
        - [x] HTML : this is the exact same spellbook tab as the PC; try your best to reuse verbatim, including all supporting features like filtering and grid layout.
          - [x] The Differences
            - [x] NPC: No preparation button
            - [x] NPC: No favorites button
            - [x] NPC: Add Spellcaster Level
        - [x] SCSS
        - [x] Behaviors
        - [x] Settings
          - [x] hideSpellbookTabNpc
            - [x] when on: hide the spellbook tab altogether
      - [x] Effects
      - [x] Biography
      - [x] Journal
      - [x] Toggle Lock
  - [x] Ltd NPC
    - [x] Configure to appear when user is NOT GM and actor is limited
    - [x] Biography
- [x] Settings
  - [x] Hide NPC Journal
  - [x] Resting for NPCs
  - [x] Hide chat card for NPC rest
  - [x] Mark linked/unlinked NPCs
    - This feature puts an unlinked symbol by the player name when they pull up a sheet that is not linked to the original actor when the option is "unlinked" or "both".
    - It supposedly puts a different symbol there when linked to the original actor when the option is "both".
    - It supposedly puts a different symbol still when there is no token but "both" or "unlinked" is selected.
      - ⚠ The prop accessing logic looks like it has become outdated for determining which token we're looking at. Review this with the debugger on when you are back on your computer with hardware acceleration turned on.
    - [x] Consider packing this information into context when building it ♥
  - [x] Disable health bar
  - [x] Hide hit point overlay
  - [x] Always show traits
  - [x] Move traits below resources
  - [x] Hide Spellbook tab on NPC
  - [x] Default NPC sheet width

### Linked/Unlinked Tokens implementation

I think this might not work anymore... Not completely...:

```js
// In the NPC Sheet class
async function setSheetClasses(app, html, data) {
  const { token } = app;

  // ...

  if (
    token &&
    token.actor.prototypeToken.actorLink &&
    game.settings.get(CONSTANTS.MODULE_ID, 'linkMarkerNpc') == 'both'
  ) {
    html.find('.tidy5e-sheet.tidy5e-npc').addClass('linked');
  }
  if (
    token &&
    !token.actor.prototypeToken.actorLink &&
    (game.settings.get(CONSTANTS.MODULE_ID, 'linkMarkerNpc') == 'unlinked' ||
      game.settings.get(CONSTANTS.MODULE_ID, 'linkMarkerNpc') == 'both')
  ) {
    html.find('.tidy5e-sheet.tidy5e-npc').addClass('unlinked');
  }
  if (
    !token &&
    (game.settings.get(CONSTANTS.MODULE_ID, 'linkMarkerNpc') == 'unlinked' ||
      game.settings.get(CONSTANTS.MODULE_ID, 'linkMarkerNpc') == 'both')
  ) {
    html.find('.tidy5e-sheet.tidy5e-npc').addClass('original');
  }
}
```

## Refactor and Refine

- [x] Some Player settings are applying to NPCs because of sharing actor components. Identify all SettingsProvider calls within shared components and parameterize them out to the parent components to resolve; example: "Show exhaustion tracker only on hover" applies to both but should only apply to PCs.
- [x] Have `TextInput` respect dtype "Number" and perform deltas as expected. The functionality currently lives in `submitText()`.
- [x] Consider converting `tooltip` to just `title` on text inputs
- [x] Convert `src\sheets\character\parts\CharacterHitPoints.svelte` inputs to use the input components and `selectOnFocus={true}`
- [x] Implement NPC Death Saves
  - flags.tidy5e-sheet.death.success / flags.tidy5e-sheet.death.failure
- [x] Consider refactoring so that callers of `submitText()` are adjusted to use `TextInput` or `NumberInput` as necessary.
- [x] Make the HP bar slide smoothly on change
- [x] Do we want to use a base sheet for all actors? No, because they all use different context from established sheets.
- [x] Move `_inventory` and `_inventory-grid` styles to where they should go in the components
- [x] Cannibalize as many `_character-sheet` styles as possible to where they should go in the components
- [x] Extract a universal portait container that directs rounded styles and anything else that can be shareable
  - [x] Cannibalize the global styles which are shared by NPC and Character profiles
- [x] Default Tab applies to NPCs as well as PCs and Vehicles.
  - [x] Wire up for NPC
  - [x] Wire up for Vehicle
- [x] Lair Action Initiative input is not allowing me to clear it out. It always sets it to 0. Fix that.
- [x] "TIDY5E.EmptySection" / "This section is empty. Unlock the sheet to edit." banner should show in the player character sheet Inventory tab when allow-edit is false and there are no inventory sections shown
- [x] PC spellbook tab
  - [x] "DND5E.NoSpellLevels": "This character has no spellcaster levels, but you may add spells manually." if `filters.spellbook.size` is false / 0
  - [x] "TIDY5E.EmptySection" / "This section is empty. Unlock the sheet to edit." if allow-edit is false and there are no spells
- [x] PC features tab
  - [x] "TIDY5E.EmptySection" / "This section is empty. Unlock the sheet to edit." if allow-edit is false and there are no features and it's locked
- [x] Ditto PC effects tab
- [x] Cannibalize `_portrait.scss` styles