# ICMS Recipe: Event

> A content type and layout for publishing events.

### Useful commands
- Install recipe:
  ```bash
  composer require iqual/icms_bundle_event && drush recipe ../packages/icms_bundle_event
  ```

### Theming topics

Every element that represents an event — calendar chips and spanning bars,
agenda rows, the legend's filter pills, and the teaser cards of an event
listing — carries one `icms-topic-<termId>` class per topic of the event.

This bundle ships no per-topic colors and adds no field to the topics
vocabulary: by default everything uses the frontend's primary color. A project
recolors a topic everywhere with a single declaration, because the components
paint with the custom property these classes set:

```css
.icms-topic-3 { --icms-topic-color: var(--color-blue-500); }
```

Term IDs are used rather than names because names are translated, which would
make the selector language-dependent. An event with several topics carries all
of their classes, so the usual cascade decides when two of them are colored.
Teaser cards carry the class but paint nothing with it, so a listing only
changes appearance if a project opts in.

### Calendar settings

The "Calendar" display of the event layout block is tuned per project through the
config object `icms_bundle_event_logic.calendar_settings` (no admin UI):

```bash
drush config:set icms_bundle_event_logic.calendar_settings week_start 7
```

Keys: `week_start` (1 = Monday … 7 = Sunday), `show_topic_filter`,
`max_events_per_day`, `show_event_time`.

### Registrations

Registrations are Webform submissions attached to the event node. Editors find them
on the event's **Results** tab: one event date at a time, one row per person (the
newest submission wins), searchable, and downloadable as a CSV of exactly what is on
screen. Access follows the registration form's own access rules (**Settings →
Access**), not a permission.

A form counts as a registration form when it has the `node_id` and `occurrence_id`
elements, so duplicated forms get the same screen. Its results table is kept in sync
with the form's elements on every save, unless an editor saves a customized table
(**Results → Customize**) as the default for everyone. To hand it back, clear that
customization and re-save the form. Projects change the listing by overriding
`results()` and `skippedColumns()` of `EventRegistrationsController`.
