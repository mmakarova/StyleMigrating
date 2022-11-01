- [Updating private CSS styles used in previous versions to v22.2](#updating-private-css-styles-used-in-previous-versions-to-v222)
  - [Reasons to move Blazor components to a new rendering mechanism](#reasons-to-move-blazor-components-to-a-new-rendering-mechanism)
  - [How to use this document to migrate your previous CSS styles to styles used in v22.2](#how-to-use-this-document-to-migrate-your-previous-css-styles-to-styles-used-in-v222)
  - [DxGrid](#dxgrid)
    - [Hide vertical lines](#hide-vertical-lines)
    - [Color alternate rows](#color-alternate-rows)
    - [Place a scrollable DxGrid to DxPopup](#place-a-scrollable-dxgrid-to-dxpopup)
  - [DxToolbar](#dxtoolbar)
    - [Center a toolbar item content](#center-a-toolbar-item-content)
  - [DxScheduler](#dxscheduler)
    - [Change an edit form width](#change-an-edit-form-width)
  - [DxEditors](#dxeditors)
    - [DxDateEdit](#dxdateedit)
      - [Hide a a date picker button](#hide-a-a-date-picker-button)
      - [Highlight a week on a mouse hover](#highlight-a-week-on-a-mouse-hover)
    - [DxCalender](#dxcalender)
      - [Change a font color of weekend](#change-a-font-color-of-weekend)
      - [Hide a week number](#hide-a-week-number)
      - [Hide a footer](#hide-a-footer)
      - [Hide the Today button of a footer](#hide-the-today-button-of-a-footer)

# Updating private CSS styles used in previous versions to v22.2

## Reasons to move Blazor components to a new rendering mechanism 

In v22.2, we moved to a new rendering engine of our Blazor components to avoid difficultes that we had using the Bootstrap framework:

* the necessity to create a complex render to implement an UI element that is not present in the Bootstrap framework
* the impossibility to create a lightweight component render because of a rigid structure of Bootstrap CSS classes
* the possibility to get a broken layout after upgrading a version of the Bootstrap framework

A new render helped us implement [new features](https://community.devexpress.com/blogs/aspnet/archive/2022/10/13/blazor-upcoming-breaking-changes-in-rendering-and-bootstrap-support-v22-2.aspx) in v22.2 and lay the foundation for the easier development of components in future versions. 

## How to use this document to migrate your previous CSS styles to styles used in v22.2
In the [Blazor — Upcoming Breaking Changes in Rendering and Bootstrap Support (v22.2)](https://community.devexpress.com/blogs/aspnet/archive/2022/10/13/blazor-upcoming-breaking-changes-in-rendering-and-bootstrap-support-v22-2.aspx) blog post, we described what projects can be affected by moving our components to the new render mechanism. 

Below we summurized the most often cases how our users modified our private CSS selectors to apply a style to an element. 
 
There are two ways to use this article:

* you can press Ctrl+F and search for a private CSS selector that you used in a previous version. This will help you find all selectors that you used in v22.1 and copy selectors that you need to use in v22.2.
* you can use this article content and find the necessary action in this article content.


If don't find the necessary selector, the general way to move your previous custom CSS selectors to a new version is to inspect our component render and create a new CSS selector. You can review two articles where an approach to inspect a page layout is described: 

[View and change CSS](https://developer.chrome.com/docs/devtools/css/)
[How to implement CSS-related solutions for DevExpress components](https://supportcenter.devexpress.com/internal/ticket/details/T632424)

If you don't manage to write the necessary selector, feel free to create a new ticket in [Support Center](http://devexpress.com/support/center). We will do our best to help you.

## DxGrid

### Hide vertical lines

**In both v22.1 and v22.2, use the same razor code:**

  ```cs
<DxGrid Data="@forecasts" CssClass="my-grid">
    <Columns>
        <DxGridDataColumn Caption="Date" FieldName="Date" />
        <DxGridDataColumn Caption="Temperature" FieldName="TemperatureF" />
    </Columns>
</DxGrid>
  ```
**In v22.1, use the following CSS rules:**

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
**In v22.2, use the following CSS rules:**

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

### Color alternate rows

**In v22.2, use the following CSS rule:**

```cs
<style>
    .my-grid .dxbl-grid-table > tbody > tr:nth-child(2n+1) {
        background-color: lightgray;
    }
</style>

<DxGrid Data="@forecasts" CssClass="my-grid">
    <Columns>
        <DxGridDataColumn Caption="Date" FieldName="Date" />
        <DxGridDataColumn Caption="Temperature" FieldName="TemperatureF" />
    </Columns>
</DxGrid>
```

**Another option is to handle the CustomizeElement event:**

```cs
<style>
    .highlighted-item > td {
        background-color: rgba(245, 198, 203, 0.5);
    }
</style>

<DxGrid Data="@forecasts" CssClass="my-grid" CustomizeElement="Grid_CustomizeElement">
    <Columns>
        <DxGridDataColumn Caption="Date" FieldName="Date" />
        <DxGridDataColumn Caption="Temperature" FieldName="TemperatureF" />
    </Columns>
</DxGrid>


@code {
    private WeatherForecast[] forecasts;
    protected override async Task OnInitializedAsync() {
        forecasts = await ForecastService.GetForecastAsync(DateTime.Now);
    }
    void Grid_CustomizeElement(GridCustomizeElementEventArgs e) {
        if (e.ElementType == GridElementType.DataRow) {
            e.CssClass = e.VisibleIndex % 2 == 0 ? "highlighted-item" : null;
        }

    }
}
```

### Place a scrollable DxGrid to DxPopup

**In v22.2, use the following code:**

```css
<style type="text/css">
    .mop-data-grid {
        height: 100%;
        width:100%
    }

    .popupContent {
        height: 800px;
    }
</style>


<DxPopup HeaderText="Adresse" BodyCssClass="popupContent" @bind-Visible="@isVisible">        
        <DxGrid Data="@forecasts" PageSize="@PageSize"
                CssClass="mop-data-grid">
            <Columns>
                <DxGridDataColumn Caption="Date" FieldName="Date" />
                <DxGridDataColumn Caption="Temperature" FieldName="TemperatureF" />
            </Columns>
        </DxGrid>    
</DxPopup>


@code {
    private WeatherForecast[]? forecasts;
    bool isVisible = true;

    public int PageSize { get; set; }

    protected override async Task OnInitializedAsync() {
        forecasts = await ForecastService.GetForecastAsync(DateTime.Now);
        PageSize = forecasts.Count();
    }
}
```

## DxToolbar

### Center a toolbar item content

**In both v22.1 and v22.2, use the same razor code**

  ```cs
<DxToolbar ItemRenderStyleMode="ToolbarRenderStyleMode.Contained">
    <Items>

        <DxToolbarItem Name="Add" IconCssClass="fa fa-plus" Text="Add New Program" Tooltip="Add New Program" />

        <DxToolbarItem CssClass="mycss" Name="ShowAllSwitch" BeginGroup="true" GroupName="MyTestGroup" RenderStyleMode="ToolbarItemRenderStyleMode.Plain">
            <Template>
                <div>
                    <DxCheckBox Checked="@ShowAll" CheckType="CheckType.Switch">Show All Programs</DxCheckBox>
                </div>
            </Template>
        </DxToolbarItem>

        <DxToolbarItem Name="SearchBox" Alignment="@ToolbarItemAlignment.Right" BeginGroup="true" RenderStyleMode="ToolbarItemRenderStyleMode.Contained">
            <Template>
                <DxTextBox @bind-Text="@SearchString"
                           ClearButtonDisplayMode="@DataEditorClearButtonDisplayMode.Auto"
                           NullText="Search Programs...">
                </DxTextBox>
            </Template>
        </DxToolbarItem>

    </Items>
</DxToolbar>
@code {
    public bool ShowAll { get; set; } = true;
    public string SearchString { get; set; } = "";
}
  ```
**In v20.1, use the following CSS rules**

```css
.btn-group.dxbs-toolbar-group:nth-child(2),
.btn-group.dxbs-toolbar-group:nth-child(2) > div {
    width: 100%;
}

    .btn-group.dxbs-toolbar-group:nth-child(2) .dx-checkbox {
        display: flex;
        align-items: center;
        justify-content: center;
    }
```
**In v22.2, use the following CSS rules**

```css
    .dxbl-btn-group.dxbl-toolbar-group:nth-child(2) {
        width: 100%;
        display: flex;
        justify-content: center;
    }
```

## DxScheduler

### Change an edit form width

**In v22.1, use the following CSS rules**

```css
.dxbs-appointment-edit-dialog {
    width: 800px!important;
    max-width: 800px!important;
}
```
**In v22.2, use the following CSS rules**

```css
.dxbs-apt-edit-dialog {
    width: 800px!important;
    max-width: 800px!important;
}
```

## DxEditors

### DxDateEdit 

Internally, we use DxCalendar in the DxDateEdit popup. So, if you need to apply a style to DxCalendar of the DxDateEdit popup, you can write selectors for DxCalendar. To write a selector for a specific DxDateEdit, assign the DxDateEdit.DropDownCssClass property.

#### Hide a a date picker button

**In both v22.1 and v22.2, use the same razor code:**
```cs
<DxDateEdit CssClass="my-editor-readonly" @bind-Date="@DateStart" ReadOnly="true" TimeSectionVisible="true" ></DxDateEdit>

@code {
    public DateTime DateStart { get; set; } = DateTime.Now;    
}
```

**In 22.1, use the following CSS rule:**

```css
.my-editor-readonly .dxbs-editor-input-container > input {
    display: none;
}
```
**In v22.2, use the following CSS rule:**

```css
.my-editor-readonly .dxbl-btn-group {
    display: none;
}

```

#### Highlight a week on a mouse hover

**In both v22.1 and v22.2, use the same razor code:**
```cs
<DxDateEdit @bind-Date="@DateStart" DropDownCssClass="customDateEdit">
</DxDateEdit>

@code {
    public DateTime DateStart { get; set; } = DateTime.Now;    
}
```

**In v22.1, use the following CSS rule:**

```css
.customDateEdit .dxbs-calendar-table tr:hover {
    background-color: lightgray;
}
```
**In v22.2, use the following CSS rule:**

```css
.customDateEdit .dxbl-calendar-week-row:hover {
    background-color: lightgray;
}
```

### DxCalender

#### Change a font color of weekend

**In both v22.1 and v22.2, use the same razor code:**
```cs
<DxCalendar SelectedDate="@DateStart"  CssClass="myCalendarCss"></DxCalendar>

@code {
    public DateTime DateStart { get; set; } = DateTime.Now;
}

```

**In 22.1, use the following CSS rule:**

```css
.myCalendarCss .dxbs-weekend {
    color: black!important;
}

```
**In v22.2, use the following CSS rule:**

```css
.myCalendarCss .dxbl-calendar-day.dxbl-calendar-weekend {
    color: black;
}
```

#### Hide a week number

**In both v22.1 and v22.2, use the same razor code:**
```cs
<DxCalendar SelectedDate="@DateStart"></DxCalendar>

@code {
    public DateTime DateStart { get; set; } = DateTime.Now;
}
```

**In v19.2, use the following CSS rule:**

```css
.dxbs-calendar td:not(.dxbs-day) {
    display: none;
}

```
**In v22.2, use the following CSS rule:**

```css
.dxbl-calendar-week-number {
    display: none;
}

```
#### Hide a footer

**In both v22.1 and v22.2, use the same razor code:**
```cs
<DxCalendar SelectedDate="@DateStart" CssClass="customCalendar"></DxCalendar>

@code {
    public DateTime DateStart { get; set; } = DateTime.Now;
}
```

**In v22.1, use the following CSS rule:**

```css
.customCalendar .dxbs-calendar-footer {
    display: none;
}


```
**In v22.2, use the following CSS rule:**

```css
.customCalendar .dxbl-calendar-footer {
    display: none;
}
```

#### Hide the Today button of a footer

**In both v22.1 and v22.2, use the same razor code:**
```cs
<DxCalendar SelectedDate="@DateStart" CssClass="customCalendar"></DxCalendar>

@code {
    public DateTime DateStart { get; set; } = DateTime.Now;
}
```

**In v22.1, use the following CSS rule:**

```css
.customCalendar .dxbs-calendar-footer > button:first-child {
    display: none;
}


```
**In v22.2, use the following CSS rule:**

```css
.customCalendar .dxbl-calendar-footer > button:first-child {
    display: none;
}
```

