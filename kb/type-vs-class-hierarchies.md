# Type Vs Class Hierarchies 

Use `type` hierarchy and `@handler` classes with `switch ($type)` dispatch (example: formula processing) if:

* it is useful to manage a single operation implementation for all types in one place (that is, in handler class) 
* there are lots of operations to be performed on objects and user code may introduce more
* object is naturally just data (database record, for example) and you don't want to cast it to some class just to run the operation on it 

Use class hierarchy (example: view rendering) if:

* operation implementations for each class are mostly independent
* there are only 1-2 operations and no more are expected
* object is defined in configuration or layer files

If not sure, go with class hierarchy.
