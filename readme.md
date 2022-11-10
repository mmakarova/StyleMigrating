<a name="thetableofcontents"></a>
- [How to update CSS styles to v22.2](#how-to-update-css-styles-to-v222)
- [Most common customizations](#most-common-customizations)
  - [DxGrid](#dxgrid)
    - [Hide vertical lines](#hide-vertical-lines)
    - [Color alternate rows](#color-alternate-rows)
    - [Place a scrollable DxGrid into DxPopup](#place-a-scrollable-dxgrid-into-dxpopup)
  - [DxToolbar](#dxtoolbar)
    - [Center a toolbar item content](#center-a-toolbar-item-content)
  - [DxTabs](#dxtabs)
    - [Make tabs rounded](#make-tabs-rounded)
  - [DxScheduler](#dxscheduler)
    - [Change an edit form width](#change-an-edit-form-width)
  - [DxEditors](#dxeditors)
    - [DxComboBox](#dxcombobox)
      - [Modify the "No data to display" message](#modify-the-no-data-to-display-message)
    - [DxDateEdit](#dxdateedit)
      - [Hide a a date picker button](#hide-a-a-date-picker-button)
      - [Localize the Time section scroll picker's text](#localize-the-time-section-scroll-pickers-text)
      - [Highlight a week on a mouse hover](#highlight-a-week-on-a-mouse-hover)
    - [DxCalender](#dxcalender)
      - [Change a font color of weekend](#change-a-font-color-of-weekend)
      - [Hide a week number](#hide-a-week-number)
      - [Hide a footer](#hide-a-footer)
      - [Hide the Today button of a footer](#hide-the-today-button-of-a-footer)
  - [DxPopup](#dxpopup)
    - [Customize the Close header button's icon](#customize-the-close-header-buttons-icon)

# How to update CSS styles to v22.2

In the [Most common customizations](#most-common-customizations) section of this repository, we summarized the most often cases when our users should use our private CSS selectors to apply a style to an element. 
 
Also, you can press Ctrl+F and search for a private CSS selector that you used in a previous version. This will help you find a selector that you used in v22.1 or prior, and copy the new equivalent of this selector.

If you didn't manage to find your scenario in this document, you can create a new CSS selector by inspecting a component render as described in the following articles: 

[View and change CSS](https://developer.chrome.com/docs/devtools/css/)<br/>
[How to implement CSS-related solutions for DevExpress components](https://supportcenter.devexpress.com/internal/ticket/details/T632424)

Feel free to write to our [Support Center](http://devexpress.com/support/center). We are ready to research your specific case.

[Return to the table of contents.](#thetableofcontents)

# Most common customizations

## DxGrid

### Hide vertical lines

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
[Return to the table of contents.](#thetableofcontents)

### Color alternate rows

The universal approach to color alternate rows is to handle the CustomizeElement event:

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

At the samle time, to color DxGrid rows without the master-detail relationship and grouping. 
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

[Return to the table of contents.](#thetableofcontents)

### Place a scrollable DxGrid into DxPopup

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
[Return to the table of contents.](#thetableofcontents)

## DxToolbar

### Center a toolbar item content

In both v22.1 and v22.2, use the same razor code:

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
In v20.1, use the following CSS rules:

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
In v22.2, use the following CSS rules:

```css
    .dxbl-btn-group.dxbl-toolbar-group:nth-child(2) {
        width: 100%;
        display: flex;
        justify-content: center;
    }
```
[Return to the table of contents.](#thetableofcontents)

## DxTabs

### Make tabs rounded

In both v22.1 and v22.2, use the same razor code:
```cs
<DxTabs CssClass="MyTabsCss">
  <DxTab Text="Home" CssClass="MyTabCss"></DxTab>
  <DxTab Text="Products" CssClass="MyTabCss"></DxTab>
  <DxTab Text="Support" CssClass="MyTabCss"></DxTab>
</DxTabs>
```

In v22.1, use the follwing CSS rules:
```css
.MyTabsCss > ul {
        border-bottom-width: 0px;
    }
    .MyTabCss > a {
        border-radius: 20px !important;
        border: 1px solid white !important;
    }

        .MyTabCss > a:hover {
            border-color: #e9ecef !important;
        }

        .MyTabCss > a.active {
            border-color: dodgerblue !important;
            background-color: dodgerblue !important;
        }
```
In v22.2, use the follwing CSS rules:
```css
.MyTabsCss {
    border-bottom-width:0px;
}
.MyTabCss  {
    border-radius: 20px !important;
    border: 1px solid white !important;
}

    .MyTabCss:hover {
        border-color: #e9ecef !important;
    }

    .MyTabCss.dxbl-active {
        border-color: dodgerblue !important;
        background-color: dodgerblue !important;
    }
    .MyTabCss.dxbl-active:after {
        background-color:transparent!important;
    }
```
[Return to the table of contents.](#thetableofcontents)

## DxScheduler

### Change an edit form width

In v22.1, use the following CSS rules:

```css
.dxbs-appointment-edit-dialog {
    width: 800px!important;
    max-width: 800px!important;
}
```
In v22.2, use the following CSS rules:

```css
.dxbs-apt-edit-dialog {
    width: 800px!important;
    max-width: 800px!important;
}
```
[Return to the table of contents.](#thetableofcontents)

## DxEditors

### DxComboBox 

#### Modify the "No data to display" message

In v19.2 use the following code:

```cs
<style>
    .MyComboBox .dxbs-empty-data-row {
        color: transparent;
        width: 300px;
        user-select:none;
    }
.MyComboBox .dxbs-empty-data-row:before {
      position: absolute;
      content: 'This is my looooong text';
      color: #212529;
      font: inherit;
      width: 100%;
      left: 0;
}
</style>
<DxComboBox Data="@MyList" CssClass="MyComboBox"   FilteringMode="@DataGridFilteringMode.Contains"   AllowUserInput="true">
</DxComboBox>
```

In 22.1, use the following code:

```cs
 .dxbl-listbox-empty-data-item {
        color: transparent;
        width: 300px;
        user-select: none;
    }
    .dxbl-listbox-empty-data-item:before {
            position: absolute;
            content: 'Custom text';
            color: #212529;
            font: inherit;
            width: 100%;
            left: 0;
    }

<DxComboBox Data="@MyList"   FilteringMode="@DataGridFilteringMode.Contains"   AllowUserInput="true">
</DxComboBox>
```
[Return to the table of contents.](#thetableofcontents)

### DxDateEdit 

Internally, we use DxCalendar in the DxDateEdit popup. So, if you need to apply a style to DxCalendar of the DxDateEdit popup, you can write selectors for DxCalendar. To write a selector for a specific DxDateEdit, assign the DxDateEdit.DropDownCssClass property.

[Return to the table of contents.](#thetableofcontents)

#### Hide a a date picker button

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
[Return to the table of contents.](#thetableofcontents)

#### Localize the Time section scroll picker's text

In both v22.1 and v22.2, use the same razor code:

```cs
<DxDateEdit @bind-Date="@DateTimeValue"
            TimeSectionVisible="true" DropDownBodyCssClass="my-drop-down-body"   DropDownVisibleChanged="OnDropDownVisibleChanged"
            CssClass="cw-320" />

@code {
    DateTime DateTimeValue { get; set; } = DateTime.Now.AddHours(-7);
    void OnDropDownVisibleChanged(bool isVisible)
    {
        if (isVisible)
            JSRuntime.InvokeVoidAsync("updateRoller", "heure", "m", "seconde");
    }
}
```
In 22.1, use the following script:

```js
var dropDownBody, localizedHour, localizedMinute,localizedSecond;
function updateRoller(hour, minute, second) {
    localizedHour = hour;
    localizedMinute = minute;
    localizedSecond = second;
    setTimeout(function() {
        dropDownBody = document.querySelector(".my-drop-down-body");
        var tabHeader = dropDownBody.querySelector(".dxbs-date-time-edit-dropdown-tabs-time");
        tabHeader.onclick  = function() {
            setTimeout(function() {
                var captions = dropDownBody.querySelectorAll(".roller-title");
                captions[0].innerText = hour;
                captions[1].innerText = minute;
                captions[2].innerText = second;
            }, 300);
        }
    }, 700);
}
```

In v22.2, use the following script:

```js
var dropDownBody, localizedHour, localizedMinute,localizedSecond;
function updateRoller(hour, minute, second) {
    localizedHour = hour;
    localizedMinute = minute;
    localizedSecond = second;
    setTimeout(function() {
        dropDownBody = document.querySelector(".my-drop-down-body");
        var tabHeader = dropDownBody.querySelector(".dxbl-date-time-edit-tabs");
        tabHeader.onclick  = function() {
            setTimeout(function() {
                var captions = dropDownBody.querySelectorAll(".dxbl-roller-title");
                captions[0].innerText = hour;
                captions[1].innerText = minute;
                captions[2].innerText = second;
            }, 300);
        }
    }, 700);
}
```
[Return to the table of contents.](#thetableofcontents)

#### Highlight a week on a mouse hover

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
[Return to the table of contents.](#thetableofcontents)

### DxCalender

#### Change a font color of weekend

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
[Return to the table of contents.](#thetableofcontents)

#### Hide a week number

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
[Return to the table of contents.](#thetableofcontents)

#### Hide a footer

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
[Return to the table of contents.](#thetableofcontents)

#### Hide the Today button of a footer

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
[Return to the table of contents.](#thetableofcontents)

## DxPopup

### Customize the Close header button's icon

In both v22.1 and v22.2, use the same razor code:

```cs
<DxPopup HeaderText="Header" @bind-Visible="@PopupVisible" CssClass="customPopup">
    <Content>
       ...
    </Content>
</DxPopup>
```

In 22.1, use the following CSS rule:
```css
.customPopup .dxbs-popup-header-button{
    content: "icn";
    color: transparent;
    display: inline-block;
    background-image: url("/images/facebook_icon.svg");
    background-repeat: no-repeat;
    background-size: contain;
}
.customPopup .dxbs-popup-header-button svg {
    display: none;
}
```

In v22.2, use the following CSS rule:
```css
.customPopup .dxbl-popup-header-button .dxbl-image {
    content: "icn";
    color: transparent;
    display: inline-block;
    background-image: url("/images/facebook_icon.svg");
    background-repeat: no-repeat;
    background-size: contain;
}
```
[Return to the table of contents.](#thetableofcontents)
