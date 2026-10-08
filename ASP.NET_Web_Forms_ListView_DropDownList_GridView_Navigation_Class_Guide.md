# ASP.NET Web Forms — ListView, DropDownList, GridView & Navigation Controls

## Classroom Practical Guide

This lesson demonstrates:

1. ListView control
2. DropDownList with Data Binding
3. GridView control
4. Data Binding in GridView
5. Sorting GridView
6. Paging GridView
7. Navigation controls
8. Line-by-line explanation
9. Complete working project code

> **Technology:** ASP.NET Web Forms with C#  
> **Project type:** ASP.NET Web Application / Web Forms  
> **Example:** Student Management

---

# 1. Learning Objectives

After this class, students should understand:

- What a server-side data-bound control is.
- How to bind a collection to a control.
- How `ListView` displays repeated records.
- How `DropDownList` displays selectable values.
- How `GridView` displays tabular data.
- How `DataSource` and `DataBind()` work.
- How to sort GridView data.
- How to implement paging.
- How navigation controls work.
- How controls communicate through events.

---

# 2. Important Concept — Data Binding

Data binding means connecting a UI control with data.

For example:

```text
C# List
   ↓
DataSource
   ↓
DataBind()
   ↓
GridView / ListView / DropDownList
   ↓
HTML displayed in browser
```

Example:

```csharp
GridView1.DataSource = students;
GridView1.DataBind();
```

### Line 1

```csharp
GridView1.DataSource = students;
```

The `students` collection becomes the data source of the GridView.

### Line 2

```csharp
GridView1.DataBind();
```

ASP.NET reads the data source and generates the HTML rows.

---

# 3. Project Structure

Create a Web Forms project:

```text
StudentWebForms
│
├── Site.Master
├── Site.Master.cs
├── Default.aspx
├── Default.aspx.cs
├── Students.aspx
├── Students.aspx.cs
└── Web.config
```

For this classroom example, we will mainly use:

```text
Students.aspx
Students.aspx.cs
Site.Master
Site.Master.cs
```

---

# 4. Model Class

Create:

```text
Student.cs
```

Code:

```csharp
public class Student
{
    public int Id { get; set; }

    public string Name { get; set; }

    public string Course { get; set; }

    public string City { get; set; }

    public int Mark { get; set; }
}
```

## Line-by-line explanation

```csharp
public class Student
```

Creates a class named `Student`.

```csharp
public int Id { get; set; }
```

Stores the student's ID.

```csharp
public string Name { get; set; }
```

Stores the student's name.

```csharp
public string Course { get; set; }
```

Stores the course name.

```csharp
public string City { get; set; }
```

Stores the student's city.

```csharp
public int Mark { get; set; }
```

Stores the student's mark.

---

# 5. Creating Sample Data

For this classroom project, we will use an in-memory list instead of a database.

Create a method:

```csharp
private List<Student> GetStudents()
{
    return new List<Student>
    {
        new Student { Id = 1, Name = "Arun", Course = "ASP.NET", City = "Pondicherry", Mark = 85 },
        new Student { Id = 2, Name = "Priya", Course = "React", City = "Chennai", Mark = 91 },
        new Student { Id = 3, Name = "Karthik", Course = "AWS", City = "Pondicherry", Mark = 78 },
        new Student { Id = 4, Name = "Divya", Course = "Azure", City = "Villupuram", Mark = 88 },
        new Student { Id = 5, Name = "Vijay", Course = "ASP.NET", City = "Cuddalore", Mark = 72 },
        new Student { Id = 6, Name = "Meena", Course = "React", City = "Chennai", Mark = 95 },
        new Student { Id = 7, Name = "Ravi", Course = "AWS", City = "Pondicherry", Mark = 81 },
        new Student { Id = 8, Name = "Anitha", Course = "Azure", City = "Chennai", Mark = 89 },
        new Student { Id = 9, Name = "Suresh", Course = "ASP.NET", City = "Pondicherry", Mark = 67 },
        new Student { Id = 10, Name = "Lakshmi", Course = "React", City = "Cuddalore", Mark = 93 }
    };
}
```

## Explanation

```csharp
private List<Student> GetStudents()
```

Creates a private method that returns a list of `Student` objects.

```csharp
return new List<Student>
```

Creates a new list.

```csharp
new Student
```

Creates one student object.

```csharp
Id = 1
```

Assigns ID 1.

```csharp
Name = "Arun"
```

Assigns the student's name.

```csharp
Course = "ASP.NET"
```

Assigns the course.

```csharp
City = "Pondicherry"
```

Assigns the city.

```csharp
Mark = 85
```

Assigns the mark.

---

