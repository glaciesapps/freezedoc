# Add new module

Adding a new module in icehrm is relatively easy. You don't have to build frontend manually, just defining them on javascript is enough.  

Let's see how to add a new module step by step.  
  
We'll create a new module named **teams** under **Employees** menu to manage teams within a company.

These changes should be done inorder to add the new module.
We'll create this module with two tabs named **Teams** and **Team Members**.
- First add a new migration at **core/migrations** to create database tables.  

<img  src="assets/migration.png">
  
- Add the migration on top of **list.php** inside migrations directory. Then new database tables will be automatically created.   

<img  src="assets/list.png">

  
- Add the module to **core/admin** and **core/modules** (note that if a module has only admin section adding it to core/admin is enough).    

<img  src="assets/core-index.png">  

Edit the meta.json file. Change the menu value to **Employees** to place the module under that menu.  
 
<img  src="assets/meta-json.png">

  

- Add related classes to **core/src** (e.g For teams its core/src/Teams )

<img  src="assets/teamadminmanager.png">
  
    
<img  src="assets/employeeteams.png">

  

- Add module to **web/admin/src** and **web/modules/src** (note that if a module has only admin section adding it to web/admin/src is enough)

<img  src="assets/web-index.png">  
  
    
<img  src="assets/web-lib.png">

  
- Add reference to **gulpfile.js**, under **admin-js** and **module-js** tasks (note that if a module has only admin section adding it to admin-js is enough)

<img  src="assets/gulpfile.png">

  
- Do a **vagrant ssh** and run gulp inside /vagrant to rebuild the frontend.
```
~ $ cd /vagrant
~ $ gulp
```

**note :** Check the log and fix if there are any errors.
  
   
   
 <img  src="assets/team-module.png">

 
