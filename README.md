# Project title: POS Web SQL


### Introduction

This project is aiming to create a database in Web SQL for POS System. It is including the tables creation, data insertion, and retrieve the value from tables stored in Web SQL. In this project, the data that have been inserted into tables in Web SQL is the exported data from SQL file of the database in PhpMyAdmin. Another purpose of this project is to compare the last entered data into table based on the datetime in Web SQL. 

### Installation
To access this project, the software programs need to be installed:
* XAMPP Installer
* Any IDE; in this project, Microsoft Visual Studio Code or PhpStorm is used.

Fistly, you need to create the database and tables in PhpMyAdmin, then insert the data into the tables. Below is the steps to create the database in PHPMyAdmin:

1. Open PHPMyAdmin (http://localhost/phpmyadmin)
2. ```Create a database with name kedairuncitsl```
     ![](gitImg/createDB.png)
     
3. Create tables with table name **category**, **supplier**, and **item**.
     * Table: category
     
     ![](gitImg/2022-04-27%20(7).png)
     
     * Table: supplier
     
     ![](gitImg/2022-04-27%20(8).png)
     
     * Table: item
     
     ![](gitImg/2022-04-27%20(4).png)
     

### Example/ Tutorial
* Create a PHP file named connection.php. This file use to connect the database.

```php
<?php
define("HOST", "localhost");     // The host you want to connect to.
define("USER", "root");    // The database username.
define("PASSWORD","");    // The database password.
define("DATABASE", "kedairuncitsl");    // The database name.
define("PASSWORD_KEY", "SMARTLAB");
define("QUOTES_GPC", (ini_get('magic_quotes_gpc') ? TRUE : FALSE));
date_default_timezone_set('Asia/Kuala_Lumpur');

try {
    $dbx = new PDO('mysql:host='.HOST.';dbname='.DATABASE, USER, PASSWORD);
    $dbx->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    $dbx->setAttribute(PDO::ATTR_EMULATE_PREPARES, false);
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}
?>
```

### Issue tracker for others
     https://github.com/nurulshafiqa/POS_SYSTEM/issues
     
### API documentation
1) Function errorHandler
  ```javascript
  function errorHandler(transaction, error) {
            console.log('Oops. Error was '+error.message+' (Code '+error.code+')');
            return true;}
  ```
  
* What a function do
     - This function is used to write out any errors and it will console the error message.
     
* What the function's parameters or arguments are
     - **transaction** and **error** are the parameters for this function.
     
* What a function returns
     - This function will return boolean true if there's an error.

2) Function runExample()

```javascript
function runExample() {
     createDbAndTables();
     getAllTables(getResult);
     getAllTablesFromDB(getResultSetFromTable);
     }
```
* What a function do
     - This function is used to call another functions
     
* What the function's parameters or arguments are
     - This function has no parameter
     
* What a function returns
     - This function has no return value.
 
3. Function createDbAndTables(callback)

