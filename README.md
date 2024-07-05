# fl_heatmap

A heatmap widget for Flutter apps.

![heatmap electricity consumption](https://user-images.githubusercontent.com/13302336/151945676-a5d81296-ef46-4067-9ee5-4c40b6d69e78.png)

![heatmap water consumption](https://user-images.githubusercontent.com/13302336/152556319-d89cfdf4-71bd-4d34-92a2-9975f6548e8a.png)

## Usage

### 1 - Depend on it

Add it to your package's pubspec.yaml file

```yml
dependencies:
  fl_heatmap: ^0.1.0
```


### 2 - Install it

Install packages from the command line

```sh
flutter packages get
```

### 3 - Use it

This is an example for the months of four years:
```dart
import 'package:fl_heatmap/fl_heatmap.dart';

class _ExampleState extends State<ExampleApp> {
  HeatmapItem? selectedItem;

  late HeatmapData heatmapData;

  @override
  void initState() {
    _initExampleData();
    super.initState();
  }

  void _initExampleData() {
    const rows = [
      '2022',
      '2021',
      '2020',
      '2019',
    ];
    const columns = [
      'Jan',
      'Feb',
      'Mär',
      'Apr',
      'Mai',
      'Jun',
      'Jul',
      'Aug',
      'Sep',
      'Okt',
      'Nov',
      'Dez',
    ];
    final r = Random();
    const String unit = 'kWh';
    heatmapData = HeatmapData(rows: rows, columns: columns, items: [
      for (int row = 0; row < rows.length; row++)
        for (int col = 0; col < columns.length; col++)
          HeatmapItem(
              value: r.nextDouble() * 6,
              unit: unit,
              xAxisLabel: columns[col],
              yAxisLabel: rows[row]),
    ]);
  }

  @override
  Widget build(BuildContext context) {
    final title = selectedItem != null
        ? '${selectedItem!.value.toStringAsFixed(2)} ${selectedItem!.unit}'
        : '--- ${heatmapData.items.first.unit}';
    final subtitle = selectedItem != null
        ? '${selectedItem!.xAxisLabel} ${selectedItem!.yAxisLabel}'
        : '---';
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Heatmap plugin example app'),
        ),
        body: SingleChildScrollView(
          child: Column(
            children: [
              const SizedBox(height: 16),
              Text(title, textScaleFactor: 1.4),
              Text(subtitle),
              const SizedBox(height: 8),
              Heatmap(
                  onItemSelectedListener: (HeatmapItem? selectedItem) {
                    debugPrint(
                        'Item ${selectedItem?.yAxisLabel}/${selectedItem?.xAxisLabel} with value ${selectedItem?.value} selected');
                    setState(() {
                      this.selectedItem = selectedItem;
                    });
                  },
                  rowsVisible: 5, // Only the first 5 rows are visible
                  heatmapData: heatmapData)
            ],
          ),
        ),
      ),
    );
  }
}
```

If necessary you can inherit from `HeatmapItem` and attach some payload, e.g. the electricity costs for the consumption:

```dart
class CustomHeatmapItem extends HeatmapItem {
  CustomHeatmapItem(
      {required this.costs,
      required double value,
      required String unit,
      required String xAxisLabel,
      required String yAxisLabel})
      : super(
            value: value,
            unit: unit,
            xAxisLabel: xAxisLabel,
            yAxisLabel: yAxisLabel);
  
  final double costs;
}
```

## Features

* Supporting custom color schemes with dynamic size or use predefined color palettes 
  such as `colorPaletteTemperature`, `colorPaletteRed`, `colorPaletteBlue`.
* x- and y-axis labels are completely dynamic
* Do not show the x-axis/y-axis labels if necessary 
* Detect clicks on cells and get back the data item to show detailed information about the cell
* Use different styles for the cells if necessary
* Show only the first lines and user can request to show all

## Apps using this plugin

| [EHW+](https://ehwplus.web.app) | Your app? |
|---|---|
| ![EHW+](https://play-lh.googleusercontent.com/TDPCEwTmGEootK5VnPLu94AZ4Ks4-eTa_sg9IoLqWOk_aBr-OxjkfKe2s0OkvLgWKNc_=w100-rw)| ... |