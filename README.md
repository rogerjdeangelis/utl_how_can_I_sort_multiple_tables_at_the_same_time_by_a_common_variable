# utl_how_can_I_sort_multiple_tables_at_the_same_time_by_a_common_variable
How can I sort multiple tables at the same time by a common variable.  I need to sort two tables, sashelp.classfit and sashelp.class,  by age.

    ```  How can I sort multiple tables at the same time by a common variable                                                                                         ```
    ```                                                                                                                                                               ```
    ```  I need to sort two tables, sashelp.classfit and sashelp.class,  by age.                                                                                      ```
    ```                                                                                                                                                               ```
    ```  see                                                                                                                                                          ```
    ```  https://communities.sas.com/t5/Base-SAS-Programming/SORTING/m-p/415941                                                                                       ```
    ```                                                                                                                                                               ```
    ```  INPUT   Sort these by age                                                                                                                                    ```
    ```  =========================                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```    SASHELP.CLASS total obs=19                                                                                                                                 ```
    ```                                                                                                                                                               ```
    ```    NAME       SEX    AGE    HEIGHT    WEIGHT                                                                                                                  ```
    ```                                                                                                                                                               ```
    ```    Alfred      M      14     69.0      112.5                                                                                                                  ```
    ```    Alice       F      13     56.5       84.0                                                                                                                  ```
    ```    Barbara     F      13     65.3       98.0                                                                                                                  ```
    ```    Carol       F      14     62.8      102.5                                                                                                                  ```
    ```    Henry       M      14     63.5      102.5                                                                                                                  ```
    ```   ...                                                                                                                                                         ```
    ```                                                                                                                                                               ```
    ```   SASHELP.CLASSFIT total obs=19                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```   NAME       SEX    AGE    HEIGHT    WEIGHT    PREDICT  ...                                                                                                   ```
    ```                                                                                                                                                               ```
    ```   Joyce       F      11     51.3       50.5     56.993                                                                                                        ```
    ```   Louise      F      12     56.3       77.0     76.488                                                                                                        ```
    ```   Alice       F      13     56.5       84.0     77.268                                                                                                        ```
    ```   James       M      12     57.3       83.0     80.388                                                                                                        ```
    ```   Thomas      M      11     57.5       85.0     81.167                                                                                                        ```
    ```  ...                                                                                                                                                          ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```  PROCESS                                                                                                                                                      ```
    ```  =======                                                                                                                                                      ```
    ```                                                                                                                                                               ```
    ```    data log (keep=dsns status errortext errorcode);                                                                                                           ```
    ```                                                                                                                                                               ```
    ```       do dsns="classfit","class";                                                                                                                             ```
    ```                                                                                                                                                               ```
    ```         call symputx('dsn',dsns);                                                                                                                             ```
    ```                                                                                                                                                               ```
    ```            rc=dosubl('                                                                                                                                        ```
    ```                                                                                                                                                               ```
    ```               %let ErrorText= ;                                                                                                                               ```
    ```               %let ErrorCode= ;                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```               proc sort data=sashelp.&dsn out=&dsn;                                                                                                           ```
    ```                by age;                                                                                                                                        ```
    ```               run;                                                                                                                                            ```
    ```                                                                                                                                                               ```
    ```               %let ErrorText= &syserrortext;                                                                                                                  ```
    ```               %let ErrorCode= &syserr;                                                                                                                        ```
    ```            ');                                                                                                                                                ```
    ```                                                                                                                                                               ```
    ```         ErrorText  =  symget('ErrorText');                                                                                                                    ```
    ```         ErrorCode  =  symget('ErrorCode');                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```         if  rc ne 0                                                                                                                                           ```
    ```             or ErrorCode ne "0" Then do;                                                                                                                      ```
    ```             Status        =  "Failed      ";                                                                                                                  ```
    ```         end;                                                                                                                                                  ```
    ```         else do;                                                                                                                                              ```
    ```            Status="Completed";                                                                                                                                ```
    ```         end;                                                                                                                                                  ```
    ```         output;                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```       end;                                                                                                                                                    ```
    ```       stop;                                                                                                                                                   ```
    ```                                                                                                                                                               ```
    ```    run;quit;                                                                                                                                                  ```
    ```                                                                                                                                                               ```
    ```  OUTPUT                                                                                                                                                       ```
    ```  ======                                                                                                                                                       ```
    ```                                                                                                                                                               ```
    ```    WORK.LOG total obs=2                                                                                                                                       ```
    ```                                                                                                                                                               ```
    ```    Obs    DSNS        ERRORTEXT    ERRORCODE     STATUS                                                                                                       ```
    ```                                                                                                                                                               ```
    ```     1     classfit                     0        Completed                                                                                                     ```
    ```     2     class                        0        Completed                                                                                                     ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```    WORK.CLASSFIT total obs=19                                                                                                                                 ```
    ```                                                                                                                                                               ```
    ```      NAME       SEX    AGE    HEIGHT    WEIGHT                                                                                                                ```
    ```                                                                                                                                                               ```
    ```      Joyce       F      11     51.3       50.5                                                                                                                ```
    ```      Thomas      M      11     57.5       85.0                                                                                                                ```
    ```      James       M      12     57.3       83.0                                                                                                                ```
    ```      Jane        F      12     59.8       84.5                                                                                                                ```
    ```      John        M      12     59.0       99.5                                                                                                                ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```    WORK.CLASSFIT total obs=19                                                                                                                                 ```
    ```                                                                                                                                                               ```
    ```      NAME       SEX    AGE    HEIGHT    WEIGHT    PREDICT  ...                                                                                                ```
    ```                                                                                                                                                               ```
    ```      Joyce       F      11     51.3       50.5     56.993                                                                                                     ```
    ```      Thomas      M      11     57.5       85.0     81.167                                                                                                     ```
    ```      Louise      F      12     56.3       77.0     76.488                                                                                                     ```
    ```      James       M      12     57.3       83.0     80.388                                                                                                     ```
    ```      John        M      12     59.0       99.5     87.016                                                                                                     ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```  *                _            _       _                                                                                                                      ```
    ```   _ __ ___   __ _| | ___    __| | __ _| |_ __ _                                                                                                               ```
    ```  | '_ ` _ \ / _` | |/ _ \  / _` |/ _` | __/ _` |                                                                                                              ```
    ```  | | | | | | (_| | |  __/ | (_| | (_| | || (_| |                                                                                                              ```
    ```  |_| |_| |_|\__,_|_|\___|  \__,_|\__,_|\__\__,_|                                                                                                              ```
    ```                                                                                                                                                               ```
    ```  ;                                                                                                                                                            ```
    ```                                                                                                                                                               ```
    ```  No need just use                                                                                                                                             ```
    ```                                                                                                                                                               ```
    ```     sahelp.class                                                                                                                                              ```
    ```     sahelp.classfit                                                                                                                                           ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```  *          _       _   _                                                                                                                                     ```
    ```   ___  ___ | |_   _| |_(_) ___  _ __                                                                                                                          ```
    ```  / __|/ _ \| | | | | __| |/ _ \| '_ \                                                                                                                         ```
    ```  \__ \ (_) | | |_| | |_| | (_) | | | |                                                                                                                        ```
    ```  |___/\___/|_|\__,_|\__|_|\___/|_| |_|                                                                                                                        ```
    ```                                                                                                                                                               ```
    ```  ;                                                                                                                                                            ```
    ```                                                                                                                                                               ```
    ```    data log (keep=dsns status errortext errorcode);                                                                                                           ```
    ```                                                                                                                                                               ```
    ```       do dsns="classfit","class";                                                                                                                             ```
    ```                                                                                                                                                               ```
    ```         call symputx('dsn',dsns);                                                                                                                             ```
    ```                                                                                                                                                               ```
    ```            rc=dosubl('                                                                                                                                        ```
    ```                                                                                                                                                               ```
    ```               %let ErrorText= ;                                                                                                                               ```
    ```               %let ErrorCode= ;                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```               proc sort data=sashelp.&dsn out=&dsn;                                                                                                           ```
    ```                by age;                                                                                                                                        ```
    ```               run;                                                                                                                                            ```
    ```                                                                                                                                                               ```
    ```               %let ErrorText= &syserrortext;                                                                                                                  ```
    ```               %let ErrorCode= &syserr;                                                                                                                        ```
    ```            ');                                                                                                                                                ```
    ```                                                                                                                                                               ```
    ```         ErrorText  =  symget('ErrorText');                                                                                                                    ```
    ```         ErrorCode  =  symget('ErrorCode');                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```         if  rc ne 0                                                                                                                                           ```
    ```             or ErrorCode ne "0" Then do;                                                                                                                      ```
    ```             Status        =  "Failed      ";                                                                                                                  ```
    ```         end;                                                                                                                                                  ```
    ```         else do;                                                                                                                                              ```
    ```            Status="Completed";                                                                                                                                ```
    ```         end;                                                                                                                                                  ```
    ```         output;                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```       end;                                                                                                                                                    ```
    ```       stop;                                                                                                                                                   ```
    ```                                                                                                                                                               ```
    ```    run;quit;                                                                                                                                                  ```

