<script lang="ts">
  import { CONSTANTS } from 'src/constants';
  import { FoundryAdapter } from 'src/foundry/foundry-adapter';
  import { type CharacterSheetContext } from 'src/types/types';
  import SheetEditor from 'src/components/editor/SheetEditor.svelte';
  import { getContext } from 'svelte';
  import type { Readable } from 'svelte/store';
  import RerenderAfterFormSubmission from '../../../components/utility/RerenderAfterFormSubmission.svelte';
  import TextInput from '../../../components/inputs/TextInput.svelte';
  import { TidyFlags } from 'src/foundry/TidyFlags';

  let context = getContext<Readable<CharacterSheetContext>>('context');

  const localize = FoundryAdapter.localize;
</script>

<div class="scroll-container journal-container">
  <div
    class="left-notes note-entries"
    class:limited={$context.showLimitedSheet}
  >
    <RerenderAfterFormSubmission
      andOnValueChange={TidyFlags.tryGetFlag($context.actor, 'notes1.value') ??
        ''}
    >
      <article use:$context.activateEditors>
        <div class="section-titles">
          <TextInput
            document={$context.actor}
            field="flags.{CONSTANTS.MODULE_ID}.notes1.name"
            value={TidyFlags.tryGetFlag($context.actor, 'notes1.name') ?? ''}
            placeholder={localize('TIDY5E.JournalPersonsOfInterest')}
            selectOnFocus={true}
            stopChangePropagation={true}
            disabled={!$context.editable}
          />
        </div>
        <SheetEditor
          content={$context.notes1EnrichedHtml}
          target="flags.{CONSTANTS.MODULE_ID}.notes1.value"
          editable={$context.editable}
        />
      </article>
    </RerenderAfterFormSubmission>
    <RerenderAfterFormSubmission
      andOnValueChange={TidyFlags.tryGetFlag($context.actor, 'notes2.value') ??
        ''}
    >
      <article use:$context.activateEditors>
        <div class="section-titles">
          <TextInput
            document={$context.actor}
            field="flags.{CONSTANTS.MODULE_ID}.notes2.name"
            value={TidyFlags.tryGetFlag($context.actor, 'notes2.name') ?? ''}
            placeholder={localize('TIDY5E.JournalLocationsOfInterest')}
            selectOnFocus={true}
            stopChangePropagation={true}
            disabled={!$context.editable}
          />
        </div>
        <SheetEditor
          content={$context.notes2EnrichedHtml}
          target="flags.{CONSTANTS.MODULE_ID}.notes2.value"
          editable={$context.editable}
        />
      </article>
    </RerenderAfterFormSubmission>
    <RerenderAfterFormSubmission
      andOnValueChange={TidyFlags.tryGetFlag($context.actor, 'notes3.value') ??
        ''}
    >
      <article use:$context.activateEditors>
        <div class="section-titles">
          <TextInput
            document={$context.actor}
            field="flags.{CONSTANTS.MODULE_ID}.notes3.name"
            value={TidyFlags.tryGetFlag($context.actor, 'notes3.name') ?? ''}
            placeholder={localize('TIDY5E.JournalQuests')}
            selectOnFocus={true}
            stopChangePropagation={true}
            disabled={!$context.editable}
          />
        </div>
        <SheetEditor
          content={$context.notes3EnrichedHtml}
          target="flags.{CONSTANTS.MODULE_ID}.notes3.value"
          editable={$context.editable}
        />
      </article>
    </RerenderAfterFormSubmission>
    <RerenderAfterFormSubmission
      andOnValueChange={TidyFlags.tryGetFlag($context.actor, 'notes4.value') ??
        ''}
    >
      <article use:$context.activateEditors>
        <div class="section-titles">
          <TextInput
            document={$context.actor}
            field="flags.{CONSTANTS.MODULE_ID}.notes4.name"
            value={TidyFlags.tryGetFlag($context.actor, 'notes4.name') ?? ''}
            placeholder={localize('TIDY5E.JournalMisc')}
            selectOnFocus={true}
            stopChangePropagation={true}
            disabled={!$context.editable}
          />
        </div>
        <SheetEditor
          content={$context.notes4EnrichedHtml}
          target="flags.{CONSTANTS.MODULE_ID}.notes4.value"
          editable={$context.editable}
        />
      </article>
    </RerenderAfterFormSubmission>
  </div>
  <div
    class="right-notes note-entries"
    class:limited={$context.showLimitedSheet}
  >
    <RerenderAfterFormSubmission
      andOnValueChange={TidyFlags.tryGetFlag($context.actor, 'notes.value') ??
        ''}
    >
      <article class="journal-notes" use:$context.activateEditors>
        <div class="section-titles">{localize('TIDY5E.JournalEntries')}</div>
        <SheetEditor
          content={$context.notesEnrichedHtml}
          target="flags.{CONSTANTS.MODULE_ID}.notes.value"
          editable={$context.editable}
        />
      </article>
    </RerenderAfterFormSubmission>
  </div>
</div>

<style lang="scss">
  .journal-container {
    display: flex;
    align-items: flex-start;
    flex-direction: row;
    overflow-x: inherit;
    gap: 1rem;
  }

  .left-notes,
  .right-notes {
    flex: 1;
    height: 100%;
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }

  .left-notes {
    max-width: 21.875rem;
  }
</style>
