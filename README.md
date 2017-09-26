This is an example dataset to demonstrate how data transforms works. In this example, we explain how filtering and applying formula can be done before dataset gets rendered in showcase page. It assumes publisher is already familiar with Data Packages and views specifications (`views` property in Data Package specifications).

### Transforming data

Data transforms are specified in `resources` attribute of `views` property. Each `resource` is an object that contains following attributes:

* `"name"` - name of the resource as a reference.
* `"transform"` - array of transforms. Each transform is an object, which properties vary depending on transform type.

### Filtering data

Under the graph on the top of this page, you can find a table that displays filtered data. Raw data is displayed in preview section. As you can see we are filtering by year - showing data for 2000 and onwards. This is described in the second view object of `views` property:

```
{
  "name": "table-view-from-2000",
  "specType": "table",
  "resources": [
    {
      "name": "global",
      "transform": [
        {
          "type": "filter",
          "expression": "(new Date(data['Year'])).getFullYear() >= 2000"
        }
      ]
    }
  ]
}
```

where `transform` property has:

* `"type": "filter"` - this way we define the transform to be a filter.
* `"expression"` - any JavaScript expression that evaluates to Boolean using some field(s) from the resource. E.g., in our example we are using `"Year"` field - converting it into JS date object then getting year by using `getFullYear()` method and filtering by larger or equal sign.

### Applying formula

Under the filtered data table, there is another table that displays raw data but with column "Gas Fuels" as a percentage of total figures. JSON representation of it can be found in the third view object:

```
{
  "name": "table-view-in-percentage",
  "specType": "table",
  "resources": [
    {
      "name": "global",
      "transform": [
        {
          "type": "formula",
          "expressions": [
            "data['Gas Fuel'] / data['Total'] * 100 + '%'"
          ],
          "asFields": ["Gas Fuel"]
        }
      ]
    }
  ]
}
```

where `transform` property has:

* `"type": "formula"` - defining transform type to be used.
* `"expressions"` - list of any JavaScript expressions that will be applied to specified data field.
* `"asFields"` - list of field names. This should be used according to list of expressions.

### Descriptor for this data package

This is the full `datapackage.json` of this dataset:

```
{
  "license": "ODC-PDDL",
  "name": "transform-examples-on-co2-fossil-global",
  "resources": [
    {
      "name": "global",
      "path": "global.csv",
      "format": "csv",
      "schema": {
        "fields": [
          {
            "description": "Year",
            "format": "any",
            "name": "Year",
            "type": "date"
          },
          {
            "description": "Total carbon emissions from fossil fuel consumption and cement production (million metric tons of C)",
            "name": "Total",
            "type": "number"
          },
          {
            "description": "Carbon emissions from gas fuel consumption",
            "name": "Gas Fuel",
            "type": "number"
          },
          {
            "description": "Carbon emissions from liquid fuel consumption",
            "name": "Liquid Fuel",
            "type": "number"
          },
          {
            "description": "Carbon emissions from solid fuel consumption",
            "name": "Solid Fuel",
            "type": "number"
          },
          {
            "description": "Carbon emissions from cement production",
            "name": "Cement",
            "type": "number"
          },
          {
            "description": "Carbon emissions from gas flaring",
            "name": "Gas Flaring",
            "type": "number"
          },
          {
            "description": "Per capita carbon emissions (metric tons of carbon; after 1949 only)",
            "name": "Per Capita",
            "type": "number"
          }
        ]
      }
    }
  ],
  "sources": [
    {
      "name": "CDIAC",
      "web": "http://cdiac.esd.ornl.gov/ftp/ndp030/CSV-FILES/global.1751_2010.csv"
    },
    {
      "name": "GitHub repo",
      "web": "https://github.com/datapackage-examples/transform-co2-fossil-global"
    }
  ],
  "title": "Data Transform examples on Global CO2 Emissions from Fossil Fuels since 1751",
  "views": [
    {
      "id": "graph",
      "label": "Graph",
      "state": {
        "graphType": "lines-and-points",
        "group": "Year",
        "series": [
          "Total",
          "Solid Fuel"
        ]
      },
      "type": "Graph"
    },
    {
      "name": "table-view-from-2000",
      "specType": "table",
      "resources": [
        {
          "name": "global",
          "transform": [
            {
              "type": "filter",
              "expression": "(new Date(data['Year'])).getFullYear() >= 2000"
            }
          ]
        }
      ]
    },
    {
      "name": "table-view-in-percentage",
      "specType": "table",
      "resources": [
        {
          "name": "global",
          "transform": [
            {
              "type": "formula",
              "expressions": [
                "data['Gas Fuel'] / data['Total'] * 100 + '%'"
              ],
              "asFields": ["Gas Fuel"]
            }
          ]
        }
      ]
    }
  ]
}
```
