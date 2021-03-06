![MapperGenerator-CI](https://github.com/robersonliou/MapperGenerator/workflows/MapperGenerator-CI/badge.svg)
[![Nuget (with prereleases)](https://img.shields.io/nuget/vpre/MapperGenerator)](https://www.nuget.org/packages/MapperGenerator)
# MapperGenerator

A sample mapper using source generator for .NET

## Nuget
#### URL
https://www.nuget.org/packages/MapperGenerator/

#### Add package refernece to `.csproj`
``` xml
<PackageReference Include="MapperGenerator" Version="0.0.2" />
```

## Getting Started

#### 1. Prepare two classes for mapping. Here we create a `PersonEntity` and a `PersonViewModel`.

_PersonEntity.cs_
```csharp
public class PersonEntity
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```

_PersonViewModel.cs_
```csharp
public class PersonViewModel
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```

> For now we only support fully matched mapping, you need to define same type and naming of properties between two related classes.

#### 2. Plug `MappingAttribute` to the mapping target.

_PersonViewModel.cs_
```csharp
[Mapping(typeof(PersonEntity))]
public class PersonViewModel
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```

#### 3. Add namespace `MapperGenerator` to the entry class (ex: `Program.cs/Main` )

#### 4. Then source generator will automatically generate `Mapper.cs` which inculded two mapping method.

_Mapper.cs_
```csharp
//<auto-generated>
using System;
namespace MapperGenerator
{
    public static class Mapper
    {
        public static MyConsumedApp.Models.PersonViewModel MapToPersonViewModel(MyConsumedApp.Entities.Person source)
        {
            var target = new MyConsumedApp.Models.PersonViewModel();
            target.Id = source.Id;
            target.Name = source.Name;
            return target;
        }

        public static MyConsumedApp.Models.PersonViewModel ToPersonViewModel(this MyConsumedApp.Entities.Person source)
        {
            var target = new MyConsumedApp.Models.PersonViewModel();
            target.Id = source.Id;
            target.Name = source.Name;
            return target;
        }
    }
}
```

#### 5. Test result
```csharp
 var personEntity = new Person
{
    Id = 1,
    Name = "Roberson"
};

//static mapping method.
var vm1 = Mapper.MapToPersonViewModel(personEntity);

//extension method.
var vm2 = personEntity.ToPersonViewModel();
```

## Welcome to feedback

Currently it's just a prototype mapper sample created by source generator, welcome to contribute or giving any sugesstion.
