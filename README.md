# MySQL-Essential-Training

# Change MySQL Default Password
```
https://stackoverflow.com/questions/7534056/mysql-root-password-change
```

```
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('mypass');
```

```
FLUSH PRIVILEGES;
```

# How to start XAMPP MySQL by running a batch file?
```
https://stackoverflow.com/questions/39428053/how-to-start-xampp-mysql-by-running-a-batch-file
```

```
set root=c:\xampp\
%root%mysql\bin\mysqld.exe --defaults-file=%root%mysql\bin\my.ini --standalone --console
pause
```
