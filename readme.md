<a name="thetableofcontent"></a>
- [Guide to updating CSS styles used in previous versions to v22.2](#guide-to-updating-css-styles-used-in-previous-versions-to-v222)
  - [General information](#general-information)
  - [Affected components](#affected-components)
  - [How to use this document](#how-to-use-this-document)
  - [Most general requests based on users' requests](#most-general-requests-based-on-users-requests)
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

# Guide to updating CSS styles used in previous versions to v22.2

## General information

In v22.1, we encountered issues with the Bootstrap framework preventing us from further effective and quick development of our Blazor components. Among these issues were the following :
* The necessity to create a complex render to implement an UI element that is not present in the Bootstrap framework.
* The impossibility to create a lightweight component render because of the rigid structure of Bootstrap CSS classes.
* The possibility to get a broken layout after an upgrade to a new version of the Bootstrap framework

The led us to the need to develop our own rendering engine. In v22.2, we have implemented a new render engine for [certain components](#affected-components). The new render helped us implement [new features](https://community.devexpress.com/blogs/aspnet/archive/2022/10/13/blazor-upcoming-breaking-changes-in-rendering-and-bootstrap-support-v22-2.aspx) and lay the foundation for the easier and quicker development of components in future versions.

At the same time, this new rendering engine completely changes the internal visual structure of [certain Blazor](#affected-components) components. Users that used our previous CSS styles should change them to new CSS styles. 

You will find more information about the affected projects in the [Blazor — Upcoming Breaking Changes in Rendering and Bootstrap Support (v22.2)](https://community.devexpress.com/blogs/aspnet/archive/2022/10/13/blazor-upcoming-breaking-changes-in-rendering-and-bootstrap-support-v22-2.aspx).

This document is designed to help you change your previous CSS styles to new ones. If your project is affected by upgrading to v22.2, please start with the [How to use this document](#how-to-use-this-document) section that describes how to use this document.

[Return back to the table of content of the document.](#thetableofcontent)

## Affected components

Changes in rendering will directly affect the following components.

Major components:

* DxGrid
* DxScheduler (inner elements such as editors and buttons)
* DxRichEdit (toolbar elements and context menus)

Data Editors:
* DxCalendar
* DxCheckBox
* DxComboBox
* DxDateEdit
* DxListBox
* DxMaskedInput
* DxMemo
* DxSpinEdit
* DxTagBox
* DxTextBox
* DxTimeEdit

Layout:
* DxTabs
* DxFormLayout
* DxPopup
* DxDropDown
* DxFlyout

Navigation:
* DxPager
* DxToolbar
* DxAccordion
* DxContextMenu

Blazor components not listed above will continue to use our current Bootstrap rendering engine. As you might expect, you can continue to use these components as you did previously

[Return back to the table of content of the document.](https://github.com/mmakarova/StyleMigrating)

## How to use this document 

In the [Most general requests based on users' requests](#most-general-requests-based-on-users-requests) section of this repository, we summarized the most often cases when our users should use our private CSS selectors to apply a style to an element. 
 
Also, you can press Ctrl+F and search for a private CSS selector that you used in a previous version. This will help you find a selector that you used in v22.1 or prior, and copy the new equivalent of this selector.

If you didn't manage to find your scenario in this document, you can create a new CSS selector by inspecting a component render as described in the following articles: 

[View and change CSS](https://developer.chrome.com/docs/devtools/css/)<br/>
[How to implement CSS-related solutions for DevExpress components](https://supportcenter.devexpress.com/internal/ticket/details/T632424)

Feel free to write to our [Support Center](http://devexpress.com/support/center). We are ready to research your specific case.

[Return back to the table of content of the document.](https://github.com/mmakarova/StyleMigrating)

## Most general requests based on users' requests

### DxGrid

#### Hide vertical lines

In both v22.1 and v22.2, use the same razor code:

  ```cs
<DxGrid Data="@forecasts" CssClass="my-grid">
    <Columns>
        <DxGridDataColumn Caption="Date" FieldName="Date" />
        <DxGridDataColumn Caption="Temperature" FieldName="TemperatureF" />
    </Columns>
</DxGrid>
  ```
In v22.1, use the following CSS rules:

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
In v22.2, use the following CSS rules:

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
[Return back to the table of content of the document.](https://github.com/mmakarova/StyleMigrating)

#### Color alternate rows

In v22.2, use the following CSS rule:

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

Another option is to handle the CustomizeElement event:

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
[Return back to the table of content of the document.](https://github.com/mmakarova/StyleMigrating)

#### Place a scrollable DxGrid to DxPopup

In v22.2, use the following code:

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
[Return back to the table of content of the document.](https://github.com/mmakarova/StyleMigrating)

### DxToolbar

#### Center a toolbar item content

In both v22.1 and v22.2, use the same razor code

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
In v20.1, use the following CSS rules

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
In v22.2, use the following CSS rules

```css
    .dxbl-btn-group.dxbl-toolbar-group:nth-child(2) {
        width: 100%;
        display: flex;
        justify-content: center;
    }
```
[Return back to the table of content of the document.](https://github.com/mmakarova/StyleMigrating)

### DxScheduler

#### Change an edit form width

In v22.1, use the following CSS rules

```css
.dxbs-appointment-edit-dialog {
    width: 800px!important;
    max-width: 800px!important;
}
```
In v22.2, use the following CSS rules

```css
.dxbs-apt-edit-dialog {
    width: 800px!important;
    max-width: 800px!important;
}
```
[Return back to the table of content of the document.](https://github.com/mmakarova/StyleMigrating)

### DxEditors

#### DxDateEdit 

Internally, we use DxCalendar in the DxDateEdit popup. So, if you need to apply a style to DxCalendar of the DxDateEdit popup, you can write selectors for DxCalendar. To write a selector for a specific DxDateEdit, assign the DxDateEdit.DropDownCssClass property.

[Return back to the table of content of the document.](https://github.com/mmakarova/StyleMigrating)

##### Hide a a date picker button

In both v22.1 and v22.2, use the same razor code:
```cs
<DxDateEdit CssClass="my-editor-readonly" @bind-Date="@DateStart" ReadOnly="true" TimeSectionVisible="true" ></DxDateEdit>

@code {
    public DateTime DateStart { get; set; } = DateTime.Now;    
}
```

In 22.1, use the following CSS rule:

```css
.my-editor-readonly .dxbs-editor-input-container > input {
    display: none;
}
```
In v22.2, use the following CSS rule:

```css
.my-editor-readonly .dxbl-btn-group {
    display: none;
}

```
[Return back to the table of content of the document.](https://github.com/mmakarova/StyleMigrating)

##### Highlight a week on a mouse hover

In both v22.1 and v22.2, use the same razor code:
```cs
<DxDateEdit @bind-Date="@DateStart" DropDownCssClass="customDateEdit">
</DxDateEdit>

@code {
    public DateTime DateStart { get; set; } = DateTime.Now;    
}
```

In v22.1, use the following CSS rule:

```css
.customDateEdit .dxbs-calendar-table tr:hover {
    background-color: lightgray;
}
```
In v22.2, use the following CSS rule:

```css
.customDateEdit .dxbl-calendar-week-row:hover {
    background-color: lightgray;
}
```
[Return back to the table of content of the document.](https://github.com/mmakarova/StyleMigrating)

#### DxCalender

##### Change a font color of weekend

In both v22.1 and v22.2, use the same razor code:
```cs
<DxCalendar SelectedDate="@DateStart"  CssClass="myCalendarCss"></DxCalendar>

@code {
    public DateTime DateStart { get; set; } = DateTime.Now;
}

```

In 22.1, use the following CSS rule:

```css
.myCalendarCss .dxbs-weekend {
    color: black!important;
}

```
In v22.2, use the following CSS rule:

```css
.myCalendarCss .dxbl-calendar-day.dxbl-calendar-weekend {
    color: black;
}
```
[Return back to the table of content of the document.](https://github.com/mmakarova/StyleMigrating)

##### Hide a week number

In both v22.1 and v22.2, use the same razor code:
```cs
<DxCalendar SelectedDate="@DateStart"></DxCalendar>

@code {
    public DateTime DateStart { get; set; } = DateTime.Now;
}
```

In v19.2, use the following CSS rule:

```css
.dxbs-calendar td:not(.dxbs-day) {
    display: none;
}

```
In v22.2, use the following CSS rule:

```css
.dxbl-calendar-week-number {
    display: none;
}

```
[Return back to the table of content of the document.](https://github.com/mmakarova/StyleMigrating)

##### Hide a footer

In both v22.1 and v22.2, use the same razor code:
```cs
<DxCalendar SelectedDate="@DateStart" CssClass="customCalendar"></DxCalendar>

@code {
    public DateTime DateStart { get; set; } = DateTime.Now;
}
```

In v22.1, use the following CSS rule:

```css
.customCalendar .dxbs-calendar-footer {
    display: none;
}


```
In v22.2, use the following CSS rule:

```css
.customCalendar .dxbl-calendar-footer {
    display: none;
}
```
[Return back to the table of content of the document.](https://github.com/mmakarova/StyleMigrating)

##### Hide the Today button of a footer

In both v22.1 and v22.2, use the same razor code:
```cs
<DxCalendar SelectedDate="@DateStart" CssClass="customCalendar"></DxCalendar>

@code {
    public DateTime DateStart { get; set; } = DateTime.Now;
}
```

In v22.1, use the following CSS rule:

```css
.customCalendar .dxbs-calendar-footer > button:first-child {
    display: none;
}


```
In v22.2, use the following CSS rule:

```css
.customCalendar .dxbl-calendar-footer > button:first-child {
    display: none;
}
```
[Return back to the table of content of the document.](https://github.com/mmakarova/StyleMigrating)
