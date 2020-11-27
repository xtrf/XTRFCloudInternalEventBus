# Versioning

## Introducing changes to the events

Because of the nature of the distributed systems and variety of producers and consumers for this internal bus it is not reasonable to require all consumers to know every event of every system and follow strict versioning scheme with required updates when new one is released.

The recommended approach is to never break backwards compatibility and if it is neccessary, then introduce grace period when consumers can be updated, while emitting event than can be consumed by older and newer clients.

## What could change

There are several potential types of changes:

1. New event is introduced
2. Event is changed
   2.1. New field is added
   2.2. Field type/meaning is changed
   2.3. Field is removed
3. Event is removed

Preserving BC on internal bus means that:

- New events can be introduced (1.), but we should be carefull to not add more fields than necessary and specify clearly its purpose
- New fields can be added (2.1) to events
- Field type/meaning CAN NOT change (2.2) and we should use add+remove operation performed over longer period
- Field can only be removed (2.3.) if no consumer is using it
- Events can only be removed (3.) if no consumer is using it

Following those rules will allow smooth operation of all components without any downtimes.

## Consumer guidelines

- Consumer should ignore unknown events
- Consumer should only use the fields it needs and ignore others

## Producer guidelines

- If producer wants to add something, the purpose should be clearly specified
  - Keep in mind, that current consumers will ignore this event/field
  - If consumers should support new event immediately, then producer should start emitting it after it is implemented in consumers first
  - If consumers should support new required field immediately, then the field should be introduced at first as an optional one, so consumers can handle events with and without it
- If producer wants to change an event, the option of creating new one should be explored
  - Keep in mind the purpose of the original event
  - If new event is introduced instead, the original event should still be emitted until all consumers switch to the new one
  - Modifying fields can only be accomplished by adding new field and at a later time removing the old one after all consumers are upgraded
