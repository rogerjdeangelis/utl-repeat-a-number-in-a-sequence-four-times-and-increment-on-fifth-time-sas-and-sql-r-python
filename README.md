# utl-repeat-a-number-in-a-sequence-four-times-and-increment-on-fifth-time-sas-and-sql-r-python
Repeat a number in a sequence four times and increment on fifth time sas and sql r python
    %let pgm=utl-repeat-a-number-in-a-sequence-four-times-and-increment-on-fifth-time-sas-and-sql-r-python;

    Repeat a number in a sequence four times and increment on fifth time sas and sql r python

      SOLUTIONS
         1 r sql
         2 python sql
           to resolve the macro variables, the sql code has to be on one line
         3 base sas
         4 base r

    github
    https://tinyurl.com/3uj459mv
    https://github.com/rogerjdeangelis/utl-repeat-a-number-in-a-sequence-four-times-and-increment-on-fifth-time-sas-and-sql-r-python

    stackoverflow R
    https://tinyurl.com/y7vvzetb
    https://stackoverflow.com/questions/79145285/repeat-a-number-in-a-sequence-n-times-and-increment-in-r

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /**************************************************************************************************************************/
    /*                           |                                                        |                                   */
    /*      INPUT                |           PROCESS                                      |    OUTPUT (SAS)                   */
    /*      =====                |           =======                                      |    ============                   */
    /*                           |                                                        |                                   */
    /*  %let tym1 =2001.;        | INCREMENT BY 0.25 FROM 2001 TO 2003                    |    SEQ      SEQUENCE              */
    /*  %let tym2 =2003.;        | AND RETURN THE INTEGER PART                            |                                   */
    /*  %let step=0.25;          |                                                        |  2001.00      2001 repeat         */
    /*                           | R AND PYTHON SQL (DO LOOP)                             |  2001.25      2001                */
    /*                           | ==========================                             |  2001.50      2001                */
    /*                           |                                                        |  2001.75      2001                */
    /*                           | with                                                   |                                   */
    /*                           |  recursive sequence(n) as (                            |  2002.00      2002 increment      */
    /*                           |   select                                               |  2002.25      2002                */
    /*                           |      &tym1                                             |  2002.50      2002                */
    /*                           |   union                                                |  2002.75      2002                */
    /*                           |      all                                               |                                   */
    /*                           |   select                                               |  2003.00      2003                */
    /*                           |      n + &step                                         |  2003.25      2003                */
    /*                           |   from                                                 |  2003.50      2003                */
    /*                           |      sequence                                          |  2003.75      2003                */
    /*                           |   where                                                |                                   */
    /*                           |      n < &tym2                                         |                                   */
    /*                           |   )                                                    |                                   */
    /*                           |   select                                               |                                   */
    /*                           |      cast(n as integer)  as sequence                   |                                   */
    /*                           |   from                                                 |                                   */
    /*                           |     sequence                                           |                                   */
    /*                           |                                                        |                                   */
    /*                           -------------------------------------------------------  -                                   */
    /*                           |                                                        |                                   */
    /*                           | BASE R                                                 |                                   */
    /*                           | ======                                                 |                                   */
    /*                           |                                                        |                                   */
    /*                           | want<-as.data.frame(rep(&tym1:&tym2, each =1/&step))   |      |                            */
    /*                           |                                                        |                                   */
    /*                           -------------------------------------------------------  |                                   */
    /*                           |                                                        |                                   */
    /*                           | SAS (GOOD EXAMPLE OF WHILE CLAUSE)                     |                                   */
    /*                           | ===                                                    |                                   */
    /*                           |                                                        |                                   */
    /*                           | data seq;                                              |                                   */
    /*                           |  do seq=&tym1 to &tym2+1 by &step while (seq<&tym2+1); |                                   */
    /*                           |    sequence=floor(seq);                                |                                   */
    /*                           |    output;                                             |                                   */
    /*                           |  end;                                                  |                                   */
    /*                           | run;quit;                                              |                                   */
    /*                           |                                                        |                                   */
    /**************************************************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

     %let tym1 =2001.;
     %let tym2 =2003.;
     %let step=0.25;

     /*                    _
    / |  _ __   ___  __ _| |
    | | | `__| / __|/ _` | |
    | | | |    \__ \ (_| | |
    |_| |_|    |___/\__, |_|
                       |_|
    */


    %let tym1 =2001.;
    %let tym2 =2003.;
    %let step=0.25;

    %utl_rbeginx;
    parmcards4;
    library(sqldf)
    source("c:/oto/fn_tosas9x.r")
    sequence <- sqldf("
       with
        recursive sequence(n) as (
         select
            &tym1
         union
            all
         select
            n + &step
         from
            sequence
         where
            n < (&tym2 + 1)
         )
         select
            CAST(n AS INTEGER)  as sequence
         from
           sequence
         where
           n < (&tym2 + 1)
    ")
     sequence
    fn_tosas9x(
          inp    = sequence
         ,outlib ="d:/sd1/"
         ,outdsn ="rwant"
         )
    ;;;;
    %utl_rendx;

    libname sd1 "d:/sd1";
    proc print data=sd1.rwant;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* R                SAS                                                                                                   */
    /*                                                                                                                        */
    /* > sequence       ROWNAMES    SEQUENCE                                                                                  */
    /*    sequence                                                                                                            */
    /* 1      2001          1         2001                                                                                    */
    /* 2      2001          2         2001                                                                                    */
    /* 3      2001          3         2001                                                                                    */
    /* 4      2001          4         2001                                                                                    */
    /* 5      2002          5         2002                                                                                    */
    /* 6      2002          6         2002                                                                                    */
    /* 7      2002          7         2002                                                                                    */
    /* 8      2002          8         2002                                                                                    */
    /* 9      2003          9         2003                                                                                    */
    /* 10     2003         10         2003                                                                                    */
    /* 11     2003         11         2003                                                                                    */
    /* 12     2003         12         2003                                                                                    */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                _   _                             _
    |___ \   _ __  _   _| |_| |__   ___  _ __    ___  __ _| |
      __) | | `_ \| | | | __| `_ \ / _ \| `_ \  / __|/ _` | |
     / __/  | |_) | |_| | |_| | | | (_) | | | | \__ \ (_| | |
    |_____| | .__/ \__, |\__|_| |_|\___/|_| |_| |___/\__, |_|
            |_|    |___/                                |_|
    */

    %let tym1 =2001.;
    %let tym2 =2003.;
    %let step=0.25;

    proc datasets lib=sd1 nolist nodetails;
     delete pywant;
    run;quit;

    %utl_pybeginx;
    parmcards4;
    exec(open('c:/oto/fn_python.py').read());
    s= \
    "with recursive sequence(n) as ( select &tym1 union all select n + &step from sequence where n < (&tym2 + 1) ) select CAST(n AS INTEGER) as sequence from sequence where n < (&tym2+
    want=pdsql(s)
    print(want);
    fn_tosas9x(want,outlib='d:/sd1/',outdsn='pywant',timeest=3);
    ;;;;
    %utl_pyendx(resolve=Y);

    libname sd1 "d:/sd1";
    proc print data=sd1.pywant;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  PYTHON             SAS                                                                                                */
    /*                                                                                                                        */
    /*      sequence       SEQUENCE                                                                                           */
    /*                                                                                                                        */
    /*  0       2001         2001                                                                                             */
    /*  1       2001         2001                                                                                             */
    /*  2       2001         2001                                                                                             */
    /*  3       2001         2001                                                                                             */
    /*  4       2002         2002                                                                                             */
    /*  5       2002         2002                                                                                             */
    /*  6       2002         2002                                                                                             */
    /*  7       2002         2002                                                                                             */
    /*  8       2003         2003                                                                                             */
    /*  9       2003         2003                                                                                             */
    /*  10      2003         2003                                                                                             */
    /*  11      2003         2003                                                                                             */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*____   _
    |___ /  | |__   __ _ ___  ___   ___  __ _ ___
      |_ \  | `_ \ / _` / __|/ _ \ / __|/ _` / __|
     ___) | | |_) | (_| \__ \  __/ \__ \ (_| \__ \
    |____/  |_.__/ \__,_|___/\___| |___/\__,_|___/

    */

    data seq;
     do seq=&tym1 to &tym2+1 by &step while (seq<&tym2+1);
       sequence=floor(seq);
       output;
     end;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* Obs      SEQ      SEQUENCE (FLOOR(SEQ)                                                                                 */
    /*                                                                                                                        */
    /*   1    2001.00      2001                                                                                               */
    /*   2    2001.25      2001                                                                                               */
    /*   3    2001.50      2001                                                                                               */
    /*   4    2001.75      2001                                                                                               */
    /*   5    2002.00      2002                                                                                               */
    /*   6    2002.25      2002                                                                                               */
    /*   7    2002.50      2002                                                                                               */
    /*   8    2002.75      2002                                                                                               */
    /*   9    2003.00      2003                                                                                               */
    /*  10    2003.25      2003                                                                                               */
    /*  11    2003.50      2003                                                                                               */
    /*  12    2003.75      2003                                                                                               */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*  _     _
    | || |   | |__   __ _ ___  ___   _ __
    | || |_  | `_ \ / _` / __|/ _ \ | `__|
    |__   _| | |_) | (_| \__ \  __/ | |
       |_|   |_.__/ \__,_|___/\___| |_|

    */

    %utl_rbeginx;
    parmcards4;
    library(haven)
    source("c:/oto/fn_tosas9x.R")
    want<-as.data.frame(rep(&tym1 : &tym2, each = 1/&step))
    colnames(want)="SEQUENCE"
    want
    fn_tosas9x(
          inp    = want
         ,outlib ="d:/sd1/"
         ,outdsn ="rwant"
         )
    ;;;;
    %utl_rendx;

    proc print data=sd1.rwant;
    run;quit;


    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  R                                                                                                                     */
    /*                                                                                                                        */
    /*     SEQUENCE     ROWNAMES    SEQUENCE                                                                                  */
    /*                                                                                                                        */
    /*  1      2001         1         2001                                                                                    */
    /*  2      2001         2         2001                                                                                    */
    /*  3      2001         3         2001                                                                                    */
    /*  4      2001         4         2001                                                                                    */
    /*  5      2002         5         2002                                                                                    */
    /*  6      2002         6         2002                                                                                    */
    /*  7      2002         7         2002                                                                                    */
    /*  8      2002         8         2002                                                                                    */
    /*  9      2003         9         2003                                                                                    */
    /*  10     2003        10         2003                                                                                    */
    /*  11     2003        11         2003                                                                                    */
    /*  12     2003        12         2003                                                                                    */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
