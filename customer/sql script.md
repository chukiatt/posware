
# SQL script to recreate JSON data for dataSource.data



/*
Server: .. ไม่บอกหรอก .. <br>
Database: PoswareDB <br>
Run Script and copy result to update "Customer/dataSource.dat" on github repo "posware" <br>
*/

USE PoswareDB

DECLARE @Table TABLE (RowNo int, Value varchar(1000))

INSERT @Table
SELECT ROW_NUMBER() OVER(ORDER BY LicenseID) RowNo, '"'+DatabaseID+'": {"LicenseID": '+Str(LicenseID)+', "NodeName": "'+NodeName+'", "ConnectionString": "'+DatabaseConnection+'" }, ' Value FROM DatabaseIdentities

DECLARE @i int = 1
DECLARE @n int = (SELECT COUNT(*) FROM @Table)
DECLARE @Value varchar(1000)
DECLARE @JSON varchar(max) = '';

WHILE @i < @n 
BEGIN
	SET @Value = (SELECT Value FROM @Table WHERE RowNo = @i)
	SET @JSON = @JSON + @Value
	SET @i = @i + 1
END

SELECT '{ '+@JSON+ '}' 





