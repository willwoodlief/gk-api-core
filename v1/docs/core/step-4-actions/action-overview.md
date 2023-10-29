# Actions

Event listeners, that can change attribute values, and can turn them on or off.
Actions can also turn on and off parents of tokens.
The attributes do not need to be attached to the same token as the attribute that holds the action,
but as long as the attribute can be changed by the user who owns the token, it can be changed.


Actions can be registered that run scripts or remotes. 
The values returned by these are the new values of the attribute,
or the truthful value that turns on or off attributes or parent clusters.

But an action results can also be a constant value.

An action runs on the bounds of the attribute it is attached to (and not the entire token).
This means an action can be restricted, and different bounds can do different things for the same event.

Actions listen to events, and can cancel them. If multiple actions are listening to the same event, 
Only one has to cancel it for the event to not be allowed to happen.

An action can only listen to one event.

## Events

Each event has a system attribute that must have a value of truthful for the event to be allowed.
When something is going to happen, any action has the ability to decide to set the event attribute to false or 0,
or event_success of false in a json value

For any event, the attribute is the same, so all tokens have the same creation attribute to listen to.

if an attribute or token is out of bounds, or comes back in bounds, there is no event. Either the thing exists or not.

* token creation
* owner-change
* token-set addition
* token-set removal
* token-set mass attribute altering
* destruction
* value change
* live attribute added to token
* live attribute removed from token
* parent in token turned on
* parent in token turned off

Set tokens (tokens that are part of the definition of a set) have some extra actionable stuff
* Token was added to the set
* Token left the set
* Child was added to the set
* Parent was added to the set
* Link was made from the set


Actions can be used to set rules for creation of a token:

* To disallow creation, each token starts with a created attribute whose value must be truthful, if this value is false, then the token is not saved and creation fails.
* Creation rate limiting is handled by the creation actions script_state

Rate limiting is scripts running on token set changes, or lifecycle changes
* on a lifecycle script, it has to return a truthful value to allow (this is changed from the existing docs)

Multiple actions can listen to the same lifecycle changes or other conditions.
* So, to rate limit token creation and charge someone have two actions. One to limit and one to charge
* But, this can also be the same action

An inheritance chain will run the actions from the ancestors to the current
* if any action fails, then there will be a db rollback, and nothing is saved


# parts of an action

* what attribute the action targets, each action can only change one attribute type. 
  * But, any children or descendants of that type can also be affected, so many attributes can possibly be changed.
  * Optionally, the target can be a parent of a token to turn it on and off 
* What the action does to the target, it can be a switch, or a value change. 
  * If targeting token parents, it can only be a switch
* Can select one or more attributes to have their values set to params to a script or remote call
* Can select a target path for the attribute, to only affect an attribute of certain tokens, types, sets or owners
  * to select an attribute on same token, no path needed. 
  * to select a parent of the token this is on, no path needed

When making an action to charge tokens for an event, use a remote set to quickly run api to check balance and transfer


            so an action:
                action-name: can be any unique name for actions
                action-owner: actions are be owned by a user
                target-path: see paths
                action-type: value change or switch or void (just runs a script or remote)
                input-params: [{path: name of param on the script or remote}]
                run-policy: always, per token, per token type, per set, once only per token type, one only per token
                value: a sript, or remote id, or another attribute path



# Requirements: making sure only so many paths exists

If needing the requirements that only so many things can or can not exist, in how they are in token sets:

Actions can be set to allow or deny any lifecycle, by seeing if the target attribute value changing will cause a target path specifier to fail.
This can be done by counting the number of path specifiers anywhere, using the current target attribute value,
and if only one, before the value change, or not in the allowed count range,
it will block the lifecycle change

Such actions do not need scripts or remotes


# Turning on and off a live attribute

A token's live attribute can be turned off and this will be everywhere

# turning on and off a token's parent

All the attributes from a parent of a token can be turned on and off at once,
but if the token has dynamic attributes that overwrite this then those stay on

