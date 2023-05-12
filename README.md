# How to change the row text style when selecting a row in Flutter DataTable (SfDataGrid)

We have previously covered the support to changing the color of a selected row in [Flutter DataGrid](https://www.syncfusion.com/flutter-widgets/flutter-datagrid). In this article, we will now focus on modifying the text style of a row when it is selected.

## STEP 1:
The [DataGridRowAdapter](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/DataGridRowAdapter-class.html).buildRow() method is called whenever a row is selected. The data of the selected rows is stored in the dataGridController.selectedRows property. To modify the text color of the selected row, we need to check if the currently selected row exists in the dataGridController.selectedRows collection. If a match is found, we update the color accordingly. However, if the selected row is different from the current row, the color is set to null.

```dart
class EmployeeDataSource extends DataGridSource {
  @override
  DataGridRowAdapter? buildRow(DataGridRow row) {
    Color selectedRowTextColor = Colors.amber;

    return DataGridRowAdapter(
        cells: row.getCells().map<Widget>((dataGridCell) {
      return Container(
          alignment: Alignment.center,
          child: Text(
            dataGridCell.value.toString(),
            style: TextStyle(
                color: dataGridController.selectedRows.contains(row)
                    ? selectedRowTextColor
                    : null),
          ));
    }).toList());
  }
}

```
## STEP 2:
Initialize the [SfDataGrid](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/SfDataGrid-class.html) widget with all the required properties. To utilize the [DataGridController](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/DataGridController-class.html), we need to assign it to the [SfDataGrid.controller](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/SfDataGrid/controller.html) property.

```dart
import 'package:flutter/material.dart';
import 'package:syncfusion_flutter_datagrid/datagrid.dart';
import 'package:syncfusion_flutter_core/theme.dart';

DataGridController dataGridController = DataGridController();

class SfDataGridDemoState extends State<SfDataGridDemo> {
  late EmployeeDataSource _employeeDataSource;
  List<Employee> _employees = <Employee>[];

  @override
  void initState() {
    super.initState();
    _employees = getEmployeeData();
    _employeeDataSource = EmployeeDataSource(_employees);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Flutter SfDataGrid')),
      body: SfDataGridTheme(
        data: SfDataGridThemeData(selectionColor: Colors.purple),
        child: SfDataGrid(
            controller: dataGridController,
            source: _employeeDataSource,
            selectionMode: SelectionMode.multiple,
            columns: getColumns,
            columnWidthMode: ColumnWidthMode.fill),
      ),
    );
  }
}

```