# Datasets API

The Datasets API provides access to in-memory datasets in a Workbook, meaning data that has been loaded to Worksheets, SQL Reports and Tables.

#### getNames()
Returns the names of all in-memory datasets in a Workbook. 

```javascript
const datasetNames = this.app.datasets.getNames();
for(const name of datasetNames){
    const rows = this.app.datasets.get(name);
    if(rows.length > 0){
        const firstRow = rows[0];
        if(Object.keys(firstRow).includes('Amount')){
            // Sum amounts
        }
    }
}
let sum = 0;
for(const row of rows){
    sum += row.Amount;
}
```

<br/>

#### get(datasetName: string)
Returns the named in-memory dataset as an array of objects. 

```javascript
const rows = this.app.datasets.get('Customers');
let sum = 0;
for(const row of rows){
    sum += row.Amount;
}
```