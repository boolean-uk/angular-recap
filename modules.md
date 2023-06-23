# Angular modules

## Tables of contents

- [Overview](#overview)
- [Creating a module](#creating-a-module)
- [Example module](#example-module)
  - [Declarations](#declarations)
  - [Imports](#imports)
  - [Exports](#exports)

## Overview

Angular modules are used as a means of compartmentalising related components and code together. Typically, we'll compartmentalise code into features. For instance, in a student record system we might have the following modules:

- Students
- Teachers
- Classes

Each of these modules may have several pages, components, and services within them. If we look at the Student modules, we might see a structure like so:

- components
  - `StudentForm`
    - A form responsible for editing fields associated with the student (such as first name, last name, email, etc)
  - `StudentList`
    - A component that displays a list of students
- containers
  - `CreateStudent`
    - A component/page that uses the `StudentForm` component to create a new student
  - `EditStudent`
    - A component/page that uses the `StudentForm` to edit an existing student
  - `ListStudent`
    - A component/page that uses the `StudentList` component to display a list of students
- services
  - `StudentService`
    - A service that holds the list of students, and handles the creation/deletion of students
- `StudentModule`
  - The module that ties all of the above together, as well as imports any dependencies the components/services require, and finally exports any components/services that should be accessible to external modules.

## Creating a module

Like most concepts in Angular, the CLI makes it really easy to create a new module. We simply run the command:

```bash
ng new module example
```

If we want to create the module inside a folder, we might run the command:

```bash
ng new module features/example
```

## Example module

If we ran one of the commands above, and inspect the file that was created, we might see the following:

```ts
import { NgModule } from "@angular/core";
import { CommonModule } from "@angular/common";

@NgModule({
  declarations: [],
  imports: [CommonModule],
  exports: [],
})
export class ExampleModule {}
```

For the most part, the class of the module itself is left empty. The "meat" of the module is contained within the object we pass into the `@NgModule` directive.

Let's take a look at each of the properties of the object and see what they are responsible for.

### Declarations

The `declarations` array that we can see in the example module is responsible for defining all of the components that exist inside the module. For example, if we look back to the `StudentModule` we mentioned in the [overview section](#overview) we might see a `declarations` array like so:

```ts
    {
        declarations: [
            StudentForm,
            StudentList,
            CreateStudent,
            EditStudent,
            ListStudents
        ],
        ...
    }
```

### Imports

The `imports` array that we can see in the example module is responsible for listing any external modules that our `ExampleModule` depends on. For example, if we look bcak to the `StudentModule` we mentioned in the [overview section](#overview) we might see the following:

```ts
    {
        imports: [
            CommonModule,
            ClassesModule
        ],
        ...
    }
```

This might be because the `StudentsModule` requires some components from the `ClassesModule`

### Exports

The `exports` array that we can see in the example module is responsible for listing any components/services external modules may need to use from the `StudentModule`. For example, if we look bcak to the `StudentModule` we mentioned in the [overview section](#overview) we might see the following

```ts
    {
        exports: [
            CreateStudent,
            EditStudent,
            ListStudents
        ],
        ...
    }
```

The above components are pages a user could visit. Due to this, they will likely need to be imported into the module that handles the routing for app. In order for the routing module to be able to have access to these components, two things need to happen. Firstly, the `StudentModule` needs to export them, like we can see above. Secondly, the routing module would need to import the `StudentModule` like we saw in the [imports section](#imports)