# 6. Working with ListView

## Students.aspx

```aspx
<%@ Page Language="C#" AutoEventWireup="true"
    CodeBehind="Students.aspx.cs"
    Inherits="StudentWebForms.Students" %>

<!DOCTYPE html>

<html>
<head runat="server">
    <title>Student List</title>
</head>

<body>

<form id="form1" runat="server">

    <h1>Student List using ListView</h1>

    <asp:ListView ID="lvStudents"
        runat="server">

        <LayoutTemplate>

            <div id="itemPlaceholder"
                runat="server">
            </div>

        </LayoutTemplate>

        <ItemTemplate>

            <div style="border:1px solid #ccc;
                        padding:10px;
                        margin:10px;">

                <h3>
                    <%# Eval("Name") %>
                </h3>

                <p>
                    Course:
                    <%# Eval("Course") %>
                </p>

                <p>
                    City:
                    <%# Eval("City") %>
                </p>

                <p>
                    Mark:
                    <%# Eval("Mark") %>
                </p>

            </div>

        </ItemTemplate>

    </asp:ListView>

</form>

</body>
</html>
```

---

# 7. ListView Line-by-Line Explanation

```aspx
<asp:ListView ID="lvStudents" runat="server">
```

Creates an ASP.NET ListView.

### `ID`

```text
lvStudents
```

The name used from C#.

### `runat="server"`

Tells ASP.NET that this is a server-side control.

---

## LayoutTemplate

```aspx
<LayoutTemplate>
```

Defines the overall layout of the ListView.

```aspx
<div id="itemPlaceholder" runat="server">
</div>
```

This is the location where ListView items are inserted.

---

## ItemTemplate

```aspx
<ItemTemplate>
```

Defines how one record should look.

If there are 10 students, ASP.NET repeats this template 10 times.

---

## Eval()

```aspx
<%# Eval("Name") %>
```

Reads the `Name` property from the current data item.

For example:

```text
Arun
Priya
Karthik
...
```

The same idea works for:

```aspx
<%# Eval("Course") %>
<%# Eval("City") %>
<%# Eval("Mark") %>
```

---

# 8. Binding ListView

Open:

```text
Students.aspx.cs
```

Use:

```csharp
protected void Page_Load(object sender, EventArgs e)
{
    if (!IsPostBack)
    {
        BindListView();
    }
}

private void BindListView()
{
    lvStudents.DataSource = GetStudents();

    lvStudents.DataBind();
}
```

## Line-by-line

```csharp
protected void Page_Load(object sender, EventArgs e)
```

This event executes when the page loads.

```csharp
if (!IsPostBack)
```

Checks whether this is the first page load.

This is important because we do not want to reload the data unnecessarily after every button click.

```csharp
BindListView();
```

Calls our binding method.

```csharp
lvStudents.DataSource = GetStudents();
```

Gets the student list and assigns it to the ListView.

```csharp
lvStudents.DataBind();
```

Actually binds the data to the ListView.

---

# 9. DropDownList

A DropDownList allows the user to select one item.

Example:

```aspx
<asp:DropDownList
    ID="ddlCourse"
    runat="server"
    AutoPostBack="true"
    OnSelectedIndexChanged="ddlCourse_SelectedIndexChanged">
</asp:DropDownList>
```

## Explanation

```text
ID="ddlCourse"
```

C# will use this name.

```text
AutoPostBack="true"
```

The page posts back immediately when the user changes the selection.

```text
OnSelectedIndexChanged
```

Calls the C# event handler.

---

# 10. Binding DropDownList

C#:

```csharp
private void BindCourses()
{
    ddlCourse.DataSource = new List<string>
    {
        "ASP.NET",
        "React",
        "AWS",
        "Azure"
    };

    ddlCourse.DataBind();

    ddlCourse.Items.Insert(0, new ListItem("-- Select Course --", ""));
}
```

## Explanation

```csharp
ddlCourse.DataSource = new List<string>
```

Creates the data source.

```csharp
"ASP.NET",
"React",
"AWS",
"Azure"
```

These become dropdown items.

```csharp
ddlCourse.DataBind();
```

Transfers the data into the DropDownList.

```csharp
ddlCourse.Items.Insert(...)
```

Adds a default option at the top.

---

# 11. Filtering Data Using DropDownList

Add a GridView:

