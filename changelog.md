# Change Log

## v0.1.1

* Combine links and actions as transitions
* Add `bodyTemplate` to Transitions

## v0.1.0

* Add root `hyperdescribe` element
* Change `resources` to `entities` to make more generic
* Make `entities` a recursive object while also make a root-level entity
* Move data-specific items under `content` property for entities.
* Add `code` in `meta` for linking to code
* Change `name` to Array `classes`
* Add `options` to Field Objects. This allows the field to act like a `<select>` in HTML
* Add `defaultValue` to Field Objects. This says what the default value of the field is, and can be changed.If the `value` field is set, though, the value cannot be changed