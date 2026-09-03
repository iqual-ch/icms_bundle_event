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
