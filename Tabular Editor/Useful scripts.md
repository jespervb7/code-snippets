# Output all model objects (measures, tables, columns) for documentation purposes

```C#
// Construct a list of all visible columns and measures:
var objects = Model.AllMeasures.Where(m => !m.IsHidden && !m.Table.IsHidden).Cast<ITabularNamedObject>()
      .Concat(Model.AllColumns.Where(c => !c.IsHidden && !c.Table.IsHidden));

// Get their properties in TSV format (tabulator-separated):
var tsv = ExportProperties(objects,"Name,ObjectType,Parent,Description,FormatString,DataType,Expression");

// (Optional) Output to screen (can then be copy-pasted into Excel):
tsv.Output();
```

# Script to create DumpFilters Measure **WIP Doesn't work at the moment**

The following script creates the DumpFilters measure, the TLDR of it is that it shows all applied filters (useful as tooltip or other options. For more information checkout [this link by SQLBI](https://www.sqlbi.com/articles/displaying-filter-context-in-power-bi-tooltips/)

```C#
var dax = "VAR MaxFilters = 3 RETURN ";
var dumpFilterDax = @"IF (
    ISFILTERED ( {0} ), 
    VAR ___f = FILTERS ( {0} )
    VAR ___r = COUNTROWS ( ___f )
    VAR ___t = TOPN ( MaxFilters, ___f, {0} )
    VAR ___d = CONCATENATEX ( ___t, {0}, "", "" )
    VAR ___x = ""{0} = "" & ___d 
        & IF(___r > MaxFilters, "", ... ["" & ___r & "" items selected]"") & "" ""
    RETURN ___x & UNICHAR(13) & UNICHAR(10)
)";

// Loop through all columns of the model to construct the complete DAX expression:
bool first = true;
foreach(var column in Model.AllColumns)
{
    if(!first) dax += " & ";
    dax += string.Format(dumpFilterDax, column.DaxObjectFullName);
    if(first) first = false;
}

// Add the measure to the currently selected table:
Selected.Table.AddMeasure("DumpFilters", dax);

```

# Script to add the dax code to a measures description in the model

```C#
foreach (var m in Selected.Measures)
{
    int index = m.Description.IndexOf("Expression:");
    if (index >= 0)
    {
        m.Description = m.Description.Substring(0, index+11) + System.Environment.NewLine + m.Expression;
    }
    else
    {
        m.Description = m.Description + System.Environment.NewLine + System.Environment.NewLine + "Expression:" + System.Environment.NewLine + m.Expression;
    };
}; 
```
