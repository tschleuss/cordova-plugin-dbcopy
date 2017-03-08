cordova-plugin-dbcopy
=====================

Add a prepopulated SQLite database in your Phonegap/Cordova Android and iOS app.

### Note

The database file may have extensions or not for e.g the db file name would be sample.db or sample.sqlite or sample. It doesn't matter what is the file extension, just remember to use the whole filename with extensions(if having one otherwise not) as a paramter when passing to the plugin methods.

### Database file location

The database initial location for both platforms has been now changed to www folder. Now you have to put your database file inside www folder.

### Installation

Plugin can be install with simple cordova plugin add command -

    cordova plugin add https://github.com/an-rahulpandey/cordova-plugin-dbcopy.git


### Plugin Response Success and Error

Now you will get a proper plugin response in case of any errors or success.

Response format

```javascript 
{
  message : "message contains the response string like Invalid DB Location or DB Doesn't Exists or Db Copied Successfully",
  code: integer value such as 404, 200, 516
}
```
 
 Code -

``` 
 404 - DB or Source or Destination Doesn't exists, see message string.
 516 - DB Already Exists.
 200 - Called Method Executed Successfully.
```

### Methods

Currently there are two methods supported by the plugin.

**Copy**
=========================================

This Method allows you the copy the database from www directory.
```javascript 
    window.plugins.sqlDB.copy(dbname, location, success,error);
```
  Here -

   **dbname** -> Is the name of the database you want to copy. The dbname can be filename (without extensions) or filename.db or filename.sqlite. The plugin will look for and copy the file according to the filename provided here. And the same file name should be used while opening the database via [SQLitePlugin](https://github.com/litehelpers/Cordova-sqlite-storage).

   **location** -> You can pass three integer arguments here (Use 0 for Android)-

 ```javascript 
      (for ios only)
      location = 0; // It will copy the database in the default SQLite Database directory. This is the default location for database
      or
      location = 1; // If set will copy the database to Library folder instead of Documents folder.
      or
      location = 2; // (Disable iCloud Backup) If set will copy the database to Library/LocalDatabase. The database will not be synced by the iCloud Backup.
```

   **success** -> function will be called if the db is copied sucessfully.

   **error** -> function will be called if the there is some problem in copying the db or the file already exists on the location.

**Copy Database from Device Storage**
===============================================

This is an untested version. Let me know if you have any suggestions. Also Pull Request are always welcome.
```javascript 
    window.plugins.sqlDB.copyDbFromStorage(dbname, location, source, success, error);
```
 Here - 
 
   **dbname** -> Is the name of the database you want to copy. The dbname can be filename (without extensions) or filename.db or filename.sqlite. The plugin will look for and copy the file according to the filename provided here. And the same file name should be used while opening the database via [SQLitePlugin](https://github.com/litehelpers/Cordova-sqlite-storage).

   **location** -> You can pass three integer arguments here (Use 0 for Android)-

```
       (for ios only)
       location = 0; // It will copy the database in the default SQLite Database directory. This is the default location for database
       or
       location = 1; // If set will copy the database to Library folder instead of Documents folder.
       or
       location = 2; // (Disable iCloud Backup) If set will copy the database to Library/LocalDatabase. The database will not be synced by the iCloud Backup.
```
 **source** -> Source File location like /sdcard/mydb/db.db. Please provide a valid existing location and the dbname should be present in the path.
 
 **deletedb** -> A boolean value if set to true, will delete the existing db from the local app database folder before copying the new db. Please provide proper boolean value true or false;

 **success** -> function will be called if the db is copied sucessfully.

 **error** -> function will be called if the there is some problem in copying the db or the file already exists on the location.
 
**Copy Database To Device Storage**
============================================

This is an untested version. Let me know if you have any suggestions. Also Pull Request are always welcome.
```javascript 
    window.plugins.sqlDB.copyDbToStorage(dbname, location, destination, success, error);
```
 Here - 
 
 **dbname** -> Is the name of the database you want to copy. The dbname can be filename (without extensions) or filename.db or filename.sqlite. The plugin will look for and copy the file according to the filename provided here. And the same file name should be used while opening the database via [SQLitePlugin](https://github.com/litehelpers/Cordova-sqlite-storage).
 
 **location** -> You can pass three integer arguments here (Use 0 for Android)-
```javascript
   (for ios only)
   location = 0; // It will copy the database in the default SQLite Database directory. This is the default location for database
   or
   location = 1; // If set will copy the database to Library folder instead of Documents folder.
   or
   location = 2; // If set will copy the database from Library/LocalDatabase.
```
   **destination** -> Destination File location like /sdcard/mydb/ Please provide a valid existing location and "/" should be present at the end of the path. Do not append db name in the path.
   
   **success** -> function will be called if the db is copied sucessfully.
   
   **error** -> function will be called if the there is some problem in copying the db or the file already exists on the location.
   
   
**Remove**
==================================
This method allows you to remove the database from the apps default database storage location.

```javascript 
    window.plugins.sqlDB.remove(dbname, location, success,error);
```

Here -

  **dbname** -> Is the name of the database you want to remove. If the database file is having any extension, please provide that also.

  **location** -> The integer value for the location of database, see the copy method for options.

  **success** -> function will be called if the db is removed sucessfully.

  **error** -> function will be called if the there is some problem in removing the db or the file doesn't exists on the default database storage location.

###Example Usage

In your JavaScript or HTML use the following method -

```javascript 
function dbcopy()
{
        //Database filename to be copied is demo.db

        //location = 0, will copy the db to default SQLite Database Directory
        window.plugins.sqlDB.copy("demo.db", 0, copysuccess,copyerror);

        or

        //location = 1, will copy the database to /Library folder
        window.plugins.sqlDB.copy("demo.db", 1, copysuccess,copyerror);

        or

        //location = 2, will copy the database to /Library/LocalDatabase folder (Disable iCloud Backup)
        window.plugins.sqlDB.copy("demo.db", 2, copysuccess,copyerror);

}

function removeDB()
{
      var location = 1;
      window.plugins.sqlDB.remove("demo.db", location, rmsuccess,rmerror);  
}

function copysuccess()
{
        //open db and run your queries
         db = window.sqlitePlugin.openDatabase({name: "demo.db"});.
}

function copyerror(e)
{
        //db already exists or problem in copying the db file. Check the Log.
        console.log("Error Code = "+JSON.stringify(e));
        //e.code = 516 => if db exists
}
```
