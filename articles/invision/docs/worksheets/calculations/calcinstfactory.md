
# Calculation Instance Factory

You can use this feature when you need / want to use a custom JavaScript library to perform the spreadsheet calculations instead of using standard formulas. You would want to use this option over standard formulas when one or more of the following applies:

1.	In order to do the calculations, you need data that is not available in the data set of the spreadsheet, or in the data set of other spreadsheets loaded in the Workbook.

2.	You need to do fairly complex calculations with a lot of conditionals (if-else).

3.	You need to use math functions or custom logic that is not supported by the standard Eaze library.

4.	You want the calculation service to maintain state between executions (the calculation service executes whenever a cell value change).

5.	You want one instance of the calculation service pr spreadsheet.

## Setting up a Calculation Instance Factory

To set up a Calculation Instance Factory, click "Insert Configuration Template".  
This will create an Xml configuration like the one below:

    <ComputeInstanceFactory>   
    <Instance Name="my.calculator" FactoryFunction="myLib.createCalculator">
        <FactoryFunctionArguments>
            <SqlScript Id="sql script object id | @Object[sql script name].Id"/> 
            ...
        </FactoryFunctionArguments>
    </Instance>    
    </ComputeInstanceFactory>

*	``<Instance Name="calculation service instance name"/>`` specifies the name of the variable that the calculation service created by the FactoryFunction is assigned to. Use this name to call a function in your calculation service.

 For example, with ``<Instance Name="my.calculator"/>``, if the created calculation service has a function 'add', you can call the add function from a formula like this: 
 ```
 this.my.calculator.add(…)
 ```

*	``<Instance FactoryFunction="factory function name"/>`` specifies the name of the function called by InVision to create an instance of the calculation service. The factory function must be available on the (browser) window object, and it must return a new instance of a the calculation service. The factory function can take 0 or many arguments, which can be defined in the FactoryFunctionArguments section.

*	``<SqlScript id="id|@Object[<name>].Id"/>`` specifies a SQL Script in the Solution that returns some data. The SQL Scripts in the FactoryFunctionArguments section is passed as arguments to the  FactoryFunction in the order they are defined.
At the moment, only SQL Scripts that returns data are supported.  
You can specify the Id of the SQL Script either using the actual object id, or using the ``@Object[<name>].Id`` syntax.

## What is a Calculation Service

A calculation service is simply a JavaScript object that has the necessary business logic to perform some operation on data related to a spreadsheet. 

## What is the Factory Function

A factory function is a function that creates an instance of a calculation service and returns it to the spreadsheet calculation engine.

## Creating a Calculation Service and Factory Function

You have two options for creating a calculation service and factory function:

1.	Use a JavaScript Solution Object when you need small and / or simple calculation service.

2.	Develop, test and build using your preferred toolbox, for example TypeScript, Visual Studio Code, NodeJS, Jasmine, etc and then deploy to the /Scripts/plugins folder so that it's loaded to Workbooks. If you have dependencies on 3rd party libraries, such as requirejs, d3js etc, you need to edit the plugins.json file in the /Scripts/plugins folder and list all files in the order that they should be loaded in the browser. The contents of the file must have the following format:

        {
            "files" : ["3rdparty-lib1.js", "3rdparty-lib2", "mycalc-lib.js"]
        }

___

## Primary Operators

[Operator . (x.y)]()

Member access. 
**Example** 
```
"xyx".substring(…)
```

[Operator X?.y]()

Null conditional member access. Returns null if the left hand operand is null. 
**Example** 
```
X = null; 
Y = X?.substring(…)
```

___

## Unary operators


[Operator + (+x)]()

Returns the value of the operand. 

[Operator - (-x)]() 

Numeric negation. 
>**Example** 
```
Y = -X;
```


[Operator ! (!x)]()

Logical negation. 
>**Example** 
```
X = !true;(X becomes false)
```