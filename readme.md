- [How to change elements styles of elements](#how-to-change-elements-styles-of-elements)
  - [DxGrid](#dxgrid)
    - [Hide vertical lines:](#hide-vertical-lines)
    - [Hide horizontal lines:](#hide-horizontal-lines)

## How to change elements styles of elements
In v22.2, we changed the most of our components render. This was done for several reasons.

<a id="dxgrid"></a>
### DxGrid

<a id="dxgrid-hide-vertical-lines"></a>
#### Hide vertical lines:

**In both v22.1 and v22.2, use the same razor code**

  ```cs
<DxGrid Data="@forecasts" CssClass="my-grid">
    <Columns>
        <DxGridDataColumn Caption="Date" FieldName="Date" />
        <DxGridDataColumn Caption="Temperature" FieldName="TemperatureF" />
    </Columns>
</DxGrid>
  ```
**In v22.1, use the following CSS rules**

```css
.my-grid .table-bordered td {
    border-width: 0;
}
.my-grid > .card {
    border-left: 0px;
    border-right: 0px;
}
    .my-grid > .card .dxbs-grid-header {
        border-left: 0px;
    }
```
**In v22.2, use the following CSS rules**

```css
.my-grid {
    border-left: 0px;
    border-right: 0px;
}

    .my-grid.dxbl-grid .dxbl-grid-table > tbody > tr > td {
        border-left-width: 0px;
    }

    .my-grid.dxbl-grid .dxbl-grid-table > thead > tr > th {
        border-left-width: 0px;
    }

```

<a id="dxgrid-hide-horizontal-lines"></a>
#### Hide horizontal lines:

