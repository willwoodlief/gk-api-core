# Scheduling Bounds

bounds api defined at [bounds.yaml](../../../api-docs/bounds.yaml)

Create and manage time bounds. These bounds are made up of cron rules, stop and start time, and length of period
Bounds can only be deleted if they are not used anywhere.

Bounds cannot be edited, remove a bounds from an attribute and add in a new one

Bound names can be unique to the bounds of the user, and follow the same rules with numbers, spaces and punctuation
Bound names cannot be aliased, but they are not public either, nobody is going to see your bound names

:id here is either the guid of the bounds, or the bound name. The id must be owned by the user



| Method | Path                     | Route Name | Operation                                        | Args                                                |
|--------|--------------------------|------------|--------------------------------------------------|-----------------------------------------------------|
| Post   | bounds/schedule          |            | Makes a new schedule                             | name, cron, start, stop, period                     |
| Delete | bounds/schedule/:id      |            | Deletes an unused schedule                       |                                                     |
| Get    | bounds/schedule/:id      |            | shows the time data with maybe list of schedules | optional time range for scheduling                  |
| Get    | bounds/schedules         |            | Shows a list of all the bounds the user has      | maybe pagination , optional range to show schedules |
| Get    | bounds/schedule/:id/ping |            | returns true or false if a time in bounds        | date time or none for now                           |