```aspx
<asp:GridView
    ID="gvStudents"
    runat="server"
    AutoGenerateColumns="False"
    AllowSorting="true"
    AllowPaging="true"
    PageSize="4"
    OnSorting="gvStudents_Sorting"
    OnPageIndexChanging="gvStudents_PageIndexChanging">

    <Columns>

        <asp:BoundField
            DataField="Id"
            HeaderText="ID"
            SortExpression="Id" />

        <asp:BoundField
            DataField="Name"
            HeaderText="Name"
            SortExpression="Name" />

        <asp:BoundField
            DataField="Course"
            HeaderText="Course"
            SortExpression="Course" />

        <asp:BoundField
            DataField="City"
            HeaderText="City"
            SortExpression="City" />

        <asp:BoundField
            DataField="Mark"
            HeaderText="Mark"
            SortExpression="Mark" />

    </Columns>

</asp:GridView>
```

---

# 12. GridView Explanation

```aspx
<asp:GridView
```

Creates a GridView server control.

```text
ID="gvStudents"
```

The C# variable name.

```text
AutoGenerateColumns="False"
```

Prevents ASP.NET from automatically generating columns.

Why use `False`?

Because we want full control over the columns.

---

## AllowSorting

```text
AllowSorting="true"
```

Allows the user to click sortable column headers.

---

## AllowPaging

```text
AllowPaging="true"
```

Enables pagination.

---

## PageSize

```text
PageSize="4"
```

Displays four records per page.

---

# 13. BoundField

Example:

```aspx
<asp:BoundField
    DataField="Name"
    HeaderText="Name"
    SortExpression="Name" />
```

### DataField

```text
DataField="Name"
```

Gets the `Name` property from the Student object.

### HeaderText

```text
HeaderText="Name"
```

Displays `Name` in the table header.

### SortExpression

```text
SortExpression="Name"
```

Tells GridView which property to sort.

---

# 14. Binding GridView

C#:

```csharp
private void BindGrid()
{
    gvStudents.DataSource = GetStudents();

    gvStudents.DataBind();
}
```

This is the basic GridView data-binding process.

---

# 15. Complete Page_Load

```csharp
protected void Page_Load(object sender, EventArgs e)
{
    if (!IsPostBack)
    {
        BindCourses();

        BindListView();

        BindGrid();
    }
}
```

## Execution flow

When the page opens:

```text
Page_Load
   ↓
IsPostBack?
   ↓
NO
   ↓
BindCourses()
   ↓
BindListView()
   ↓
BindGrid()
   ↓
Browser displays controls
```

---

# 16. GridView Sorting

GridView sorting event:

```csharp
protected void gvStudents_Sorting(
    object sender,
    GridViewSortEventArgs e)
{
    List<Student> students = GetStudents();

    students = students
        .OrderBy(x => x.GetType()
        .GetProperty(e.SortExpression)
        .GetValue(x, null))
        .ToList();

    gvStudents.DataSource = students;

    gvStudents.DataBind();
}
```

However, for beginners, it is easier and safer to explicitly handle the fields.

Use this classroom-friendly version:

```csharp
private string SortDirection
{
    get
    {
        return ViewState["SortDirection"] as string ?? "ASC";
    }

    set
    {
        ViewState["SortDirection"] = value;
    }
}

private string SortColumn
{
    get
    {
        return ViewState["SortColumn"] as string ?? "Id";
    }

    set
    {
        ViewState["SortColumn"] = value;
    }
}
```

---

# 17. Simple Sorting Code

Add:

```csharp
using System.Linq;
```

Then:

```csharp
protected void gvStudents_Sorting(
    object sender,
    GridViewSortEventArgs e)
{
    List<Student> students = GetStudents();

    if (e.SortExpression == "Name")
    {
        students = SortDirection == "ASC"
            ? students.OrderBy(x => x.Name).ToList()
            : students.OrderByDescending(x => x.Name).ToList();
    }

    if (e.SortExpression == "Course")
    {
        students = SortDirection == "ASC"
            ? students.OrderBy(x => x.Course).ToList()
            : students.OrderByDescending(x => x.Course).ToList();
    }

    if (e.SortExpression == "City")
    {
        students = SortDirection == "ASC"
            ? students.OrderBy(x => x.City).ToList()
            : students.OrderByDescending(x => x.City).ToList();
    }

    if (e.SortExpression == "Mark")
    {
        students = SortDirection == "ASC"
            ? students.OrderBy(x => x.Mark).ToList()
            : students.OrderByDescending(x => x.Mark).ToList();
    }

    if (SortColumn == e.SortExpression)
    {
        SortDirection = SortDirection == "ASC"
            ? "DESC"
            : "ASC";
    }
    else
    {
        SortColumn = e.SortExpression;
        SortDirection = "ASC";
    }

    gvStudents.DataSource = students;

    gvStudents.DataBind();
}
```

> **Important classroom point:** For a real database application, sorting should normally be performed by the database query rather than loading all records into memory.

---

