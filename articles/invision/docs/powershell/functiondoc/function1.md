
# Pb. (dot-sourcing)

PowerShell supports **dot-sourcing**. You can type a dot (**.**) followed by a **space**, followed by a **ps1** file name pointing to a script you want to include in the current script context. 


**InVision** supports its own dot-sourcing where you can type **Pb.** followed by a **space**, followed by the name of a **PowerShell** or **SQL** script defined in the solution. 

This will make code in another script available to you. This is useful if you want to call a function residing in another script.   
In SaaS solutions, the Solution Devs will not be able to put a file on any disk. This feature meets the need to dot-source a script without requiring access to disk. 

**Note**  
If a PowerShell Script and a SQL Script have the same name, you can specify the type after the comma (i.e. Pb. ‘MyScript, SQL’ or ‘MyScript, Pwsh’). Hence comma is not an allowed character in the names of scripts that are to be referred via Pb. 
<br/>
<br/>

**Example 1**  

**Embed PowerShell Script**

We need to import data from several Excel worksheets, so We create a PowerShell script that implements a generic import of Excel data.  
We call this script ``Excel Impl``. It has functions that do the work of bulk copying Excel Worksheet data into a SQL Server table.
 
Then we create a script that will be a function library that works as a layer between the implementation and the scripts that are going to use the functions.  
We call this script ``Excel Functions``. 

Lastly, we create the script that needs to import data from Excel worksheets into SQL server tables.  
We call this script ``Pb. Example Excel``.

<br/>

![dot-sourcing](https://profitbasedocs.blob.core.windows.net/images/function1a.png "dot-sourcing")

<br/>

``Excel Impl`` has the reference to SQL Server. 

<br/>

![dot-sourcing](https://profitbasedocs.blob.core.windows.net/images/function1b.png "dot-sourcing")

<br/>

It also contains code that will be used by the ``Excel Functions`` script.

<br/>

![dot-sourcing](https://profitbasedocs.blob.core.windows.net/images/function1c.png "dot-sourcing")

<br/>

``Excel Functions`` uses ``Pb. 'Excel Impl'`` to enable use of the ``Import-ExcelData`` function inside ``Excel Impl``

<br/>

![dot-sourcing](https://profitbasedocs.blob.core.windows.net/images/function1d.png "dot-sourcing")

<br/>

Finally We use ``Pb. Excel Functions`` to enable use of the ``PbImport-Excel`` function inside the ``Excel Functions``.
 
<br/>

![dot-sourcing](https://profitbasedocs.blob.core.windows.net/images/function1e.png "dot-sourcing")

<br/>
<br/>

**Example 2** 

**Embed SQL Script**



We have a SQL Script called ``SQL w macro``. 
<br/>

![dot-sourcing](https://profitbasedocs.blob.core.windows.net/images/function1a2.png "dot-sourcing")
<br/>

It has a macro defined.  
<br/>

![dot-sourcing](https://profitbasedocs.blob.core.windows.net/images/function1b2.png "dot-sourcing")
<br/>

We make a PowerShell script called ``Test macro``.  
In this script we add a reference to the NuGet Package *Microsoft.Data.SqlClient*.  
<br/>

![dot-sourcing](https://profitbasedocs.blob.core.windows.net/images/function1c2.png "dot-sourcing")

<br/>

We add the following script where I use ``Pb. 'SQL w macro'`` to embed the SQL Script called ``SQL w macro``. 
<br/>

![dot-sourcing](https://profitbasedocs.blob.core.windows.net/images/function1d2.png "dot-sourcing")