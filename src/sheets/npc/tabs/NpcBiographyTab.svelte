<script lang="ts">
  import { FoundryAdapter } from 'src/foundry/foundry-adapter';
  import SheetEditor from 'src/components/editor/SheetEditor.svelte';

  import type { NpcSheetContext } from 'src/types/types';
  import { CONSTANTS } from 'src/constants';
  import { getContext } from 'svelte';
  import type { Readable } from 'svelte/store';
  import ContentEditableFormField from 'src/components/inputs/ContentEditableFormField.svelte';
  import RerenderAfterFormSubmission from 'src/components/utility/RerenderAfterFormSubmission.svelte';

  let context = getContext<Readable<NpcSheetContext>>('context');

  const localize = FoundryAdapter.localize;

  function activateProseMirrorListeners(node: HTMLElement) {
    $context.activateFoundryJQueryListeners(node);
  }

  $: showNpcPersonalityInfo =
    FoundryAdapter.tryGetFlag($context.actor, 'showNpcPersonalityInfo') ?? false;

  function togglePersonalityInfo() {
    FoundryAdapter.setFlag(
      $context.actor,
      'showNpcPersonalityInfo',
      !showNpcPersonalityInfo
    );
  }
</script>

<div class="notes-container">
  <div class="top-notes note-entries" class:limited={$context.showLimitedSheet}>
    <article>
      <ul class="character-details">
        <li>
          <span>{localize('T5EK.Gender')}:</span>
          <ContentEditableFormField
            element="span"
            editable={$context.owner && !$context.lockSensitiveFields}
            document={$context.actor}
            field="flags.{CONSTANTS.MODULE_ID}.gender"
            value={FoundryAdapter.tryGetFlag($context.actor, 'gender') ?? ''}
            cssClass="detail-input"
          />
        </li>
        <li>
          <span>{localize('T5EK.Age')}:</span>
          <ContentEditableFormField
            element="span"
            editable={$context.owner && !$context.lockSensitiveFields}
            document={$context.actor}
            field="flags.{CONSTANTS.MODULE_ID}.age"
            value={FoundryAdapter.tryGetFlag($context.actor, 'age') ?? ''}
            cssClass="detail-input"
            dataMaxLength={5}
          />
        </li>
        <li>
          <span>{localize('T5EK.Height')}:</span>
          <ContentEditableFormField
            element="span"
            editable={$context.owner && !$context.lockSensitiveFields}
            document={$context.actor}
            field="flags.{CONSTANTS.MODULE_ID}.height"
            value={FoundryAdapter.tryGetFlag($context.actor, 'height') ?? ''}
            cssClass="detail-input"
            dataMaxLength={20}
          />
        </li>
        <li>
          <span>{localize('T5EK.Weight')}:</span>
          <ContentEditableFormField
            element="span"
            editable={$context.owner && !$context.lockSensitiveFields}
            document={$context.actor}
            field="flags.{CONSTANTS.MODULE_ID}.weight"
            value={FoundryAdapter.tryGetFlag($context.actor, 'weight') ?? ''}
            cssClass="detail-input"
            dataMaxLength={20}
          />
        </li>
        <li>
          <span>{localize('T5EK.Eyes')}:</span>
          <ContentEditableFormField
            element="span"
            editable={$context.owner && !$context.lockSensitiveFields}
            document={$context.actor}
            field="flags.{CONSTANTS.MODULE_ID}.eyes"
            value={FoundryAdapter.tryGetFlag($context.actor, 'eyes') ?? ''}
            cssClass="detail-input"
            dataMaxLength={40}
          />
        </li>
        <li>
          <span>{localize('T5EK.Skin')}:</span>
          <ContentEditableFormField
            element="span"
            editable={$context.owner && !$context.lockSensitiveFields}
            document={$context.actor}
            field="flags.{CONSTANTS.MODULE_ID}.skin"
            value={FoundryAdapter.tryGetFlag($context.actor, 'skin') ?? ''}
            cssClass="detail-input"
            dataMaxLength={40}
          />
        </li>
        <li>
          <span>{localize('T5EK.Hair')}:</span>
          <ContentEditableFormField
            element="span"
            editable={$context.owner && !$context.lockSensitiveFields}
            document={$context.actor}
            field="flags.{CONSTANTS.MODULE_ID}.hair"
            value={FoundryAdapter.tryGetFlag($context.actor, 'hair') ?? ''}
            cssClass="detail-input"
            dataMaxLength={40}
          />
        </li>
      </ul>
    </article>
  </div>
  <div class="flex-row extra-small-gap full-height">
    <div
      on:click={togglePersonalityInfo}
      class="toggle-personality-info"
      title={localize('T5EK.TogglePersonalityInfo')}
    >
      {#if showNpcPersonalityInfo}
        <i class="fas fa-angle-double-left" />
      {:else}
        <i class="fas fa-angle-double-right" />
      {/if}
    </div>
    <div class="main-notes">
      {#if showNpcPersonalityInfo}
        <div
          class="left-notes note-entries"
          class:limited={$context.showLimitedSheet}
        >
          <RerenderAfterFormSubmission
            andOnValueChange={FoundryAdapter.tryGetFlag(
              $context.actor,
              'trait'
            ) ?? ''}
          >
            <article use:activateProseMirrorListeners>
              <div class="section-titles biopage">
                {localize('DND5E.PersonalityTraits')}
              </div>
              <SheetEditor
                content={FoundryAdapter.tryGetFlag($context.actor, 'trait') ?? ''}
                target="flags.{CONSTANTS.MODULE_ID}.trait"
                editable={$context.owner || FoundryAdapter.userIsGm()}
              />
            </article>
          </RerenderAfterFormSubmission>
          <RerenderAfterFormSubmission
            andOnValueChange={FoundryAdapter.tryGetFlag(
              $context.actor,
              'ideal'
            ) ?? ''}
          >
            <article use:activateProseMirrorListeners>
              <div class="section-titles biopage">
                {localize('DND5E.Ideals')}
              </div>
              <SheetEditor
                content={FoundryAdapter.tryGetFlag($context.actor, 'ideal') ?? ''}
                target="flags.{CONSTANTS.MODULE_ID}.ideal"
                editable={$context.owner || FoundryAdapter.userIsGm()}
              />
            </article>
          </RerenderAfterFormSubmission>
          <RerenderAfterFormSubmission
            andOnValueChange={FoundryAdapter.tryGetFlag($context.actor, 'bond') ??
              ''}
          >
            <article use:activateProseMirrorListeners>
              <div class="section-titles biopage">
                {localize('DND5E.Bonds')}
              </div>
              <SheetEditor
                content={FoundryAdapter.tryGetFlag($context.actor, 'bond') ?? ''}
                target="flags.{CONSTANTS.MODULE_ID}.bond"
                editable={$context.owner || FoundryAdapter.userIsGm()}
              />
            </article>
          </RerenderAfterFormSubmission>
          <RerenderAfterFormSubmission
            andOnValueChange={FoundryAdapter.tryGetFlag($context.actor, 'flaw') ??
              ''}
          >
            <article use:activateProseMirrorListeners>
              <div class="section-titles biopage">
                {localize('DND5E.Flaws')}
              </div>
              <SheetEditor
                content={FoundryAdapter.tryGetFlag($context.actor, 'flaw') ?? ''}
                target="flags.{CONSTANTS.MODULE_ID}.flaw"
                editable={$context.owner || FoundryAdapter.userIsGm()}
              />
            </article>
          </RerenderAfterFormSubmission>
        </div>
      {/if}
      <div
        class="right-notes note-entries"
        class:limited={$context.showLimitedSheet}
      >
        <RerenderAfterFormSubmission
          andOnValueChange={FoundryAdapter.tryGetFlag(
            $context.actor,
            'appearance'
          ) ?? ''}
        >
          <article class="appearance-notes" use:activateProseMirrorListeners>
            <div class="section-titles biopage">
              {localize('DND5E.Appearance')}
            </div>
            <SheetEditor
              content={FoundryAdapter.tryGetFlag($context.actor, 'appearance') ??
                ''}
              target="flags.{CONSTANTS.MODULE_ID}.appearance"
              editable={$context.owner || FoundryAdapter.userIsGm()}
            />
          </article>
        </RerenderAfterFormSubmission>
        <RerenderAfterFormSubmission
          andOnValueChange={$context.system.details.biography.value}
        >
          <article class="biography-notes" use:activateProseMirrorListeners>
            <div class="section-titles">
              {localize('DND5E.Background')}/{localize('DND5E.Biography')}
            </div>
            <SheetEditor
              content={$context.system.details.biography.value}
              target="system.details.biography.value"
              editable={$context.owner || FoundryAdapter.userIsGm()}
            />
          </article>
        </RerenderAfterFormSubmission>
      </div>
    </div>
  </div>
</div>

<style lang="scss">
  .notes-container {
    display: flex;
    flex: 1;
    flex-direction: column;
    height: 100%;
  }

  .main-notes {
    display: flex;
    flex: 1;
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

  .top-notes {
    width: 100%;
    margin-bottom: 1rem;

    .character-details {
      margin: 0;
      padding: 0;
      display: flex;
      gap: 0.25rem;
      flex-wrap: wrap;
    }

    li {
      display: flex;
      flex: 0 0 calc(100% / 3 - 4px);

      :global(span) {
        flex: 1;
        margin: 0 0.5rem 0 0.25rem;
      }

      :global(span:first-child) {
        flex: 0 0 0.0625rem;
        font-weight: 600;
        margin: 0;
      }
    }
  }

  .character-details :global(.detail-input) {
    flex: 1;
    margin: 0 0.5rem 0 0.25rem;
  }

  .toggle-personality-info {
    align-self: flex-start;
    padding: 1.25rem 0.25rem;
    background: var(--t5ek-faint-color);
    border-radius: 0.1875rem;
    margin-right: 0.25rem;
    color: var(--t5ek-tertiary-color);
    cursor: pointer;

    &:hover {
      color: var(--t5ek-secondary-color);
    }
  }
</style>