# 18. Easier Sorting Version for Teaching

For a first demonstration, teach this simpler example:

```csharp
protected void gvStudents_Sorting(
    object sender,
    GridViewSortEventArgs e)
{
    var students = GetStudents();

    if (e.SortExpression == "Mark")
    {
        students = students
            .OrderByDescending(x => x.Mark)
            .ToList();
    }

    gvStudents.DataSource = students;

    gvStudents.DataBind();
}
```

Now when students click:

```text
Mark
```

the records appear from highest to lowest mark.

---

# 19. GridView Paging

GridView event:

```csharp
protected void gvStudents_PageIndexChanging(
    object sender,
    GridViewPageEventArgs e)
{
    gvStudents.PageIndex = e.NewPageIndex;

    BindGrid();
}
```

## Line-by-line

```csharp
protected void gvStudents_PageIndexChanging(...)
```

Runs when the user selects another page.

```csharp
gvStudents.PageIndex = e.NewPageIndex;
```

Changes the current page.

Example:

```text
Current page = 0

User clicks page 2

NewPageIndex = 1
```

GridView uses zero-based page indexes.

```csharp
BindGrid();
```

Reloads the data.

---

# 20. Filtering GridView with DropDownList

HTML:

```aspx
<asp:DropDownList
    ID="ddlCourse"
    runat="server"
    AutoPostBack="true"
    OnSelectedIndexChanged="ddlCourse_SelectedIndexChanged">
</asp:DropDownList>
```

C#:

```csharp
protected void ddlCourse_SelectedIndexChanged(
    object sender,
    EventArgs e)
{
    string selectedCourse = ddlCourse.SelectedValue;

    var students = GetStudents();

    if (!string.IsNullOrEmpty(selectedCourse))
    {
        students = students
            .Where(x => x.Course == selectedCourse)
            .ToList();
    }

    gvStudents.DataSource = students;

    gvStudents.DataBind();
}
```

## Flow

```text
User selects AWS
       ↓
SelectedIndexChanged
       ↓
SelectedValue = "AWS"
       ↓
Where(Course == "AWS")
       ↓
GridView
       ↓
Only AWS students displayed
```

---

# 21. Working with Navigation Controls

ASP.NET Web Forms provides several navigation-related controls.

Common examples:

```text
Menu
SiteMapPath
TreeView
HyperLink
LinkButton
```

---

# 22. HyperLink Control

Example:

```aspx
<asp:HyperLink
    ID="lnkHome"
    runat="server"
    NavigateUrl="~/Default.aspx"
    Text="Home">
</asp:HyperLink>
```

### Explanation

```text
ID
```

Server-side identifier.

```text
NavigateUrl
```

Destination page.

```text
Text
```

Text displayed to the user.

---

# 23. Menu Control

Example:

```aspx
<asp:Menu
    ID="MainMenu"
    runat="server"
    Orientation="Horizontal">

    <Items>

        <asp:MenuItem
            Text="Home"
            NavigateUrl="~/Default.aspx" />

        <asp:MenuItem
            Text="Students"
            NavigateUrl="~/Students.aspx" />

        <asp:MenuItem
            Text="Courses">

            <asp:MenuItem
                Text="ASP.NET"
                NavigateUrl="~/AspNet.aspx" />

            <asp:MenuItem
                Text="React"
                NavigateUrl="~/React.aspx" />

            <asp:MenuItem
                Text="AWS"
                NavigateUrl="~/Aws.aspx" />

        </asp:MenuItem>

    </Items>

</asp:Menu>
```

---

# 24. Menu Structure

The result is conceptually:

```text
Home | Students | Courses
                    |
                    +-- ASP.NET
                    +-- React
                    +-- AWS
```

This demonstrates hierarchical navigation.

---

# 25. SiteMapPath

`SiteMapPath` shows the user's current location.

Example:

```aspx
<asp:SiteMapPath
    ID="SiteMapPath1"
    runat="server">
</asp:SiteMapPath>
```

Output can look like:

```text
Home > Students > Student Details
```

This is commonly called a breadcrumb.

---

# 26. Web.sitemap

Create:

```text
Web.sitemap
```

Code:

```xml
<?xml version="1.0" encoding="utf-8" ?>

<siteMapNode
    xmlns="http://schemas.microsoft.com/AspNet/SiteMap-File-1.0"
    title="Home"
    url="~/Default.aspx">

    <siteMapNode
        title="Students"
        url="~/Students.aspx" />

    <siteMapNode
        title="Courses"
        url="~/Courses.aspx">

        <siteMapNode
            title="ASP.NET"
            url="~/AspNet.aspx" />

        <siteMapNode
            title="React"
            url="~/React.aspx" />

        <siteMapNode
            title="AWS"
            url="~/Aws.aspx" />

    </siteMapNode>

</siteMapNode>
```