```javascript
function createDbAndTables(callback) {
            db.transaction(function(transaction) {
                transaction.executeSql('DROP TABLE category',null,function(){console.log('Drop Succeeded');},function(){console.log('Drop Failed');});
                transaction.executeSql(
                    'CREATE TABLE IF NOT EXISTS category ' +
                    ' (CAT_CODE VARCHAR(20) NOT NULL PRIMARY KEY, ' +
                    ' CAT_NAME VARCHAR(30) DEFAULT NULL, CAT_DESC VARCHAR(256) DEFAULT NULL,'+
                    ' SYNCA varchar(50) DEFAULT NULL, SYNCB varchar(50) DEFAULT NULL, SYNCD datetime DEFAULT CURRENT_TIMESTAMP,' +
                    ' SYNMA varchar(50) DEFAULT NULL, SYNMB varchar(50) DEFAULT NULL, SYNMD datetime DEFAULT NULL);'
                );

                transaction.executeSql('DROP TABLE supplier',null,function(){console.log('Drop Succeeded');},function(){console.log('Drop Failed');});

                transaction.executeSql(
                    'CREATE TABLE IF NOT EXISTS supplier ' +
                    ' (SUPP_EMAIL VARCHAR(30) NOT NULL PRIMARY KEY, ' +
                    ' SUPP_NAME VARCHAR(30) DEFAULT NULL, SUPP_ADDRESS VARCHAR(100) DEFAULT NULL,' +
                    ' SUPP_COMPANY VARCHAR(30) DEFAULT NULL, SUPP_PHONE VARCHAR(15) DEFAULT NULL,' +
                    ' SUPP_CONTACT VARCHAR(15) DEFAULT NULL,' +
                    ' SYNCA varchar(50) DEFAULT NULL, SYNCB varchar(50) DEFAULT NULL, SYNCD datetime DEFAULT CURRENT_TIMESTAMP,' +
                    ' SYNMA varchar(50) DEFAULT NULL, SYNMB varchar(50) DEFAULT NULL, SYNMD datetime DEFAULT NULL);'
                );

                transaction.executeSql('DROP TABLE item',null,function(){console.log('Drop Succeeded');},function(){console.log('Drop Failed');});

                transaction.executeSql(
                    'CREATE TABLE IF NOT EXISTS item ' +
                    ' (ITEM_CODE VARCHAR(20) NOT NULL PRIMARY KEY, ' +
                    ' ITEM_NAME VARCHAR(20) DEFAULT NULL, ITEM_IMAGE LONGBLOB DEFAULT NULL,' +
                    ' ITEM_PRICE DOUBLE DEFAULT NULL, SUPP_COMPANY VARCHAR(30) DEFAULT NULL,' +
                    ' CATEGORY_NAME VARCHAR(20) DEFAULT NULL,' +
                    ' SYNCA varchar(50) DEFAULT NULL, SYNCB varchar(50) DEFAULT NULL, SYNCD datetime DEFAULT CURRENT_TIMESTAMP,' +
                    ' SYNMA varchar(50) DEFAULT NULL, SYNMB varchar(50) DEFAULT NULL, SYNMD datetime DEFAULT NULL);'
                );

                db.transaction(
                    function(transaction) {
                        for (var i = 0; i < category.length; i++) {
                            console.log('Attempting to insert ' + category[i]["code"] + category[i]["name"] + 'and' +  category[i]["name"]);
                            transaction.executeSql(
                                'INSERT INTO category (CAT_CODE, CAT_NAME, CAT_DESC, SYNCD) VALUES (?,?,?,?);',
                                [category[i]["code"], category[i]["name"], category[i]["desc"],category[i]["syncd"]],
                                null,
                                errorHandler

                            );

                        }

                        for (var i = 0; i < supplier.length; i++) {
                            console.log('Attempting to insert ' + supplier[i]["semail"] + ',' + supplier[i]["sname"] + ',' + supplier[i]["saddress"] + ',' + supplier[i]["scompany"] + ',' + supplier[i]["sphone"] + ' and ' + supplier[i]["scontact"]);
                            transaction.executeSql(
                                'INSERT INTO supplier (SUPP_EMAIL, SUPP_NAME, SUPP_ADDRESS, SUPP_COMPANY,SUPP_PHONE, SUPP_CONTACT, SYNCD) VALUES (?,?,?,?,?,?,?);',
                                [supplier[i]["semail"], supplier[i]["sname"],supplier[i]["saddress"],supplier[i]["scompany"],supplier[i]["sphone"],supplier[i]["scontact"],supplier[i]["syncd"]],
                                null,
                                errorHandler
                            );
                        }

                        for (var i = 0; i < item.length; i++) {
                            console.log('Attempting to insert ' + item[i]["itemcode"] + ',' + item[i]["itemname"] + ',' + item[i]["itemprice"] + ',' + item[i]["itemimage"] +   ',' + item[i]["supplierComp"] + ' and ' + item[i]["cateName"]);
                            transaction.executeSql(
                                'INSERT INTO item (ITEM_CODE, ITEM_NAME, ITEM_PRICE,ITEM_IMAGE, SUPP_COMPANY,CATEGORY_NAME,SYNCD) VALUES (?,?,?,?,?,?,?);',
                                [item[i]["itemcode"], item[i]["itemname"], item[i]["itemprice"], item[i]["itemimage"], item[i]["supplierComp"],item[i]["cateName"],item[i]["syncd"]],
                                null,
                                errorHandler
                            );
                        }
                    });
            });


        }// end createDB fx
        ```

* What a function do
     - This function is used to create tables which is table category, supplier, and item in Web SQL. It is also used to insert data into those tables.
     
* What the function's parameters or arguments are
     - This function has callback as the parameter (function that is passed as an argument to another function, to be “called back” at a later time.)
     
* What a function returns
     - This function has no return value.
 
