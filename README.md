# Asp-Data-Access-Layer-Code-Generator
this is not reposity, but it could help you to auto generate Data access layer form database. it could be powerfull tools to auto generate Storedprocedure and classes
List of Generated File:
1. StoredProcedure.sql

2. Models (Folder): This folder contains All model classes

  2.1 DA  (Folder): This folder contains basic model classes with 'TableName'+'DA' File Name and 'TableName' + 'DA' partial class name. Every class contains base property and methods
  
  2.2 DAP  (Folder): This folder contains model classes with 'TableName'+'DAP' File Name and 'TableName' + 'DA' partial class name. Every class contains property display name and server side required attribiute
  
  2.3 DAP_Methods  (Folder): This folder contains model classes with 'TableName'+'DAP_Methods' File Name and 'TableName' + 'DA' partial class name. Every class contains sample CUID Query and methods for new custom methods.

Foreach property of class (or column of table) the "DisplayName" and other Required Attribute gots from Properties of Column.
for example:
the "DisplayName" attribute gets from Description of column and if description was empty it gots from column name.
and Required attribute gots from "AllowNull" of column
and MaxLen attribute gots from Len of column properties

To add more features, Some tags have been added to the description section.
for example:
If you want to use a separate and direct method to change the bit value of a specific field from the table based on the primary key, just use the -change- tag. Like this
in Description Section: use name that you like then add '#' the add tag name (-change-)
=> "Name of Category#-change-"
If you want to use a direct method to register the value of a field based on the primary key, you can use the -set-tag
If you want to consider a field as the external key to the users table and validate it (access) based on it, you can use the -auth-tag.
You can also use the - and - tags for fields. These are fields to determine the type of record performance.
If you use the tags-and-you-and-used Stored Applications, you'll be able to view all the fields correctly as the administrator. But if you send the wrong value, you can only view or edit the fields that have the correct field value.

Sample Database Table With Generated Stored Procedure and Class:

Table: tbCat
id	int	Unchecked
nam	nvarchar(200)	Checked
isActive	bit	Checked