---

# 27. Using SiteMapPath

In the page:

```aspx
<asp:SiteMapPath
    ID="SiteMapPath1"
    runat="server">
</asp:SiteMapPath>
```

ASP.NET reads `Web.sitemap` and generates the breadcrumb.

---

# 28. TreeView Control

TreeView is useful for hierarchical data.

Example:

```aspx
<asp:TreeView
    ID="tvCourses"
    runat="server">

    <Nodes>

        <asp:TreeNode
            Text="Courses">

            <asp:TreeNode
                Text="Frontend"
                NavigateUrl="~/Frontend.aspx" />

            <asp:TreeNode
                Text="Backend"
                NavigateUrl="~/Backend.aspx" />

            <asp:TreeNode
                Text="Cloud">

                <asp:TreeNode
                    Text="AWS"
                    NavigateUrl="~/Aws.aspx" />

                <asp:TreeNode
                    Text="Azure"
                    NavigateUrl="~/Azure.aspx" />

            </asp:TreeNode>

        </asp:TreeNode>

    </Nodes>

</asp:TreeView>
```

---

# 29. Complete Students.aspx Example

Use this combined page for the classroom demo:

```aspx
<%@ Page Language="C#" AutoEventWireup="true"
    CodeBehind="Students.aspx.cs"
    Inherits="StudentWebForms.Students" %>

<!DOCTYPE html>

<html>
<head runat="server">

    <title>Student Management</title>

    <style>

        body {
            font-family: Arial;
            margin: 30px;
        }

        .section {
            margin-bottom: 35px;
        }

        .grid {
            width: 100%;
        }

        .grid th {
            background: #eeeeee;
            padding: 10px;
        }

        .grid td {
            padding: 8px;
        }

    </style>

</head>

<body>

<form id="form1" runat="server">

    <div class="section">

        <asp:SiteMapPath
            ID="SiteMapPath1"
            runat="server">
        </asp:SiteMapPath>

    </div>

    <div class="section">

        <h2>Filter Students</h2>

        <asp:DropDownList
            ID="ddlCourse"
            runat="server"
            AutoPostBack="true"
            OnSelectedIndexChanged="ddlCourse_SelectedIndexChanged">
        </asp:DropDownList>

    </div>

    <div class="section">

        <h2>Students - GridView</h2>

        <asp:GridView
            ID="gvStudents"
            runat="server"
            CssClass="grid"
            AutoGenerateColumns="False"
            AllowSorting="true"
            AllowPaging="true"
            PageSize="4"
            OnSorting="gvStudents_Sorting"
            OnPageIndexChanging="gvStudents_PageIndexChanging">

            <Columns>

                <asp:BoundField
                    DataField="Id"
                    HeaderText="ID"
                    SortExpression="Id" />

                <asp:BoundField
                    DataField="Name"
                    HeaderText="Name"
                    SortExpression="Name" />

                <asp:BoundField
                    DataField="Course"
                    HeaderText="Course"
                    SortExpression="Course" />

                <asp:BoundField
                    DataField="City"
                    HeaderText="City"
                    SortExpression="City" />

                <asp:BoundField
                    DataField="Mark"
                    HeaderText="Mark"
                    SortExpression="Mark" />

            </Columns>

        </asp:GridView>

    </div>

    <div class="section">

        <h2>Students - ListView</h2>

        <asp:ListView
            ID="lvStudents"
            runat="server">

            <LayoutTemplate>

                <div id="itemPlaceholder"
                    runat="server">
                </div>

            </LayoutTemplate>

            <ItemTemplate>

                <div style="border:1px solid #ccc;
                            margin:10px;
                            padding:10px;">

                    <strong>
                        <%# Eval("Name") %>
                    </strong>

                    <br />

                    Course:
                    <%# Eval("Course") %>

                    <br />

                    City:
                    <%# Eval("City") %>

                    <br />

                    Mark:
                    <%# Eval("Mark") %>

                </div>

            </ItemTemplate>

        </asp:ListView>

    </div>

</form>

</body>
</html>
```

---

# 30. Complete Students.aspx.cs

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web.UI.WebControls;

