# Asp-Data-Access-Layer-Code-Generator
this is not reposity, but it could help you to auto generate Data access layer form database. it could be powerfull tools to auto generate Storedprocedure and classes.

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

If you used the tags -isvisible- and -iseditable- and use 'True' as IsAdmin value for StoredProcedure, you'll be able to view or edit all the fields with True or Flase value. But if you send the False value, you can only view or edit the fields that have the True field value.

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


2- Data Access Class : name is "tbCatDA"

		[ScaffoldColumn(false)] 
		[Display(AutoGenerateField = false)]
		[HiddenInput(DisplayValue = false)]
		[Range(0, int.MaxValue, ErrorMessage = "لطفاً عدد مناسب وارد نمایید")]
		public int  id; 
		[DisplayName("عنوان دسته خبری")]
		[Display(Name = "عنوان دسته خبری")]
		[StringLength(200, ErrorMessage="طول داده حداکثر 200 می باشد")]
		public string  nam;
		[DisplayName("آیا فعال است")]
		[Display(Name = "آیا فعال است")]
		public bool?   isActive;



		private SqlDataReader selectAll( bool isAdmin = false  , bool isJoin = false)
	

		private SqlDataReader selectAll(string ColumnName, object ColumnValue, bool isAdmin = false  , bool isJoin = false)
		

		private DataTable select(string SqlCommandText)
	

		private List<tbCatDA> ReadAllRows(SqlDataReader drc , bool IsJoin = false)
	

		private tbCatDA ReadOneRow(SqlDataReader drc)
		

		private List<tbCatDA> selectInternal(object id_value  , bool isAdmin = false  , bool isJoin = false)
		

		public List<tbCatDA> select(bool isAdmin = false  , bool isJoin = false)
		

		public tbCatDA selectByPK(object id_value  , bool isAdmin = false , bool isJoin = false )
		

		public string selectByPKPre(object id_value  , bool isAdmin = false , bool isJoin = false )
		

		public string selectByPKNext(object id_value  , bool isAdmin = false , bool isJoin = false )
		

		public tbCatDA selectLast(bool isJoin = false)
		

		public List<tbCatDA> selectByIndex(int StartIndex , int EndIndex , bool isAdmin = false , bool isJoin = false)
		

		public List<tbCatDA> selectSearchByIndex(int StartIndex , int EndIndex , object  id_value ,object  nam_value ,object  isActive_value , bool isAdmin = false , bool isJoin = false)
		

		public List<tbCatDA> selectSearchByPageNumber(int pagenumber,  int pageSize = 50, bool isAdmin = false , bool isJoin = false)
		

		public List<tbCatDA> select(string ColumnName , object id, bool isAdmin = false , bool isJoin = false)
		

		public List<tbCatDA> selectTop(int topCount = 0, bool isJoin = false, bool isAdmin = false )
		

		public List<tbCatDA> selectbyOrder(string ColumnName, bool Asc = true , int topCount = 0, bool isAdmin = false , bool isJoin = false)
		

		public List<tbCatDA> selectLike(string fieldName , string value, bool isAdmin = false , bool isJoin = false)
		

		public List<tbCatDA> selectLike(string ParentFieldName , string ParentFieldValue , string fieldName , string value, bool isAdmin = false , bool isJoin = false)
		

		public List<tbCatDA> selectNotLike(string fieldName , string value, bool isAdmin = false , bool isJoin = false)
		

		public List<tbCatDA> selectwhere(string whereQuery , bool isJoin = false)
		

		public object selectOneValue(String AggFunction,String ColumnName)
		

		public ReturnedData Insert()
		

		public ReturnedData Insert1_PropertyAll(object  nam_value ,object  isActive_value )
		


		public SqlCommand InsertCommand1_PropertyAll(object  nam_value ,object  isActive_value )
		

		public SqlCommand InsertCommand2_Command( string SqlCommandText, params object[] args)
		

		public void openConnectionForMultipleCommand()
		

		public void closeConnectionForMultipleCommand()
		

		public ReturnedData Update(bool isAdmin = false )
		

		public ReturnedData Update1_Property_All(object  id_value ,object  nam_value ,object  isActive_value , bool isAdmin = false )
		

		public ReturnedData UpdateIfNotNull(bool isAdmin = false )
		

		public ReturnedData Update1_Property_AllIfNotNull(object  id_value ,object  nam_value ,object  isActive_value , bool isAdmin = false )
		

		public ReturnedData UpdateOrInsert( bool isAdmin = false )
		

		public ReturnedData UpdateOrInsert_Property_All(object  id_value ,object  nam_value ,object  isActive_value , bool isAdmin = false )
		

		public ReturnedData Update2_PropertyValue_OneByPK(object id_value , string ColumnName , object value, bool isAdmin = false )
		

		public SqlCommand UpdateCommand2_CommandValues(string SqlCommandText, bool isAdmin = false , params object[] args)
		

		public ReturnedData Update3_Set_ColVal1_where_ColVal2(string ColumnName1 ,Object Value1 , string ColumnName2 , Object Value2, bool isAdmin = false )
		

		public ReturnedData Update4_CommandValues(string SqlCommandText,bool isAdmin = false  , params object[] args)
		

		public ReturnedData Update5_PropertyValue_PropertyValue_ByPK(object id_value , string ColumnName1 , object value1, string ColumnName2 , object value2, bool isAdmin = false )
		

		public SqlCommand UpdateCommand1_PropertyValueAll(object  id_value ,object  nam_value ,object  isActive_value , bool isAdmin = false )
		

		public SqlCommand UpdateCommand1_PropertyValueAllIfNotNull(object  id_value ,object  nam_value ,object  isActive_value , bool isAdmin = false )
		

		public SqlCommand UpdateOrInsert_PropertyValueAll(object  id_value ,object  nam_value ,object  isActive_value , bool isAdmin = false )
	

		public SqlCommand UpdateCommand3_PropertyValue_OneByPK(object id_value , string ColumnName , object value, bool isAdmin = false )
		

		public SqlCommand UpdateCommand4_PropertyValue_PropertyValue_ByPK(object id_value , string ColumnName1 , object value1, string ColumnName2 , object value2, bool isAdmin = false )
		

		public ReturnedData Increase(object id_value , string ColumnName , int count , bool isAdmin = false )
	

		public ReturnedData decrease(object id_value , string ColumnName , int count, bool isAdmin = false )
	
	
		public ReturnedData Delete1_Property_PK_AndIncColLowerThan(string ColName , int ColValue , object id_value )
		

		public ReturnedData Delete1_Property_PK_AndDecColGreateThan(string ColName , int ColValue , object id_value )
	

		public ReturnedData Delete1_Property_PK(object id_value , bool isAdmin = false )
		

		public ReturnedData Delete_All()
		

		public ReturnedData Delete_Where_Col_Value(string ColumnName , object value, bool isAdmin = false )
	

		public ReturnedData Delete2_Command(string SqlCommandText, bool isAdmin = false  , params object[] args)
		

		public int getLastIdentifyKey()
		

		public string GetTableName()

		public static long selectRowCount()
		

        private ReturnedData EcexuteUpdateQuery(SqlCommand Command)


