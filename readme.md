###How to change elements styles of elements

**DxGrid**
<details>
  <summary>Hide vertical lines</summary>

  **v22.1**

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
  **v22.2**

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

  **Both v22.1 and v22.2**

  ```cs
<DxGrid Data="@forecasts" CssClass="my-grid">
    <Columns>
        <DxGridDataColumn Caption="Date" FieldName="Date" />
        <DxGridDataColumn Caption="Temperature" FieldName="TemperatureF" />
    </Columns>
</DxGrid>
  ```
</details> 
<details>
  <summary>Hide vertical lines</summary>

  **v22.1**

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
  **v22.2**

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

  **Both v22.1 and v22.2**

  ```cs
<DxGrid Data="@forecasts" CssClass="my-grid">
    <Columns>
        <DxGridDataColumn Caption="Date" FieldName="Date" />
        <DxGridDataColumn Caption="Temperature" FieldName="TemperatureF" />
    </Columns>
</DxGrid>
  ```
</details> 



**DxGrid**
 