namespace StudentWebForms
{
    public partial class Students : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
            if (!IsPostBack)
            {
                BindCourses();
                BindListView();
                BindGrid();
            }
        }

        private List<Student> GetStudents()
        {
            return new List<Student>
            {
                new Student
                {
                    Id = 1,
                    Name = "Arun",
                    Course = "ASP.NET",
                    City = "Pondicherry",
                    Mark = 85
                },

                new Student
                {
                    Id = 2,
                    Name = "Priya",
                    Course = "React",
                    City = "Chennai",
                    Mark = 91
                },

                new Student
                {
                    Id = 3,
                    Name = "Karthik",
                    Course = "AWS",
                    City = "Pondicherry",
                    Mark = 78
                },

                new Student
                {
                    Id = 4,
                    Name = "Divya",
                    Course = "Azure",
                    City = "Villupuram",
                    Mark = 88
                },

                new Student
                {
                    Id = 5,
                    Name = "Vijay",
                    Course = "ASP.NET",
                    City = "Cuddalore",
                    Mark = 72
                },

                new Student
                {
                    Id = 6,
                    Name = "Meena",
                    Course = "React",
                    City = "Chennai",
                    Mark = 95
                },

                new Student
                {
                    Id = 7,
                    Name = "Ravi",
                    Course = "AWS",
                    City = "Pondicherry",
                    Mark = 81
                },

                new Student
                {
                    Id = 8,
                    Name = "Anitha",
                    Course = "Azure",
                    City = "Chennai",
                    Mark = 89
                },

                new Student
                {
                    Id = 9,
                    Name = "Suresh",
                    Course = "ASP.NET",
                    City = "Pondicherry",
                    Mark = 67
                },

                new Student
                {
                    Id = 10,
                    Name = "Lakshmi",
                    Course = "React",
                    City = "Cuddalore",
                    Mark = 93
                }
            };
        }

        private void BindCourses()
        {
            ddlCourse.DataSource = new List<string>
            {
                "ASP.NET",
                "React",
                "AWS",
                "Azure"
            };

            ddlCourse.DataBind();

            ddlCourse.Items.Insert(
                0,
                new ListItem("-- Select Course --", "")
            );
        }

        private void BindListView()
        {
            lvStudents.DataSource = GetStudents();

            lvStudents.DataBind();
        }

        private void BindGrid()
        {
            gvStudents.DataSource = GetStudents();

            gvStudents.DataBind();
        }

        protected void ddlCourse_SelectedIndexChanged(
            object sender,
            EventArgs e)
        {
            string selectedCourse =
                ddlCourse.SelectedValue;

            var students = GetStudents();

            if (!string.IsNullOrEmpty(selectedCourse))
            {
                students = students
                    .Where(x =>
                        x.Course == selectedCourse)
                    .ToList();
            }

            gvStudents.DataSource = students;

            gvStudents.DataBind();
        }

        protected void gvStudents_Sorting(
            object sender,
            GridViewSortEventArgs e)
        {
            var students = GetStudents();

            if (e.SortExpression == "Name")
            {
                students = students
                    .OrderBy(x => x.Name)
                    .ToList();
            }

            if (e.SortExpression == "Mark")
            {
                students = students
                    .OrderByDescending(x => x.Mark)
                    .ToList();
            }

            if (e.SortExpression == "Course")
            {
                students = students
                    .OrderBy(x => x.Course)
                    .ToList();
            }

            if (e.SortExpression == "City")
            {
                students = students
                    .OrderBy(x => x.City)
                    .ToList();
            }

            gvStudents.DataSource = students;

            gvStudents.DataBind();
        }

        protected void gvStudents_PageIndexChanging(
            object sender,
            GridViewPageEventArgs e)
        {
            gvStudents.PageIndex =
                e.NewPageIndex;

            BindGrid();
        }
    }
}
```

---

# 31. Complete Code Flow

When the application starts:

```text
Browser
   |
   v
Students.aspx
   |
   v
Page_Load()
   |
   +---- BindCourses()
   |
   +---- BindListView()
   |
   +---- BindGrid()
   |
   v
Browser displays data
```

When the user selects a course:

```text
DropDownList
     |
     v
SelectedIndexChanged
     |
     v
SelectedValue
     |
     v
Where()
     |
     v
Filtered List
     |
     v
GridView
```

When the user clicks a GridView column:

```text
Column Header
     |
     v
Sorting Event
     |
     v
OrderBy()
     |
     v
DataSource
     |
     v
DataBind()
```

When the user clicks page 2:

```text
Page 2
  |
  v
PageIndexChanging
  |
  v
NewPageIndex
  |
  v
BindGrid()
  |
  v
