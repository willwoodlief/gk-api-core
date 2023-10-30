# Requirements


Requirements are owned by the owner of the token defining it (not needed to be owned by same user), and have an optional requirement,
an optional parent, and optional links.

Parts of the requirement can be added or removed when the requirement is not in use anywhere.
A requirement can be deleted if its not in use.


If a user can read the defining token, then the requirement can be read and used in operations that does not change it.
If a user can write to the defining token, then the requirement can be altered.

The id of the requirement is unique each time the token is used to define a requirement.


| Method | Path                           | Route Name | Operation               | Args                                                                   |
|--------|--------------------------------|------------|-------------------------|------------------------------------------------------------------------|
| Post   | requirement                    |            | Creates a requirement   | Token, and optional parts                                              |
| post   | requirement:id/part/add        |            | Add a part              | the part added                                                         |
| delete | requirement:id/part/:id/delete |            | Deletes a part          | the id of the part to remove                                           |
| get    | requirement:id/read            |            | gets the requirement    |                                                                        |
| Delete | requirement/:id                |            | Deletes the requirement |                                                                        |
| get    | requirements/list              |            | Lists the requirements  | can filter by token type in definition or part. Otherwise it lists all |


        id
        token:  sets ownership, adds description is needed for this requirement, or for identifying it with a set path, or putting bounds of when this can be used
        parts: [ {
            part_id: each part has a unique id
            type: token-type
            minimum_needed: optional
            maximum_needed: optional
            attributes: [{
                attribute id, ( also parent or ancestor)
                numeric_min:
                numeric_max:
                string_value: (constant or regex)
                numeric_value: (numeric) min and max ignored
            }]
        }]



listing requirements, without filters, will show all requirements the user can write to and alter (but may be currently used).
other filter: currently_used (in type), type in definition, attribute in definition