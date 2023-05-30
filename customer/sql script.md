
# SQL script to recreate JSON data for dataSource.data


````

/*
Server: 
Database: PoswareDB
Run Script and copy result to update "Customer/dataSource.dat" on github repo "posware"
*/

USE PoswareDB

-- JSON String Not allow '"' qoute
UPDATE DatabaseIdentities SET NodeName = REPLACE(NodeName,'"',''),  DatabaseConnection = REPLACE(DatabaseConnection,'"','')

DECLARE @Table TABLE (RowNo int, Value varchar(1000))

INSERT @Table
SELECT ROW_NUMBER() OVER(ORDER BY LicenseID) RowNo, '"'+DatabaseID+'": {"LicenseID": '+Str(LicenseID)+', "NodeName": "'+NodeName+'", "ConnectionString": "'+DatabaseConnection+'" } ' Value FROM DatabaseIdentities

DECLARE @i int = 1
DECLARE @n int = (SELECT COUNT(*) FROM @Table)
DECLARE @Value varchar(1000)
DECLARE @JSON varchar(max) = '';
DECLARE @comma varchar(1) = ',';

WHILE @i <= @n 
BEGIN
	SET @Value = (SELECT Value FROM @Table WHERE RowNo = @i)
	SET @JSON = @JSON + @Value + IIF(@i < @n, @comma, '')
	SET @i = @i + 1
END

SELECT '{ '+@JSON+ '}' 


````



================== <br>

จากนั้น copy query result เพื่อไปใส่ใน body ของ https://localhost:7030/encrypText <br>
 
ใช้โปรแกรม Postman method POST ใส่ URL  "https://localhost:7030/encrypText" <br>
paste data  ลงในส่วนของ body กำหนดเป็น Text <br>

run แล้วเอา ผลลัพธ์ที่ได้ทำการ Encryt แล้วไปไว้ในไฟล์ "Customer/dataSource.dat" on github repo "posware" <br>
 