Page 2 Records
```

---

# 32. Important Properties to Teach

## ListView

| Property | Purpose |
|---|---|
| ID | Identifies control |
| DataSource | Provides data |
| DataBind() | Performs binding |
| ItemTemplate | Controls item layout |
| LayoutTemplate | Controls overall layout |
| Eval() | Reads data field |

## DropDownList

| Property | Purpose |
|---|---|
| DataSource | Data source |
| DataBind() | Binds data |
| SelectedValue | Selected value |
| SelectedIndex | Selected position |
| AutoPostBack | Automatically posts page |
| SelectedIndexChanged | Selection event |

## GridView

| Property | Purpose |
|---|---|
| DataSource | Provides data |
| DataBind() | Binds records |
| AutoGenerateColumns | Automatic columns |
| AllowSorting | Enables sorting |
| AllowPaging | Enables paging |
| PageSize | Records per page |
| PageIndex | Current page |

---

# 33. Data Binding — Most Important Concept

Students should remember:

```csharp
Control.DataSource = data;
Control.DataBind();
```

Example:

```csharp
gvStudents.DataSource = GetStudents();
gvStudents.DataBind();
```

The two lines mean:

```text
Where is the data?
       ↓
DataSource

Now display it.
       ↓
DataBind()
```

---

# 34. DataBind() vs DataSource

### DataSource

Tells the control:

> "This is where your data comes from."

### DataBind()

Tells the control:

> "Read that data and generate the UI now."

Example:

```csharp
gvStudents.DataSource = students;
gvStudents.DataBind();
```

---

# 35. Why IsPostBack Is Important?

Without:

```csharp
if (!IsPostBack)
```

we may repeatedly reset controls.

Example:

```csharp
protected void Page_Load(object sender, EventArgs e)
{
    BindCourses();
}
```

Every postback can recreate the dropdown data.

Better:

```csharp
protected void Page_Load(object sender, EventArgs e)
{
    if (!IsPostBack)
    {
        BindCourses();
    }
}
```

Meaning:

```text
First page load → Bind

Button/event postback → Don't initialize again
```

---

# 36. Classroom Exercise 1 — ListView

Ask students to:

1. Add `Email` to Student.
2. Display Email in ListView.
3. Add a CSS class to each student card.
4. Display a different message when marks are above 80.

Example:

```aspx
<%# Convert.ToInt32(Eval("Mark")) >= 80
        ? "Excellent"
        : "Needs Improvement" %>
```

---

# 37. Classroom Exercise 2 — DropDownList

Ask students to:

1. Add `All Courses`.
2. Filter GridView.
3. Display the selected course.
4. Add another filter for City.

Example:

```text
Course
   ↓
ASP.NET

City
   ↓
Pondicherry

Result
   ↓
Only ASP.NET students from Pondicherry
```

---

# 38. Classroom Exercise 3 — GridView

Ask students to:

1. Add Email column.
2. Sort by Name.
3. Sort by Mark.
4. Change PageSize from 4 to 3.
5. Add City sorting.
6. Add a serial number column.

---

# 39. Classroom Exercise 4 — Navigation

Create:

```text
Home
Students
Courses
   ├── ASP.NET
   ├── React
   ├── AWS
   └── Azure
Contact
```

Use:

```text
Menu
SiteMapPath
TreeView
HyperLink
```

---

# 40. Real-World Architecture

In a real application, data normally comes from:

```text
SQL Server
     ↓
ADO.NET / Entity Framework
     ↓
C# Business Logic
     ↓
DataSource
     ↓
GridView / ListView
     ↓
Browser
```

Example:

```text
Students Table
-------------------------
Id
Name
Course
City
Mark
-------------------------
       ↓
SELECT query
       ↓
C# List<Student>
       ↓
