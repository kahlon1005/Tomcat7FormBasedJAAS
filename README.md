##Configure JDBCRealm JAAS for mysql and tomcat 7 with form based authentication

####MySql

Prepare database 
Copy paste the following sql script and run from mysql command prompt

```
CREATE DATABASE tutorialsdb;

DROP DATABASE IF EXISTS tutorialsdb;
CREATE DATABASE tutorialsdb;
USE tutorialsdb;
CREATE TABLE users (
username varchar(20) NOT NULL PRIMARY KEY,
password varchar(20) NOT NULL
);
CREATE TABLE roles (
rolename varchar(20) NOT NULL PRIMARY KEY
);
CREATE TABLE users_roles (
username varchar(20) NOT NULL,
rolename varchar(20) NOT NULL,
PRIMARY KEY (username, rolename),
CONSTRAINT users_roles_fk1 FOREIGN KEY (username) REFERENCES users (username),
CONSTRAINT users_roles_fk2 FOREIGN KEY (rolename) REFERENCES roles (rolename)
);
INSERT INTO `tutorialsdb`.`users` (`username`, `password`) VALUES ('admin', 'admin');
INSERT INTO `tutorialsdb`.`roles` (`rolename`) VALUES ('user');
INSERT INTO `tutorialsdb`.`users_roles` (`username`, `rolename`) VALUES ('admin', 'user');
COMMIT;
```


####server.xml
Configure tomcat 7 server.xml for JDBCRealm

Add a realm tag in `tomcat_home/conf/server.xml` file. Place `mysql-connector-java.jar` in `tomcat_home/lib`
```
<Realm  className="org.apache.catalina.realm.JDBCRealm"
 driverName="com.mysql.jdbc.Driver"
 connectionURL="jdbc:mysql://localhost:3306/tutorialsdb"
 connectionName="root"
 connectionPassword="root"
 userTable="users"
 userNameCol="username"
 userCredCol="password"
 userRoleTable="users_roles"
 roleNameCol="rolename" />
 ```
#####Run and test the application

Start tomcat 7 server and hit
```
http://localhost:8080/Tomcat7FormBasedJAAS/protected/protected.jsp
```
and login with ```admin/admin```
