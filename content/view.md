# View vs Store Procedure

### 1) What is View
```
In SQL, a view is a virtual table based on the result-set of an SQL statement. A view contains rows and columns, just like a real table. The fields in a view are fields from one or more real tables in the database. You can add SQL statements and functions to a view and present the data as if the data were coming from one single table.
```

### 2) Syntax
#### 2.1) Create
```
CREATE  OR ALTER  VIEW  schema_name.view_name
WITH <view_attribute>
AS select_statement   
[WITH CHECK OPTION]
```

View attributes:
* `ENCRYPTION`: Using this attribute prevents the view from being published as part of SQL Server replication
* `SCHEMABINDING`: Binds the view to the schema of the underlying table. We will use this one in another article when indexing a view
* `VIEW_METADATA`: Causes SQL Server to return to the DB-Library, ODBC, and OLE DB APIs the metadata information about the view

Examle:
```
CREATE VIEW EmployeeFullInfo
AS
SELECT EMP.[BusinessEntityID]
      ,EMP.[LoginID]
      ,EMP.[JobTitle]
      ,EMP.[BirthDate]
      ,EMP.[MaritalStatus]
      ,EMP.[Gender]
      ,EMP.[HireDate]
      ,Dep.Name AS Department
	  ,SH.Name AS ShiftName
	  ,EMPPayHist.Rate AS EmployeeRate
  FROM [SQLShackDemo].[HumanResources].[Employee] AS EMP
  JOIN [SQLShackDemo].[HumanResources].[EmployeeDepartmentHistory] AS EMPDepHist
  ON EMP.BusinessEntityID =EMPDepHist.BusinessEntityID 
  JOIN [SQLShackDemo].[HumanResources].[Department] AS Dep
  ON DEP.DepartmentID =EMPDepHist .DepartmentID 
  JOIN [SQLShackDemo].[HumanResources].[Shift] SH
  ON EMPDepHist.ShiftID=SH.ShiftID 
  JOIN [SQLShackDemo].[HumanResources].[EmployeePayHistory] EMPPayHist
  ON EMP.BusinessEntityID =EMPPayHist.BusinessEntityID 
```

django-database-view
element.style {
    display: block;
    white-space: pre-wrap;
    margin: 5px;
}
