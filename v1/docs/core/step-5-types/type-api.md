#  Types

token type api defined at [tokens.yaml](../../../api-docs/tokens.yaml)

The owner of the type is the user who creates it. Type ownership cannot be transferred.

When editing types, the type must not be used anywhere, except one can always retire a type to prevent
the type from create new tokens or being a parent in new types


Token types have an owner, options, attributes, and parents

Token types name and info are in its attributes

(other api here involve creating the token, getting token attribute values , and making a type-group)

| Method | Path          | Route Name | Description                         |
|--------|---------------|------------|-------------------------------------|
| Post   | type          |            | Makes a new type                    |
| Put    | type/:id      |            | Sparse edit an type                 |
| Delete | type/:id      |            | Delete Only if not used             |
| Get    | type/:id/read |            | Gets the type definition and states |
| Get    | type/:id/list |            | List a type where used              |
| Get    | types/list    |            | List all the types                  |

    
        user: one user owns the type
        name: using the naming rules
        is_retired: default false // if true then cannot be added to token types or make new tokens
        options:
            allow_changed_map_bounds: boolean
            allow_changed_time_bounds: boolean
            allow_changed_path_bounds: boolean
            allow_actions: boolean
            attribute_final_list: [or or more attribute ids that children or descendants cannot have]
        attributes: []
        parents: []  -- the order is important
        global_states: [attribute_id, state] (read only unless use api to set the state)


## Creating a type
The user running this is the owner,
Required is name and at least one attribute.



## Editing a type

Any admin in the user's group can edit, but can only edit if not in use
Any part can be changed if type not used.
When type is used, only the retired can be set, and that only affects things made in the future.
One cannot edit a type when tokens are made or children are made.



## Listing where a type is used at

Gives a list of types that inherits this, use wide option to list the descendants to N levels.
Can filter for attributes on the token too. 

## Listing tokens that are created from this type

Gives list of tokens, wide option to list tokens made by descendants to N levels

## Listing types owned by this user

filter by attribute name or type parent or ancestor of N level range, or some combinations
