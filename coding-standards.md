# Castle Coding Standards

The following establishes the basis of Castle coding standard. Please, be a nice guy and follow this. Also fix all different usages on the code base.

**Resharper settings:** If you're a ReSharper user, make sure you're using the shared team settings available with the solution. This will allow you to use clean up command which applies most of the rules outlined below automatically.

## Indentation

It was recently discussed and decided that all files (source code, xml and html) **must use tabs** for indentation. It's possible to change these settings in Visual Studio in Tools/Options/Text Editor/&lt;language&gt;/Tabs. But these settings are global and will affect all projects. The [EditorConfig](https://visualstudiogallery.msdn.microsoft.com/c8bccfe2-650c-4b42-bc5c-845e21f96328) extension allows the settings to be specified on a solution-by-solution (or project-by-project) basis. Some Castle projects have a corresponding .editorconfig file, and should pick up the indentation settings automatically.

## Header

The license header is not optional. Every source file must include it, including test cases. No exceptions.

```csharp
// Copyright 2004-2015 Castle Project - http://www.castleproject.org/
//
// Licensed under the Apache License, Version 2.0 (the "License");
// you may not use this file except in compliance with the License.
// You may obtain a copy of the License at
//
//     http://www.apache.org/licenses/LICENSE-2.0
//
// Unless required by applicable law or agreed to in writing, software
// distributed under the License is distributed on an "AS IS" BASIS,
// WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
// See the License for the specific language governing permissions and
// limitations under the License.
```

## Source file layout

Each .cs file should try to follow this layout:

```csharp
HEADER (see above)

namespace <namespacename>
{
  imports grouped and each group separated by a new line

  public class <classname>
  {
  }
}
```

For example:

```csharp
// Copyright 2004-2015 Castle Project - http://www.castleproject.org/
//
// Licensed under the Apache License, Version 2.0 (the "License");
// you may not use this file except in compliance with the License.
// You may obtain a copy of the License at
//
//     http://www.apache.org/licenses/LICENSE-2.0
//
// Unless required by applicable law or agreed to in writing, software
// distributed under the License is distributed on an "AS IS" BASIS,
// WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
// See the License for the specific language governing permissions and
// limitations under the License.

namespace Castle.ActiveRecord
{
    using System;
    using System.Collections;

    using Castle.ActiveRecord.Framework;
    using Castle.ActiveRecord.Queries;

    using NHibernate;
    using NHibernate.Expression;

    /// <summary>
    /// Allow custom executions using the NHibernate's ISession.
    /// </summary>
    public delegate object NHibernateDelegate(ISession session, object instance);

    /// <summary>
    /// Base class for all ActiveRecord classes. Implements
    /// all the functionality to simplify the code on the
    /// subclasses.
    /// </summary>
    [Serializable]
    public abstract class ActiveRecordBase : ActiveRecordHooksBase
    {
        protected internal static ISessionFactoryHolder holder;

...
```

## Type Members

Group members by their types and access levels.

1. `static` and `readonly` fields (`public`, `protected`, `private` in this order)
1. Instance fields (`public`, `protected`, `private` in this order)
1. Member declarations
  1. Constructors (`public`, `protected`, `private` in this order)
  1. Methods and Properties (grouped by their access level or interface implementation)
  1. `static` members and `private` members at the end

Nested classes should be on the end or within a region that explains/groups the feature

```csharp
[Serializable]
public class SomeClass : BaseClass, IInterface
{
    private static readonly int SomeReadonlyFieldConstant = 1;

    private String something;
    private String else;

    public SomeClass()
    {
    }

    protected SomeClass(int someArgument) : this()
    {
    }

    public void SomeMember()
    {
        ...
    }

    #region IInterface Members

    public void DoSomething()
    {
        ...
    }

    #endregion

    protected void SomeProtectedMethod()
    {
        ...
    }

    protected String PropertyX
    {
        ...
    }

    private void SomePrivateMethod()
    {
    }

}
```

## Blocks

Braces on new line:

```csharp
public void MyMethod()
{
  if (something)
  {
  }
}
```

You can skip braces for branch control statements like exit, break and continue. In this case put the branch on the same line of the if, and use that only if the if statement is really simple.

Example:

```csharp
private void PopulateConfigNodes(XmlNode section)
{
    foreach(XmlNode node in section.ChildNodes)
    {
        if (node.NodeType != XmlNodeType.Element) continue;

        if (!Config_Node_Name.Equals(node.Name))
        {
            String message = String.Format("Unexpected node. Expect '{0}' found '{1}'", Config_Node_Name, node.Name);
...
```

## Properties

Prefer the short form for simple properties:

```csharp
public String Name
{
  get { return name; }
  set { value = value; }
}
```

However, if the property is more complex than that, use the standard form:

```csharp
public string Name
{
  get
  {
   if (something)
   {
     return name;
   }
   return null;
  }
  set { value = value; }
}
```

## Fields

Group them in some logical order. `static` fields first, then the `public` fields (which by the way should be avoided), then `protected`, then `private`.

### Constants

Prefer a meaningful name, even if it's longer.
`private static readonly int DefaultSmtpPort = 22;`

### Static and instance fields

```csharp
private static int name;

private int port;
```

## Others

### String

Prefer `string` instead of `String` (i.e. `System.String`), the C# alias should always be used.

### C-style code

Avoid it like hell. Code like the following should not be on the code base:

```csharp
public static Array BuildObjectArray(Type t, IEnumerable list, bool distinct)
{
    Set s = (distinct ? new ListSet() : null);

    ICollection c = list as ICollection;
    IList newList = c != null ? new ArrayList(c.Count) : new ArrayList();
    foreach (object o in list)
    {
        object[] p = (o is object[] ? (object[]) o : new object[] {o});
        object el = Activator.CreateInstance(t, p);
        if (s == null || s.Add(el))
            newList.Add(el);
    }

    Array a = Array.CreateInstance(t, newList.Count);
    newList.CopyTo(a, 0);
    return a;
}
```

Brevity is good, but at least a minimum of meaningful names is also desirable. Try to combine them to create something like:

```csharp
public static Array BuildObjectArray(Type type, IEnumerable list, bool distinct)
{
    Set set = (distinct ? new ListSet() : null);

    ICollection coll = list as ICollection;
    IList newList = coll != null ? new ArrayList(coll.Count) : new ArrayList();

    foreach (object item in list)
    {
        object[] p = (item is object[] ? (object[]) item : new object[] {item});
        object el = Activator.CreateInstance(type, p);

        if (set == null || set.Add(el))
        {
            newList.Add(el);
        }
    }

    Array a = Array.CreateInstance(type, newList.Count);
    newList.CopyTo(a, 0);
    return a;
}
```