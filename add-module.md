# Add new module

Adding a new module in icehrm is relatively easy. You don't have to build frontend manually, just defining them on javascript is enough.  

Let's see how to add a new module step by step.  
  
We'll create a new module named **teams** under **Employees** menu to manage teams within a company.

These changes should be done inorder to add the new module.
We'll create this module with two tabs named **Teams** and **Team Members**.
- First add a new migration at **core/migrations** to create database tables.  

<img  src="assets/migration.png">
  
```
<?php
namespace Classes\Migration;

class v20191121_270007_team_management extends AbstractMigration {

    public function up(){

        $sql = <<<'SQL'
create table `EmployeeTeams` (
	`id` bigint(20) NOT NULL AUTO_INCREMENT,
	`name` varchar(100),
	`description` text,
	`lead` bigint(20),
	`department` bigint(20),
	primary key  (`id`),
	CONSTRAINT `Fk_EmployeeTeams_Lead` FOREIGN KEY (`lead`) REFERENCES `Employees` (`id`) ON DELETE SET NULL ON UPDATE CASCADE,
	CONSTRAINT `Fk_EmployeeTeams_Department` FOREIGN KEY (`department`) REFERENCES `CompanyStructures` (`id`) ON DELETE SET NULL ON UPDATE CASCADE
) engine=innodb default charset=utf8;
SQL;

        return $this->executeQuery($sql);

        $sql = <<<'SQL'
create table `EmployeeTeamMembers` (
	`id` bigint(20) NOT NULL AUTO_INCREMENT,
	`team` bigint(20) ,
	`member` bigint(20),
	`role` varchar(60),
	primary key  (`id`),
	CONSTRAINT `Fk_EmployeeTeamMembers_Team` FOREIGN KEY (`team`) REFERENCES `EmployeeTeams` (`id`) ON DELETE CASCADE ON UPDATE CASCADE,
	CONSTRAINT `Fk_EmployeeTeamMembers_Member` FOREIGN KEY (`member`) REFERENCES `Employees` (`id`) ON DELETE CASCADE ON UPDATE CASCADE
) engine=innodb default charset=utf8;
SQL;

        return $this->executeQuery($sql);
    }
    public function down(){
        return true;
    }
}

```
- Add the migration on top of **list.php** inside migrations directory. Then new database tables will be automatically created.   

<img  src="assets/list.png">

  
- Add the module to **core/admin** and **core/modules** (note that if a module has only admin section adding it to core/admin is enough).    

<img  src="assets/core-index.png">  

```
<?php

$moduleName = 'teams';
$moduleGroup = 'admin';
define('MODULE_PATH',dirname(__FILE__));
include APP_BASE_PATH.'header.php';
include APP_BASE_PATH.'modulejslibs.inc.php';

$options = array();
$options['setRemoteTable'] = 'false';

$moduleBuilder = new \Classes\ModuleBuilder\ModuleBuilder();
$moduleBuilder->addModuleOrGroup(new \Classes\ModuleBuilder\ModuleTab(
    'Teams','EmployeeTeams','Teams','TeamAdapter','','',true
));
$options['setRemoteTable'] = 'true';
$moduleBuilder->addModuleOrGroup(new \Classes\ModuleBuilder\ModuleTab(
    'TeamMembers','EmployeeTeamMembers','Team Members','TeamMembersAdapter','','',false
));

echo \Classes\UIManager::getInstance()->renderModule($moduleBuilder);

?>
<?php
include APP_BASE_PATH.'footer.php';
```

Edit the meta.json file. Change the menu value to **Employees** to place the module under that menu.  
**model_namespace** and manager **values** should be correctly mapped as explained in the next step. 
 
<img  src="assets/meta-json.png">

 ```
 {
  "label": "Company Teams",
  "menu": "Employees",
  "order": "83",
  "icon": "fa-users",
  "user_levels": [
    "Admin",
    "Manager"
  ],
  "permissions": [],
  "model_namespace": "\\Teams\\Common\\Model",
  "manager": "\\Teams\\Admin\\Api\\TeamAdminManager"
}

 ```

- Add related classes to **core/src** (e.g For teams its core/src/Teams )
each admin module should have the AdminManager class
its path should be mapped in meta.json under manager key.

<img  src="assets/teamadminmanager.png">

Every database table should have a model class. Include the database table name and edit the functions to give access.
    
<img  src="assets/employeeteams.png">

  

- Add module to **web/admin/src** and **web/modules/src** (note that if a module has only admin section adding it to web/admin/src is enough)
This will build the frontend of the new module. Add the headers and formfields.
If you want to include data from another table for a dropbox, use select2 as the type and use the model class name of that table for remote-resource.
We can also use dropdowns with values provided by us using select as the formfield type.


<img  src="assets/web-lib.png">  
  
initialize the adapter classes in lib.js

<img  src="assets/web-index.png">  
  
    
- Add reference to **gulpfile.js**, under **admin-js** and **module-js** tasks (note that if a module has only admin section adding it to admin-js is enough)

<img  src="assets/gulpfile.png">

  
- Do a **vagrant ssh** and run gulp inside /vagrant to rebuild the frontend.
```
~ $ cd /vagrant
~ $ gulp
```

**note :** Check the log and fix if there are any errors.
  
   
   
 <img  src="assets/team-module.png">

 