GridView
```

---

# 41. Important Production Note

The classroom example uses:

```csharp
List<Student>
```

This is useful for learning controls.

In production applications, data normally comes from a database.

For example:

```sql
SELECT Id, Name, Course, City, Mark
FROM Students
ORDER BY Name;
```

Then the result is bound to the GridView.

For large datasets, filtering, sorting, and paging should preferably be performed at the database/query level rather than loading every record into memory.

---

# 42. Common Errors

## Error 1 — GridView is empty

Check:

```csharp
gvStudents.DataSource = GetStudents();
gvStudents.DataBind();
```

---

## Error 2 — Dropdown is empty

Check:

```csharp
ddlCourse.DataSource = courses;
ddlCourse.DataBind();
```

---

## Error 3 — Paging doesn't work

Check:

```aspx
AllowPaging="true"
```

and:

```csharp
gvStudents_PageIndexChanging
```

---

## Error 4 — Sorting doesn't work

Check:

```aspx
AllowSorting="true"
```

and:

```text
SortExpression
```

Example:

```aspx
SortExpression="Mark"
```

must match:

```csharp
e.SortExpression == "Mark"
```

---

## Error 5 — Data resets after postback

Check:

```csharp
if (!IsPostBack)
```

---

# 43. Quick Revision

### ListView

Used for:

```text
Custom repeated layouts
Cards
Lists
Product displays
Student cards
```

### DropDownList

Used for:

```text
Selection
Filtering
Category selection
Course selection
City selection
```

### GridView

Used for:

```text
Tabular data
Sorting
Paging
CRUD interfaces
Reports
```

### Menu

Used for:

```text
Application navigation
```

### SiteMapPath

Used for:

```text
Breadcrumb navigation
```

### TreeView

Used for:

```text
Hierarchical navigation
```

### HyperLink

Used for:

```text
Simple navigation
```

---

# 44. Interview Questions

## Q1. What is Data Binding?

Data binding connects a UI control to a data source.

---

## Q2. What is DataSource?

`DataSource` specifies the data that a control should use.

---

## Q3. What does DataBind() do?

It evaluates the data source and generates the control's UI.

---

## Q4. Difference between ListView and GridView?

`ListView` provides a highly customizable repeated layout.

`GridView` is designed primarily for tabular data.

---

## Q5. What is AllowPaging?

It enables paging in GridView.

---

## Q6. What is PageSize?

It specifies how many records appear on one page.

---

## Q7. What is SortExpression?

It identifies the field used for sorting.

---

## Q8. What is IsPostBack?

It tells us whether the page is being loaded for the first time or because of a postback.

---

## Q9. What is AutoPostBack?

It causes a control to send the page back to the server when its value changes.

---

## Q10. What is SiteMapPath?

It displays breadcrumb-style navigation based on the site's sitemap.

---

# 45. Final Student Task

Build a:

## Student Management Web Forms Application

Requirements:

```text
Home
 |
 +-- Students
 |      |
 |      +-- ListView
 |      +-- GridView
 |      +-- Search
 |      +-- Course Filter
 |      +-- Sorting
 |      +-- Paging
 |
 +-- Courses
 |
 +-- About
 |
 +-- Contact
```

Student fields:

```text
ID
Name
Email
Course
City
Mark
```

Required features:

```text
✓ ListView
✓ DropDownList
✓ Data Binding
✓ GridView
✓ GridView Sorting
✓ GridView Paging
✓ Course Filtering
✓ Menu
✓ SiteMapPath
✓ TreeView
✓ HyperLink
```

---

# 46. Teaching Sequence

Recommended classroom order:

### Part 1 — Concept

Explain:

```text
Control
   ↓
DataSource
   ↓
DataBind()
```

### Part 2 — ListView

Create:

```text
Student List
```

### Part 3 — DropDownList

Create:

```text
Course Filter
```

### Part 4 — GridView

Create:

```text
Student Table
```

### Part 5 — Sorting

Click:

```text
Name
Mark
Course
City
```

### Part 6 — Paging

Set:

```text
PageSize = 4
```

### Part 7 — Navigation

Demonstrate:

```text
Menu
SiteMapPath
TreeView
HyperLink
```

### Part 8 — Mini Project

Combine everything into:

```text
Student Management System
```

---

# 47. One-Minute Revision Diagram

```text
                 ASP.NET WEB FORMS
                         |
        +----------------+----------------+
        |                |                |
     ListView       DropDownList       GridView
        |                |                |
   Repeated UI       Selection         Table
        |                |                |
        |             Filtering       Sorting
        |                               |
        |                             Paging
        |                               |
        +---------------+---------------+
                        |
                   Data Binding
                        |
                DataSource + DataBind()
                        |
                   C# / Database
```

---

# 48. Key Code to Remember

Students should memorize this pattern:

```csharp
var data = GetStudents();

gvStudents.DataSource = data;

gvStudents.DataBind();
```

For ListView:

```csharp
lvStudents.DataSource = GetStudents();

lvStudents.DataBind();
```

For DropDownList:

```csharp
ddlCourse.DataSource = courses;

ddlCourse.DataBind();
```

For paging:

```csharp
gvStudents.PageIndex = e.NewPageIndex;

BindGrid();
```

For filtering:

```csharp
var result = students
    .Where(x => x.Course == selectedCourse)
    .ToList();
```

For sorting:

```csharp
var result = students
    .OrderBy(x => x.Name)
    .ToList();
```

---

# Conclusion

The main concept behind all these Web Forms controls is **data binding**.

Once students understand:

```text
Data Source
     ↓
Data Binding
     ↓
Control
     ↓
Event
     ↓
Updated Data
```

they can work with:

- ListView
- DropDownList
- GridView
- Sorting
- Paging
- Filtering
- Menu
- SiteMapPath
- TreeView
- HyperLink

This example provides a foundation for building a database-driven ASP.NET Web Forms Student Management System.
