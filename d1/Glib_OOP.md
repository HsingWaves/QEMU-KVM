GLib provides a built-in base class called `GObject`, which is equivalent to **QOM’s `Object`**.

```pgsql
          ┌───────────────────┐
          │    GTypeClass      │  (Base class descriptor)
          └────────┬───────────┘
                   │
          ┌────────▼─────────┐
          │    GObject       │  (Base object, like QObject/QOM Object)
          └────────┬─────────┘
                   │
          ┌────────▼─────────┐
          │  MyObjectClass    │  (User-defined class)
          └────────┬─────────┘
                   │
          ┌────────▼─────────┐
          │    MyObject      │  (User-defined object)
          └──────────────────┘
```

**`GObject` is the base class** → Similar to `QObject` in Qt or `Object` in QOM.

**`GObjectClass` stores function pointers** → Like a **vtable in C++**.

**Derived objects (`MyObject`) embed `GObject`** → Like inheritance in C++.

Compared to QOM, OOP in Glib has Reference Counting.

```C
#include <glib-object.h>
#include <stdio.h>

/* Define MyObject */
typedef struct _MyObject MyObject;
typedef struct _MyObjectClass MyObjectClass;

/* Instance structure */
struct _MyObject {
    GObject parent_instance;  // Inherit from GObject
    int value;  // Custom data field
};

/* Class structure */
struct _MyObjectClass {
    GObjectClass parent_class;  // Inherit from GObjectClass
};

/* Define a type macro */
#define MY_TYPE_OBJECT (my_object_get_type())

/* Declare GObject type */
G_DECLARE_FINAL_TYPE(MyObject, my_object, MY, OBJECT, GObject)

/* Implement GObject */
G_DEFINE_TYPE(MyObject, my_object, G_TYPE_OBJECT)

/* Object initialization */
static void my_object_init(MyObject *self) {
    self->value = 42;  // Set default value
    g_print("MyObject initialized with value=%d\n", self->value);
}

/* Class initialization */
static void my_object_class_init(MyObjectClass *klass) {
    g_print("MyObject class initialized\n");
}

/* Main function */
int main() {
    /* Create a new MyObject instance */
    MyObject *obj = g_object_new(MY_TYPE_OBJECT, NULL);

    /* Access object data */
    g_print("Object value: %d\n", obj->value);

    /* Free the object */
    g_object_unref(obj);

    return 0;
}

```

