# utl_submit_r64
Interface R and Microsoft R with SAS and WPS


    ```  Interface R and MS-R with SAS/WPS                                                                       ```
    ```                                                                                                          ```
    ```   TWO EXAMPLES                                                                                           ```
    ```                                                                                                          ```
    ```     Using R                                                                                              ```
    ```                                                                                                          ```
    ```       1. createa SAS dataset                                                                             ```
    ```       2. import SAS dataset into R                                                                       ```
    ```       3. drop table if it exists                                                                         ```
    ```       4. create table in mysql database                                                                  ```
    ```       5. export mysql table back to SAS/WPS                                                                    ```
    ```                                                                                                          ```
    ```      Using free WPS Express                                                                              ```
    ```                                                                                                          ```
    ```       Same as above but imports mysql table to SAS                                                       ```
    ```                                                                                                          ```
    ```                                                                                                          ```
    ```        options validvarname=v7;                                                                          ```
    ```        libname sd1 "d:/sd1";                                                                             ```
    ```        data sd1.class;                                                                                   ```
    ```          set sashelp.class;                                                                              ```
    ```        run;quit;                                                                                         ```
    ```                                                                                                          ```
    ```        %utl_submit_r64('                                                                                 ```
    ```        source("c:/Program Files/R/R-3.3.2/etc/Rprofile.site",echo=T);                                    ```
    ```        library(haven);                                                                                   ```
    ```        library(RMySQL);                                                                                  ```
    ```        class<-read_sas("d:/sd1/class.sas7bdat");                                                         ```
    ```        mydb = dbConnect(MySQL(), user="root", password="xxxxxxx", dbname="sakila", port=3306);          ```
    ```        dbListTables(mydb);                                                                               ```
    ```        dbSendQuery(mydb, "drop table if exists class, iris");                                            ```
    ```        dbWriteTable(mydb, name="class", value=class);                                                    ```
    ```        rs = dbSendQuery(mydb, "select * from class where sex=''M'' ");                                   ```
    ```        class_sql<-fetch(rs, n=-1);                                                                       ```
    ```        str(class_sql);                                                                                   ```
    ```        dbDisconnect(mydb);                                                                               ```
    ```        ');                                                                                               ```
    ```                                                                                                          ```
    ```        * this passes the mysql table back to SAS with no restriction on the number od records;           ```
    ```        %utl_submit_wps64('                                                                               ```
    ```        options set=R_HOME "C:/Program Files/R/R-3.3.2";                                                  ```
    ```        libname wrk sas7bdat "%sysfunc(pathname(work))";                                                  ```
    ```        proc r;                                                                                           ```
    ```        submit;                                                                                           ```
    ```        source("C:/Program Files/R/R-3.3.2/etc/Rprofile.site", echo=T);                                   ```
    ```        library(haven);                                                                                   ```
    ```        library(RMySQL);                                                                                  ```
    ```        class<-read_sas("d:/sd1/class.sas7bdat");                                                         ```
    ```        mydb = dbConnect(MySQL(), user="root", password="xxxxxxx", dbname="sakila", port=3306);          ```
    ```        dbListTables(mydb);                                                                               ```
    ```        dbSendQuery(mydb, "drop table if exists class, iris");                                            ```
    ```        dbWriteTable(mydb, name="class", value=class);                                                    ```
    ```        rs = dbSendQuery(mydb, "select * from class where sex=''M'' ");                                   ```
    ```        class_wps<-fetch(rs, n=-1);                                                                       ```
    ```        str(class_sql);                                                                                   ```
    ```        dbDisconnect(mydb);                                                                               ```
    ```        endsubmit;                                                                                        ```
    ```        import r=class_wps data=wrk.class_wps;                                                            ```
    ```        run;quit;                                                                                         ```
    ```        ');                                                                                               ```
    ```                                                                                                          ```
    ```        proc print data=class_wps;                                                                        ```
    ```        run;quit;                                                                                         ```
    ```                                                                                                          ```
    ```                                                                                                          ```
    ```     Microsoft Revolution R with multithreaded MathPack                                                   ```
    ```                                                                                                          ```
    ```        * microsoft with math pack;                                                                       ```
    ```        %utl_submit_msr64('                                                                               ```
    ```        if(require("RevoUtilsMath")){                                                                     ```
    ```          setMKLthreads(5)                                                                                ```
    ```        };                                                                                                ```
    ```        m <-  63480;                                                                                      ```
    ```        n <-  1803;                                                                                       ```
    ```        A <- matrix (runif (m*n),m,n);                                                                    ```
    ```        nrow(A);                                                                                          ```
    ```        ncol(A);                                                                                          ```
    ```        A<-scale(A, scale = FALSE);                                                                       ```
    ```        system.time (B <- crossprod(A));                                                                  ```
    ```        cov<-B/nrow(A);                                                                                   ```
    ```        cov[1000:1005,500:505];                                                                           ```
    ```        ');                                                                                               ```
    ```                                                                                                          ```
    ```           user  system elapsed                                                                           ```
    ```          29.30    0.17    5.91                                                                           ```
    ```                                                                                                          ```
    ```         Vanillla R                                                                                       ```
    ```                                                                                                          ```
    ```          real time  1:05:38.72                                                                           ```
    ```                                                                                                          ```
    ```                                                                                                          ```
    ```     PASSING MACROS AND MACRO VARIABLE TO R                                                               ```
    ```                                                                                                          ```
    ```        * R fit statements inside SAS macro;                                                              ```
    ```        %macro reg(x=WEIGHT,y=HEIGHT);                                                                    ```
    ```          fit <- lm(&y ~ &x, data=df)                                                                     ```
    ```        %mend reg;                                                                                        ```
    ```                                                                                                          ```
    ```        %utl_submit_r64("                                                                                 ```
    ```        source('c:/Program Files/R/R-3.3.2/etc/Rprofile.site',echo=T);                                    ```
    ```        library(haven);                                                                                   ```
    ```        library(hms);                                                                                     ```
    ```        df<-read_sas('d:/sd1/class.sas7bdat');                                                            ```
    ```        attach(df);                                                                                       ```
    ```        %reg(x=WEIGHT,Y=HEIGHT);                                                                          ```
    ```        summary(fit);                                                                                     ```
    ```        %reg(x=AGE,Y=HEIGHT);                                                                             ```
    ```        summary(fit);                                                                                     ```
    ```        ");                                                                                               ```
    ```                                                                                                          ```
    ```        Residuals:                                                                                        ```
    ```            Min      1Q  Median      3Q     Max                                                           ```
    ```        -3.2328 -1.8602 -0.2124  1.7970  4.1982                                                           ```
    ```                                                                                                          ```
    ```        Coefficients:                                                                                     ```
    ```                    Estimate Std. Error t value Pr(>|t|)                                                  ```
    ```        (Intercept) 42.57014    2.67989  15.885 1.24e-11 ***                                              ```
    ```        WEIGHT       0.19761    0.02616   7.555 7.89e-07 ***                                              ```
