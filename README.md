This is the prompt that I used with Claude Code to generate the Python script:

I need to convert a series of documents from XML to JSON version. Every XML element in the source file correspond to a dictionary in the JSON file. Here's an example of a conversion from an XML element that has no children to the equivalent JSON dictionary:

Source version:

```xml
<toc-element toc-title="%General.Amelia.application% Getting Started" topic="A00-00_0001-Amelia-GS-Landing.md">
```

Target version:

```json
{
  "label": "%General.Amelia.application% Getting Started",
  "path": "A00-00_0001-Amelia-GS-Landing"
}
```

When the XML element has one or more children, there is an additional `children` key, the value of which is an array, with each element in the array being a JSON dictionary in the form that I previously described, if the XML child elements don't have any descendants. In the event that a child element has descendants, the process of starting an array begins again. The following example shows the conversion between a source with nesting to a target with nesting:

Source xml:

```xml
<toc-element toc-title="Block Library" topic="B03-04_0111-Flows-Block-Library.md">
    <toc-leement toc-title="%General.Amelia.application% Asks Block" topic="B03-04_0114-Amelia-Asks-Block.md"/>
    <toc-element toc-title="%General.Amelia.application% Says Block" topic="B03-04_0113-Amelia-Says-Block.md"/>
</toc-element>
```

Target JSON:

```
{
  "label": "Block Library",
  "path": "B03-04_0111-Flows-Block-Library"
  "children": [
    {
      "label": "%General.Amelia.application% Asks Block",
      "path": "B03-04_0114-Amelia-Asks-Block"
      },
    {
      "label": "%General.Amelia.application% Says Block",
      "path": "B03-04_0113-Amelia-Says-Block"
      },
    ]
}
```

The values of the `toc-title` elements in the source contain Markdoc variables, in many cases. For now, preserve the value of the `toc-title` elements exactly. Do not do any conversion. Treat the values as string literals. Also, as shown, in the conversion, take off the `.md` that terminates the path. The paths, in general, may be of the form `sub1/.../sub_n/filename`. Preserve this file structure when converting. Create a Python script that does this conversion. The script should take a command line argument for the file that it is converting, which will be of the form `<basename>.tree`. The script should create an output file with the name in the form of `<basename>.json`in the same folder as the source file.