1 - StoredProcedure
USE [DBNews]
GO
/* ----------------------------------Insert-----------------------------------------  */
/* Insert StoredProcedure : inser record and return inserted.id*/
IF ( OBJECT_ID('dbo.spv_tbCat_Insert') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_Insert
GO
CREATE PROCEDURE dbo.spv_tbCat_Insert  
	 @nam     nvarchar      (200)  = NULL
		,@isActive     bit       = 1
		         
AS 
BEGIN 
  SET NOCOUNT OFF 

	declare @IdentityOutput table ( ID int )
  INSERT INTO [dbo].[tbCat]
  ( 
	  [nam] 
			,[isActive] 
			         
  ) 
  output inserted.id into @IdentityOutput(ID)
  VALUES 
  ( 
			@nam  
			,@isActive  
			          
  ) 
	select ID from @IdentityOutput		
 IF @@ROWCOUNT < 1
 BEGIN
  RAISERROR ('An error occured',10,1)
  RETURN -1
  END
END 
GO
/* ------------------------------------------------------------------------------------  */

/* ----------------------------------Delete-----------------------------------------  */
IF ( OBJECT_ID('dbo.spv_tbCat_Delete_byPK') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_Delete_byPK
GO
CREATE PROCEDURE dbo.spv_tbCat_Delete_byPK
    @id  int     
	  ,
	@isAdmin_inner bit 
	 
AS 
BEGIN 
  SET NOCOUNT OFF 

  DELETE [dbo].[tbCat]
  FROM   [dbo].[tbCat]
  WHERE  
		id = @id 
		 and (@isAdmin_inner = 1 or ( 1=1 ))

 IF @@ROWCOUNT < 1
 BEGIN
  RAISERROR ('An error occured',10,1)
  RETURN -1
  END
END
GO

/* ------------------------------------------------------------------------------------  */

/* ----------------------------------Delete and Update Column Grater than-----------------------------------------  */
IF ( OBJECT_ID('dbo.spv_tbCat_Delete_byPK_AndDecColGreateThan') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_Delete_byPK_AndDecColGreateThan
GO
CREATE PROCEDURE dbo.spv_tbCat_Delete_byPK_AndDecColGreateThan	
	@ColName varchar(255)	= N'1',
	@ColValue 	VARCHAR(300) = '1',
    @id  int     
	   ,
	@isAdmin_inner bit 
	       
AS 
BEGIN TRAN
  SET NOCOUNT OFF 
	  	DECLARE @sql nvarchar(max) = 'UPDATE  [dbo].[tbCat] SET [' +  @ColName +'] -= 1  where  [' +  @ColName +'] > ' + @ColValue ;
		exec sp_executesql  @sql
		
		DELETE [dbo].[tbCat]
		FROM   [dbo].[tbCat]
		WHERE  
		id = @id 
		
	
 IF @@ROWCOUNT < 1
 BEGIN
 ROLLBACK  
  RAISERROR ('An error occured',10,1)
  RETURN -1
  END
  COMMIT TRANSACTION   
GO

/* ------------------------------------------------------------------------------------  */

/* ----------------------------------Delete and Update Column Lower than-----------------------------------------  */
IF ( OBJECT_ID('dbo.spv_tbCat_Delete_byPK_AndIncColLowerThan') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_Delete_byPK_AndIncColLowerThan
GO
CREATE PROCEDURE dbo.spv_tbCat_Delete_byPK_AndIncColLowerThan	
	@ColName varchar(255)	= N'1',
	@ColValue 	VARCHAR(300) = '1',
    @id  int     
	 ,
	@isAdmin_inner bit 
	        
AS 
BEGIN TRAN
  SET NOCOUNT OFF 
	  	DECLARE @sql nvarchar(max) = 'UPDATE  [dbo].[tbCat] SET [' +  @ColName +'] -= 1  where  [' +  @ColName +'] < ' + @ColValue ;
		exec sp_executesql  @sql
		
		DELETE [dbo].[tbCat]
		FROM   [dbo].[tbCat]
		WHERE  
		id = @id 
		
	
 IF @@ROWCOUNT < 1
 BEGIN
 ROLLBACK  
  RAISERROR ('An error occured',10,1)
  RETURN -1
  END
  COMMIT TRANSACTION   
GO

/* ------------------------------------------------------------------------------------  */

/* ----------------------------------update-----------------------------------------  */

IF ( OBJECT_ID('dbo.spv_tbCat_Update_byPK') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_Update_byPK
GO
CREATE PROCEDURE dbo.spv_tbCat_Update_byPK
	 @id     int     
		,@nam     nvarchar     (200)  = NULL
		,@isActive     bit      = 1
		      ,
	@isAdmin_inner bit 
	 
AS
BEGIN 
  SET NOCOUNT OFF 
   UPDATE [dbo].[tbCat]
   SET
			 [nam] =  @nam 
		,[isActive] =  @isActive 
		  
   FROM   [dbo].[tbCat]
   WHERE  
   [id] = @id 
		  and (@isAdmin_inner = 1 or ( 1=1 ))      

 IF @@ROWCOUNT < 1
 BEGIN
  RAISERROR ('An error occured',10,1)
  RETURN -1
  END
END
GO

/* ------------------------------------------------------------------------------------  */

/* ----------------------------------updateIfNotNull-----------------------------------------  */

IF ( OBJECT_ID('dbo.spv_tbCat_UpdateIfNotNull_byPK') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_UpdateIfNotNull_byPK
GO
CREATE PROCEDURE dbo.spv_tbCat_UpdateIfNotNull_byPK
	 @id     int     
		,@nam     nvarchar     (200)  = NULL
		,@isActive     bit      = 1
		    ,
	@isAdmin_inner bit 
	   
AS
BEGIN 
  SET NOCOUNT OFF 
   UPDATE [dbo].[tbCat]
   SET
			 [nam] = ISNULL( @nam , [nam]) 
		,[isActive] = ISNULL( @isActive , [isActive]) 
		  
   FROM   [dbo].[tbCat]
   WHERE  
   [id] = @id 
		 and (@isAdmin_inner = 1 or ( 1=1 ))      

 IF @@ROWCOUNT < 1
 BEGIN
  RAISERROR ('An error occured',10,1)
  RETURN -1
  END
END 
GO

/* ------------------------------------------------------------------------------------  */

/* ----------------------------------select-GetLastID-----------------------------------------  */

IF ( OBJECT_ID('dbo.spv_tbCat_selectLastID') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_selectLastID
GO
CREATE PROCEDURE dbo.spv_tbCat_selectLastID 
AS
BEGIN 
  SET NOCOUNT OFF 
		select IDENT_CURRENT ('tbCat')
END
GO
/* ------------------------------------------------------------------------------------  */

/* ----------------------------------select-ALL-----------------------------------------  */

IF ( OBJECT_ID('dbo.spv_tbCat_select_All') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_select_All
GO
CREATE PROCEDURE dbo.spv_tbCat_select_All 
	@isAdmin_inner bit 
	 
AS
BEGIN 
  SET NOCOUNT OFF 
		select * From [dbo].[tbCat]  (NOLOCK) where (@isAdmin_inner = 1 or ( 1=1 ))  
END
GO

/* ------------------------------------------------------------------------------------  */
/* ----------------------------------select-ALL-_WithJoin -----------------------------------------  */

IF ( OBJECT_ID('dbo.spv_tbCat_select_All_WithJoin') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_select_All_WithJoin
GO
CREATE PROCEDURE dbo.spv_tbCat_select_All_WithJoin 
	@isAdmin_inner bit 
	 
AS
BEGIN 
  SET NOCOUNT OFF 
		select [dbo].[tbCat].*  From [dbo].[tbCat]   where (@isAdmin_inner = 1 or ( 1=1 ))  
END
GO

/* ------------------------------------------------------------------------------------  */
/* ----------------------------------select-Top-All-----------------------------------------  */

IF ( OBJECT_ID('dbo.spv_tbCat_selectTop_All') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_selectTop_All
GO
CREATE PROCEDURE dbo.spv_tbCat_selectTop_All 
	@TopCount 	int = 100,
	@isAdmin_inner bit 
	 
AS
BEGIN 
  SET NOCOUNT OFF 
		select Top(@TopCount) * From [dbo].[tbCat]  (NOLOCK)  where (@isAdmin_inner = 1 or ( 1=1 ))  
END
GO

/* ------------------------------------------------------------------------------------  */ 
/* ----------------------------------select-Top-All with Join-----------------------------------------  */

IF ( OBJECT_ID('dbo.spv_tbCat_selectTop_All_WithJoin') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_selectTop_All_WithJoin
GO
CREATE PROCEDURE dbo.spv_tbCat_selectTop_All_WithJoin
	@TopCount 	int = 100,
	@isAdmin_inner bit 
	 
AS
BEGIN 
  SET NOCOUNT OFF 
		select Top(@TopCount) [dbo].[tbCat].*  From [dbo].[tbCat]     where (@isAdmin_inner = 1 or ( 1=1 ))  
END
GO

/* ------------------------------------------------------------------------------------  */ 

/*-----------------------------------select Between 2  Index----------------------*/
IF ( OBJECT_ID('dbo.spv_tbCat_select_Between2Index') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_select_Between2Index
GO
CREATE PROCEDURE dbo.spv_tbCat_select_Between2Index 
	@StartIndex int	= 1,
	@EndIndex 	int = 100,
	@isAdmin_inner bit 
	 

AS
BEGIN  
		SET NOCOUNT OFF 
		select * FROM (
             select * , ROW_NUMBER() OVER(ORDER BY [tbCat].[id]   ) AS NUMBER
                     FROM [dbo].[tbCat] where (@isAdmin_inner = 1 or ( 1=1 ))
               ) AS TBL
		WHERE NUMBER BETWEEN @StartIndex and @EndIndex /**/
END
GO 

/* ------------------------------------------------------------------------------------  */ 
/*-----------------------------------select Between 2  Index _WithJoin----------------------*/
IF ( OBJECT_ID('dbo.spv_tbCat_select_Between2Index_WithJoin') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_select_Between2Index_WithJoin
GO
CREATE PROCEDURE dbo.spv_tbCat_select_Between2Index_WithJoin 
	@StartIndex int	= 1,
	@EndIndex 	int = 100,
	@isAdmin_inner bit 
	 

AS
BEGIN  
		SET NOCOUNT OFF 
		select * FROM (
             select [dbo].[tbCat].*  , ROW_NUMBER() OVER(ORDER BY [tbCat].[id]   ) AS NUMBER
                     FROM [dbo].[tbCat]  where (@isAdmin_inner = 1 or ( 1=1 ))
               ) AS TBL
		WHERE NUMBER BETWEEN @StartIndex and @EndIndex /**/
END
GO 

/* ------------------------------------------------------------------------------------  */ 
/*-----------------------------------select Search----------------------*/
IF ( OBJECT_ID('dbo.spv_tbCat_select_Search') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_select_Search
GO
CREATE PROCEDURE dbo.spv_tbCat_select_Search 
		@StartIndex int	= 1,
		@EndIndex 	int = 100,
		@id     int     = NULL
		,@nam     nvarchar    (200)  = NULL
		,@isActive     bit     = NULL
		    ,
	@isAdmin_inner bit 
	 

AS
BEGIN  
		SET NOCOUNT OFF 
 
		select * FROM (
            select * , ROW_NUMBER() OVER(ORDER BY [tbCat].[id]   ) AS NUMBER
                     FROM [dbo].[tbCat]
			where
				(@id IS NULL OR [tbCat].[id] = @id)
		 and (@nam IS NULL OR REPLACE(REPLACE(REPLACE([tbCat].[nam], ' ', ' '),  NCHAR(1610),NCHAR(1740)),  NCHAR(1603),NCHAR(1705)) LIKE '%' + REPLACE(REPLACE(REPLACE(@nam, ' ', ' '),  NCHAR(1610),NCHAR(1740)),  NCHAR(1603),NCHAR(1705)) + '%')
		 and (@isActive IS NULL OR [tbCat].[isActive] = @isActive)
		  and (@isAdmin_inner = 1 or ( 1=1 ))
            ) AS TBL
		WHERE NUMBER BETWEEN @StartIndex and @EndIndex /**/
END
GO 

/* ------------------------------------------------------------------------------------  */
/*-----------------------------------select Search_WithJoin----------------------*/
IF ( OBJECT_ID('dbo.spv_tbCat_select_Search_WithJoin') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_select_Search_WithJoin
GO
CREATE PROCEDURE dbo.spv_tbCat_select_Search_WithJoin 
		@StartIndex int	= 1,
		@EndIndex 	int = 100,
		@id     int     = NULL
		,@nam     nvarchar    (200)  = NULL
		,@isActive     bit     = NULL
		    ,
	@isAdmin_inner bit 
	 

AS
BEGIN  
		SET NOCOUNT OFF 
 
		select * FROM (
            select [dbo].[tbCat].*  , ROW_NUMBER() OVER(ORDER BY [tbCat].[id]   ) AS NUMBER
                     FROM [dbo].[tbCat] 
			where
				(@id IS NULL OR [tbCat].[id] = @id)
		 and (@nam IS NULL OR REPLACE(REPLACE(REPLACE([tbCat].[nam], ' ', ' '),  NCHAR(1610),NCHAR(1740)),  NCHAR(1603),NCHAR(1705)) LIKE '%' + REPLACE(REPLACE(REPLACE(@nam, ' ', ' '),  NCHAR(1610),NCHAR(1740)),  NCHAR(1603),NCHAR(1705)) + '%')
		 and (@isActive IS NULL OR [tbCat].[isActive] = @isActive)
		  and (@isAdmin_inner = 1 or ( 1=1 ))
            ) AS TBL
		WHERE NUMBER BETWEEN @StartIndex and @EndIndex /**/
END
GO 

/* ------------------------------------------------------------------------------------  */
  
/*-----------------------------------select Search By PageNumber----------------------*/
IF ( OBJECT_ID('dbo.spv_tbCat_select_ByPageNumber') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_select_ByPageNumber
GO
CREATE PROCEDURE dbo.spv_tbCat_select_ByPageNumber
	@pagenumber int = 0,
	@pagesize int = 50,
	@isAdmin_inner bit 
	 
AS
BEGIN  
		SET NOCOUNT OFF 
 
		select top (@pagesize) * FROM (
             select *, ROW_NUMBER() OVER(ORDER BY [tbCat].[id]    ASC) as line, total = count(*) over()
			 from tbCat  where (@isAdmin_inner = 1 or ( 1=1 )) ) AS TBL
		where line > (@pagenumber - 1) * @pagesize  /**/
END
GO 

/* ------------------------------------------------------------------------------------  */ 
/*-----------------------------------select Search By PageNumber_WithJoin----------------------*/
IF ( OBJECT_ID('dbo.spv_tbCat_select_ByPageNumber_WithJoin') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_select_ByPageNumber_WithJoin
GO
CREATE PROCEDURE dbo.spv_tbCat_select_ByPageNumber_WithJoin
	@pagenumber int = 0,
	@pagesize int = 50,
	@isAdmin_inner bit 
	 
AS
BEGIN  
		SET NOCOUNT OFF 
 
		select top (@pagesize) * FROM (
             select [dbo].[tbCat].* , ROW_NUMBER() OVER(ORDER BY [tbCat].[id]    ASC) as line, total = count(*) over()
			 from tbCat  where (@isAdmin_inner = 1 or ( 1=1 )) ) AS TBL
		where line > (@pagenumber - 1) * @pagesize  /**/
END
GO 

/* ------------------------------------------------------------------------------------  */ 
/* ----------------------------------select-ByPK-----------------------------------------  */

IF ( OBJECT_ID('dbo.spv_tbCat_select_ByPK') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_select_ByPK
GO
CREATE PROCEDURE dbo.spv_tbCat_select_ByPK 
	 @id  int    
		 ,
	@isAdmin_inner bit 
	   
AS
BEGIN 
  SET NOCOUNT OFF 
		select * From [dbo].[tbCat]   (NOLOCK) 
		where [tbCat].[id] = @id 
		 and    (@isAdmin_inner = 1 or ( 1=1 ))
END
GO
/* ------------------------------------------------------------------------------------  */ 

/* ----------------------------------select-ByPKPre-----------------------------------------  */

IF ( OBJECT_ID('dbo.spv_tbCat_select_ByPKPre') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_select_ByPKPre
GO
CREATE PROCEDURE dbo.spv_tbCat_select_ByPKPre 
	 @id  int    
		 ,
	@isAdmin_inner bit 
	   
AS
BEGIN 
  SET NOCOUNT OFF  
		select prev_word  from (select  lag([tbCat].[id]   ) over (order by [tbCat].[id]   ) as prev_word , [tbCat].[id]    from [tbCat]) as t 
		where t.id = @id 
		 
END
GO
/* ------------------------------------------------------------------------------------  */ 

/* ----------------------------------select-ByPKNext-----------------------------------------  */

IF ( OBJECT_ID('dbo.spv_tbCat_select_ByPKNext') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_select_ByPKNext
GO
CREATE PROCEDURE dbo.spv_tbCat_select_ByPKNext
	 @id  int    
		 ,
	@isAdmin_inner bit 
	   
AS
BEGIN 
  SET NOCOUNT OFF  
		select prev_word  from (select  lead([tbCat].[id]   ) over (order by [tbCat].[id]   ) as prev_word , [tbCat].[id]    from [tbCat]) as t 
		where t.id = @id 
		 
END
GO
/* ------------------------------------------------------------------------------------  */ 
/* ----------------------------------select-ByPK_WithJoin-----------------------------------------  */

IF ( OBJECT_ID('dbo.spv_tbCat_select_ByPK_WithJoin') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_select_ByPK_WithJoin
GO
CREATE PROCEDURE dbo.spv_tbCat_select_ByPK_WithJoin 
	@id  int    
		 , 
	@isAdmin_inner bit 
	   
AS
BEGIN 
  SET NOCOUNT OFF 
		select [dbo].[tbCat].*  From [dbo].[tbCat]   
		where [tbCat].[id] = @id 
		  and  (@isAdmin_inner = 1 or ( 1=1 ))
END
GO
/* ------------------------------------------------------------------------------------  */ 
/* ----------------------------------selectLast-----------------------------------------  */

IF ( OBJECT_ID('dbo.spv_tbCat_selectLast') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_selectLast
GO
CREATE PROCEDURE dbo.spv_tbCat_selectLast   
AS
BEGIN 
  SET NOCOUNT OFF 
		select TOP 1 * From [dbo].[tbCat]   (NOLOCK) 
		
END
GO
/* ------------------------------------------------------------------------------------  */
/* ----------------------------------selectLast _WithJoin-----------------------------------------  */

IF ( OBJECT_ID('dbo.spv_tbCat_selectLast_WithJoin') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_selectLast_WithJoin
GO
CREATE PROCEDURE dbo.spv_tbCat_selectLast_WithJoin  
AS
BEGIN 
  SET NOCOUNT OFF 
		select TOP 1 [dbo].[tbCat].*  From [dbo].[tbCat]    
		
END
GO
/* ------------------------------------------------------------------------------------  */



/* ----------------------------------select-ByColumn-----------------------------------------  */

IF ( OBJECT_ID('dbo.spv_tbCat_select_ByColumn') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_select_ByColumn
GO
CREATE PROCEDURE dbo.spv_tbCat_select_ByColumn 
	  @ColumnName 	varchar(255)
		,@ColumnValue	varchar(255),
	@isAdmin_inner bit 
	   
AS
BEGIN 
  SET NOCOUNT OFF 
	  	DECLARE @sql nvarchar(max) = 'select * FROM   [dbo].[tbCat]   (NOLOCK) where     ' +  @ColumnName +' = ''' +@ColumnValue + ' ''   ';
		exec sp_executesql  @sql
END
GO
/* ------------------------------------------------------------------------------------  */ 
/* ----------------------------------select-ByColumn_WithJoin-----------------------------------------  */

IF ( OBJECT_ID('dbo.spv_tbCat_select_ByColumn_WithJoin') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_select_ByColumn_WithJoin
GO
CREATE PROCEDURE dbo.spv_tbCat_select_ByColumn_WithJoin 
	  @ColumnName 	varchar(255)
		,@ColumnValue	varchar(255),
	@isAdmin_inner bit 
	   
AS
BEGIN 
  SET NOCOUNT OFF 
	  	DECLARE @sql nvarchar(max) = 'select [dbo].[tbCat].*  From [dbo].[tbCat]    where     ' +  @ColumnName +' = ''' +@ColumnValue + ' ''   ';
		exec sp_executesql  @sql
END
GO
/* ------------------------------------------------------------------------------------  */ 

/* ----------------------------------Increase-----------------------------------------  */

IF ( OBJECT_ID('dbo.spv_tbCat_Update_Increase_byPK') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_Update_Increase_byPK
GO
CREATE PROCEDURE dbo.spv_tbCat_Update_Increase_byPK
	  @ColumnName 	varchar(255),
	  @IncDecCount VARCHAR(300),
	@id  VARCHAR(300) 
		   ,
	@isAdmin_inner bit 
	 
AS
BEGIN 
  SET NOCOUNT OFF 
  
	  	DECLARE @sql nvarchar(max) = 'UPDATE [dbo].[tbCat]   set [' +  @ColumnName +'] += ' + @IncDecCount + 'where ' +  ' [id] = ' + @id   
		  ;
		exec sp_executesql  @sql
   
       

 IF @@ROWCOUNT < 1
 BEGIN
  RAISERROR ('An error occured',10,1)
  RETURN -1
  END
END
GO

/* ------------------------------------------------------------------------------------  */
/* ----------------------------------Decrease-----------------------------------------  */

IF ( OBJECT_ID('dbo.spv_tbCat_Update_Decrease_byPK') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_Update_Decrease_byPK
GO
CREATE PROCEDURE dbo.spv_tbCat_Update_Decrease_byPK
	  @ColumnName 	varchar(255),
	  @IncDecCount VARCHAR(300),
	@id  VARCHAR(300) 
		  ,
	@isAdmin_inner bit 
	 
AS
BEGIN 
  SET NOCOUNT OFF 
  
	  	DECLARE @sql nvarchar(max) = 'UPDATE [dbo].[tbCat]   set [' +  @ColumnName +'] -= ' + @IncDecCount + 'where ' +  ' [id] = ' + @id   
		  ;
		exec sp_executesql  @sql
   
       

 IF @@ROWCOUNT < 1
 BEGIN
  RAISERROR ('An error occured',10,1)
  RETURN -1
  END
END
GO

/* ------------------------------------------------------------------------------------  */



/* ----------------------------------update Or Insert-----------------------------------------  */

IF ( OBJECT_ID('dbo.spv_tbCat_UpdateOrInsert_byPK') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_UpdateOrInsert_byPK
GO
CREATE PROCEDURE dbo.spv_tbCat_UpdateOrInsert_byPK
	 @id     int     
		,@nam     nvarchar     (200)  = NULL
		,@isActive     bit      = 1
		       ,
	@isAdmin_inner bit 
	    
AS
BEGIN 
  SET NOCOUNT OFF 
   UPDATE [dbo].[tbCat]
   SET
			 [nam] =  @nam 
		,[isActive] =  @isActive 
		  
   FROM   [dbo].[tbCat]
   WHERE  
   [id] = @id 
		   and (@isAdmin_inner = 1 or ( 1=1 ))       
IF (@@ROWCOUNT = 1)
BEGIN
		  RAISERROR ('Succesed',0,0)
		  RETURN 1
END

ELSE IF(@@ROWCOUNT=0)
	BEGIN
			declare @IdentityOutput table ( ID int )
			INSERT INTO [dbo].[tbCat]
			( 
			  [nam] 
			,[isActive] 
			         
			) 
			output inserted.id into @IdentityOutput(ID)
			VALUES 
			( 
					@nam  
			,@isActive  
			          
			) 
			select ID from @IdentityOutput

		 IF (@@ROWCOUNT < 1)
			BEGIN
			  RAISERROR ('An error occured',10,1)
			  RETURN -1
			END
	END
ELSE IF (@@ROWCOUNT < 1)
	BEGIN
		  RAISERROR ('An error occured',10,1)
		  RETURN -1
	END
END
GO
/* ------------------------------------------------------------------------------------  */

/* ----------------------------------select-RowCount-----------------------------------------  */

IF ( OBJECT_ID('dbo.spv_tbCat_select_RowCount') IS NOT NULL ) 
   DROP PROCEDURE dbo.spv_tbCat_select_RowCount
GO
CREATE PROCEDURE dbo.spv_tbCat_select_RowCount 
AS
BEGIN 
  SET NOCOUNT OFF 
		select  row_count From sys.dm_db_partition_stats Where Object_Name(Object_Id) = 'tbCat' 
END
GO
/* -------------------------------------Methods For (Update By ColName)   (Update Change Status)  (selectBy PK)---------------------   */

/* ----------------------------------------------------------------------------------------------------------------------------------  */


