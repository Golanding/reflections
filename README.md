reflections
===========

Package reflections provides high level abstractions above the reflect library.

Reflect library is very low-level and as can be quite complex when it comes to do simple things like accessing a structure field value, a field tag...

The purpose of reflections package is to make developers life easier when it comes to introspect structures at runtime.
It's API is freely inspired from python language (getattr, setattr, hasattr...) and provides a simplified access to structure fields and tags.


## Installation

#### Into the gopath

```
    go get github.com/oleiade/reflections
```

#### Import it in your code

```go
    import (
        "github.com/oleiade/reflections"
    )
```

## Usage

#### Accessing structure fields

##### GetField

*GetField* returns the content of a structure field. It can be very usefull when
you'd wanna iterate over a struct specific fields values for example.

```go
    s := MyStruct {
        FirstField: "first value",
        SecondField: 2,
        ThirdField: "third value",
    }

    fieldsToExtract := []string{"FirstField", "ThirdField"}

    for _, fieldName := range fieldsToExtract {
        value, err := reflections.GetField(s, fieldName)
        DoWhatEverWithThatValue(value)
    }
```

##### HasField

*HasField* asserts a field exists through structure.

```go
    s := MyStruct {
        FirstField: "first value",
        SecondField: 2,
        ThirdField: "third value",
    }

    // has == true
    has, _ := reflections.HasField(s, "FirstField")

    // has == false
    has, _ := reflections.HasField(s, "FourthField")
```

##### Fields

*Fields* returns the list of a structure field names, so you can access or modify them later on.

```go
    s := MyStruct {
        FirstField: "first value",
        SecondField: 2,
        ThirdField: "third value",
    }

    var fields []string

    // Fields will list every structure exportable fields.
    // Here, it's content would be equal to:
    // []string{"FirstField", "SecondField", "ThirdField"}
    fields, _ = reflections.Fields(s)
```

##### Items

*Items* returns the structure's field name to values map.

```go
    s := MyStruct {
        FirstField: "first value",
        SecondField: 2,
        ThirdField: "third value",
    }

    var structItems map[string]interface{}

    // Items will return a field name to
    // field value map
    structItems, _ = reflections.Items(s)
```

##### Tags

*Tags* returns the structure's fields tag with the provided key.

```go
    s := MyStruct {
        FirstField: "first value",      `matched:"first tag"`
        SecondField: 2,                 `matched:"second tag"`
        ThirdField: "third value",      `unmatched:"third tag"`
    }

    var structTags map[string]string

    // Tags will return a field name to tag content
    // map. Nota that only field with the tag name
    // you've provided which will be matched.
    // Here structTags will contain:
    // {
    //     "FirstField": "first tag",
    //     "SecondField": "second tag",
    // }
    structTags, _ = reflections.Tags(s, "matched")
```

#### Set a structure field value

*SetField* update's a structure's field value with the one provided. Note that
unexported fields cannot be set, and that field type and value type have to match.

```go
    s := MyStruct {
        FirstField: "first value",
        SecondField: 2,
        ThirdField: "third value",
    }

    // In order to be able to set the structure's values,
    // a pointer to it has to be passed to it.
    _ := reflections.SetField(&s, "FirstField", "new value")

    // If you try to set a field's value using the wrong type,
    // an error will be returned
    err := reflection.SetField(&s, "FirstField", 123)  // err != nil
```

## Important notes

* **unexported fields** cannot be accessed or set using reflections library: the golang reflect library intentionaly prohibits unexported fields values access or modifications.


## Documentation

Documentation is available at http://godoc.org/github.com/oleiade/reflections

## Contribute

* Check for open issues or open a fresh issue to start a discussion around a feature idea or a bug.
* Fork `the repository`_ on GitHub to start making your changes to the **master** branch (or branch off of it).
* Write tests which shows that the bug was fixed or that the feature works as expected.
* Send a pull request and bug the maintainer until it gets merged and published. :) Make sure to add yourself to AUTHORS_.

.. _`the repository`: http://github.com/oleiade/reflections
.. _AUTHORS: https://github.com/oleiade/reflections/blob/master/AUTHORS.rst