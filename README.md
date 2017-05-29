This is an example Data Package to demonstrate how data transforms works. In this example, we explain how filtering and applying formula can be done before data package gets rendered in showcase page. It assumes publisher is already familiar with Data Packages and views specifications (`views` property in Data Package specifications).

### Transforming data

Data transforms are specified in `resources` property in `views`. Each resource is an object that contains following attributes:

* `"specType": "table"` - this way we define the view as a table (other options are `"simple"` (renders a graph and accepts Plotly spec) and `"vega"` (renders a graph and accepts Vega spec)).
* `"resources"` property is an array of objects in this case, where publishers can define data transforms they want to apply.
* `"name"` - name of the resource as a reference.
* `"transform"` - array of transforms. Each transform is an object, which properties vary depending on transform type. Only common property is `"type"` that is used to specify transform type.

### Filtering data

Under the graph on the top of this page, you can find a table that displays filtered data. Raw data is displayed in preview section. As you can see we are filtering by year - showing data for 2000 and onwards. This is described in the second view object of `views` property:

* `"type": "filter"` - this way we define the transform to be a filter.
* `"expression"` - any JavaScript expression that evaluates to Boolean using some field(s) from the resource. E.g., in our example we are using `"Year"` field - converting it into JS date object then getting year by using `getFullYear()` method and filtering by larger or equal sign.

### Applying formula

Under the filtered data table, there is another table that displays raw data but with column "Gas Fuels" as a percentage of total figures. JSON representation of it can be found in the third view object:

* `"type": "formula"` - defining transform type to be used.
* `"expressions"` - list of any JavaScript expressions that will be applied to specified data field.
* `"asFields"` - list of field names. This should be used according to list of expressions.

### Descriptor for this data package

{{ dp.json }}
