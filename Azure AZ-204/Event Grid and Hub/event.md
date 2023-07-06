# Events

Sam explains events like so: An event is like another way of doing a data model. I'm used to entities for things e.g. for Retroflect there is the note entity.
An event style system is an alternative and works well for things like bank transfers.
It's good at dealing with the history of something like if you wanted to see all your bank transfers you could have them as a list of events (money in, money out, money between accounts, etc) as that is what information is useful; each event has a time stamp, so you could see all the events in the last month to make up a statement for things that went on.
If you were to do it as an entity you'd probably have a field which was a list of types of transactions and it might become a bit messy - harder to keep a note of what happened when.
You'd need to have the whole API using events instead of entities, so potentially this would work better for microservices and you'd probably have some sort of mapper so that the other microservices that use entities can easily deal with this events API
