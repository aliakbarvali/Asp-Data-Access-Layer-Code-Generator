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

If you want to use a direct method to update the value of a specific field based on the primary key, you can use the -set- tag
=> "Name of Category#-set-"

If you want to consider a field as the foreign key to the users table and validate it (access) based on it, you can use the -auth- tag.

=> "Name of Category#-auth-"

-set- and -auth- together

=> "Name of Category#-auth--set-"

You can also use the -isvisible- and -iseditable- tags for fields. These are fields to determine the type of record access.

If you used the tags -isvisible- and -iseditable- and use 'True' as IsAdmin value for Stored Applications, you'll be able to view or edit all the fields with True of Flase value. But if you send the False value, you can only view or edit the fields that have the True field value.

Sample Database Table With Generated Stored Procedure and Class:

Table: tbCat

id	int	Unchecked

nam	nvarchar(200)	Checked

isActive	bit	Checked


1 - StoredProcedure



PROCEDURE dbo.spv_tbCat_Insert
PROCEDURE dbo.spv_tbCat_Delete_byPK
PROCEDURE dbo.spv_tbCat_Delete_byPK_AndDecColGreateThan
PROCEDURE dbo.spv_tbCat_Delete_byPK_AndIncColLowerThan
PROCEDURE dbo.spv_tbCat_Update_byPK
PROCEDURE dbo.spv_tbCat_UpdateIfNotNull_byPK
PROCEDURE dbo.spv_tbCat_selectLastID
PROCEDURE dbo.spv_tbCat_select_All
PROCEDURE dbo.spv_tbCat_select_All_WithJoin
PROCEDURE dbo.spv_tbCat_selectTop_All
PROCEDURE dbo.spv_tbCat_selectTop_All_WithJoin
PROCEDURE dbo.spv_tbCat_select_Between2Index
PROCEDURE dbo.spv_tbCat_select_Between2Index_WithJoin
PROCEDURE dbo.spv_tbCat_select_Search
PROCEDURE dbo.spv_tbCat_select_Search_WithJoin
PROCEDURE dbo.spv_tbCat_select_ByPageNumber
PROCEDURE dbo.spv_tbCat_select_ByPageNumber_WithJoin
PROCEDURE dbo.spv_tbCat_select_ByPKPre
PROCEDURE dbo.spv_tbCat_select_ByPK
PROCEDURE dbo.spv_tbCat_select_ByPK_WithJoin
PROCEDURE dbo.spv_tbCat_select_ByPKNext
PROCEDURE dbo.spv_tbCat_selectLast
PROCEDURE dbo.spv_tbCat_selectLast_WithJoin
PROCEDURE dbo.spv_tbCat_select_ByColumn
PROCEDURE dbo.spv_tbCat_select_ByColumn_WithJoin
PROCEDURE dbo.spv_tbCat_Update_Increase_byPK
PROCEDURE dbo.spv_tbCat_Update_Decrease_byPK
PROCEDURE dbo.spv_tbCat_UpdateOrInsert_byPK
PROCEDURE dbo.spv_tbCat_select_RowCount


