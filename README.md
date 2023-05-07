# utl-dealing-with-missing-values-consitently-within-and-between-multiple-languages-sas-R-and-python
Dealing with missing values consitently in and between multiple languages
    %let pgm=utl-dealing-with-missing-values-consitently-within-and-between-multiple-languages-sas-R-and-python;

    Dealing with missing values consitently in and between multiple languages;https://github.com/rogerjdeangelis/utl-dealing-with-missing-values-consitently-within-and-between-multiple-languages-sas-R-and-python

    github
    https://tinyurl.com/mr2b9f3y
    https://github.com/rogerjdeangelis/utl-dealing-with-missing-values-consitently-within-and-between-multiple-languages-sas-R-and-python

    ANSI SQL TO THE RESCUE.

    If you start your programming using ansi SQL all special missigs will be mapped to '.' in sas and NA in R and Python
    LESS is More (however I have outline key additional functionality for sas in the past - 128bit floats ansimode for proc sql)

    I find blind conversions to special missings in SAS, R, Python to be a serious flaw, especially Python.

             Solutions (Converts the many special missing values to one representation in SAS, R and Python)

                  1. SAS FEDSQL (ANSIMODE VIEW?)
                     SAS Maps the 28 sas special numeric missings to '.'

                  1. SAS DS2 (ANSIMODE VIEW?)
                     SAS Maps the 28 sas special numeric missings to '.'

                  2. R SQL
                     Mapping the four R missing numeric values to NA

                  3. R pyreadstat (maps sas numeric missings to NA)
                     Mapping SAS special missings numeric values to NA

                  4. Python SQL
                            It seems that R and Python believe MORE is MORE
                            I gave up trying to find all the possible mising values in Pythpn
                            NA NaaN None Inf -Inf none naan none?
                            However SQL seems to map all the ones I found to None
                            Mapping SAS special missings numeric values to NA

                  4. Python pyreadstat not consistent with SQL, so use SQL
                            I gave up trying to find all the possible mising values in Pythpn
                            NA NaaN None Inf -Inf none naan none pandas.nan?
                            pyreadstat is Inconsistent with SQL. I suggest you use
                            Pyhon SQL first then native python. If you do this
                            native python and sql will have one missing 'None'
                            However SQL seems to map everthing to None
                            Mapping SAS special missings numeric values to NA
    SOAPBOX ON

      I don't want to imply age descrimination.

      The hype around Python seems to be centered around younger programmers and possible academic bias.

      Older dinosaurs like me have a hard time deling with the complexity and inconsistency of Python.
      Maybe you can't teach old dogs new tricks.

      I do not want to imply that Python or R should go away. The packages are amazing. R with bleeding edge stats and graphics.
      Pyhthon with AI and internet of things. (raspberry PI and Arduino robotics)

    SOAPBOX ON

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* What do SAS missing values look like                                                                                    */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /* We need to convert the 27 missing values below to just one VAR=.   VAL=0000000000D1FFFF                                */
    /* Suppose someone sent you a table with some of these and you do not know which variables have special missings          */                                                          */
    /* The divide operator can silently produce some of these                                                                 */
    /*                                                                                                                        */
    /*  VAR=_   VAL=0000000000D2FFFF                                                                                          */
    /*  VAR=A   VAL=0000000000BEFFFF                                                                                          */
    /*  VAR=B   VAL=0000000000BDFFFF                                                                                          */
    /*  VAR=C   VAL=0000000000BCFFFF                                                                                          */
    /*  VAR=D   VAL=0000000000BBFFFF                                                                                          */
    /*  VAR=E   VAL=0000000000BAFFFF                                                                                          */
    /*  VAR=F   VAL=0000000000B9FFFF                                                                                          */
    /*  VAR=G   VAL=0000000000B8FFFF                                                                                          */
    /*  VAR=H   VAL=0000000000B7FFFF                                                                                          */
    /*  VAR=I   VAL=0000000000B6FFFF                                                                                          */
    /*  VAR=J   VAL=0000000000B5FFFF                                                                                          */
    /*  VAR=K   VAL=0000000000B4FFFF                                                                                          */
    /*  VAR=L   VAL=0000000000B3FFFF                                                                                          */
    /*  VAR=M   VAL=0000000000B2FFFF                                                                                          */
    /*  VAR=N   VAL=0000000000B1FFFF                                                                                          */
    /*  VAR=O   VAL=0000000000B0FFFF                                                                                          */
    /*  VAR=P   VAL=0000000000AFFFFF                                                                                          */
    /*  VAR=Q   VAL=0000000000AEFFFF                                                                                          */
    /*  VAR=R   VAL=0000000000ADFFFF                                                                                          */
    /*  VAR=S   VAL=0000000000ACFFFF                                                                                          */
    /*  VAR=T   VAL=0000000000ABFFFF                                                                                          */
    /*  VAR=U   VAL=0000000000AAFFFF                                                                                          */
    /*  VAR=V   VAL=0000000000A9FFFF                                                                                          */
    /*  VAR=W   VAL=0000000000A8FFFF                                                                                          */
    /*  VAR=X   VAL=0000000000A7FFFF                                                                                          */
    /*  VAR=Y   VAL=0000000000A6FFFF                                                                                          */
    /*  VAR=Z   VAL=0000000000A5FFFF                                                                                          */
    /*                                                                                                                        */
    /**************************************************************************************************************************/
    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    libname sd1 "d:/sd1";

    data  have sd1.have;
      array ms[28] m1-m28 (._, ., .A .B .C .D .E .F .G .H .I .J .K .L .M .N .O .P .Q .R .S .T .U .V .W .X .Y .Z );
      do idx=1 to dim(ms);
        missings=ms[idx];
        output;
      end;
      keep missings;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* The 28 missings I do not like that they are printed without a leading '.'                                              */
    /*                                                                                                                        */
    /*  Up to 40 obs from last table WORK.HAVE total obs=28 06MAY2023:17:19:19                                                */
    /*                                                                                                                        */
    /*  Obs    MISSINGS   Obs    MISSINGS                                                                                     */
    /*                                                                                                                        */
    /*    1        _       17        O                                                                                        */
    /*    2        .       18        P                                                                                        */
    /*    3        A       19        Q                                                                                        */
    /*    4        B       20        R                                                                                        */
    /*    5        C       21        S                                                                                        */
    /*    6        D       22        T                                                                                        */
    /*    7        E       23        U                                                                                        */
    /*    8        F       24        V                                                                                        */
    /*    9        G       25        W                                                                                        */
    /*   10        H       26        X                                                                                        */
    /*   11        I       27        Y                                                                                        */
    /*   12        J       28        Z                                                                                        */
    /*   13        K                                                                                                          */
    /*   14        L                                                                                                          */
    /*   15        M                                                                                                          */
    /*   16        N                                                                                                          */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  Up to 40 obs from WANT_FEDSLQ total obs=28 06MAY2023:18:14:07                                                         */
    /*  Obs    MISSINGS  Obs    MISSINGS                                                                                      */
    /*                                                                                                                        */
    /*    1        .      17        .                                                                                         */
    /*    2        .      18        .                                                                                         */
    /*    3        .      19        .                                                                                         */
    /*    4        .      20        .                                                                                         */
    /*    5        .      21        .                                                                                         */
    /*    6        .      22        .                                                                                         */
    /*    7        .      23        .                                                                                         */
    /*    8        .      24        .                                                                                         */
    /*    9        .      25        .                                                                                         */
    /*   10        .      26        .                                                                                         */
    /*   11        .      27        .                                                                                         */
    /*   12        .      28        .                                                                                         */
    /*   13        .                                                                                                          */
    /*   14        .                                                                                                          */
    /*   15        .                                                                                                          */
    /*   16        .                                                                                                          */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                __          _           _    __ _
     ___  __ _ ___   / _| ___  __| |___  __ _| |  / _(_)_  __
    / __|/ _` / __| | |_ / _ \/ _` / __|/ _` | | | |_| \ \/ /
    \__ \ (_| \__ \ |  _|  __/ (_| \__ \ (_| | | |  _| |>  <
    |___/\__,_|___/ |_|  \___|\__,_|___/\__, |_| |_| |_/_/\_\
                                           |_|
    */
    proc fedsql ansimode;
      create
         table want_fedSlq  as
      select
         missings
      from
         have
    ;quit;

    /*                   _     ____
     ___  __ _ ___    __| |___|___ \
    / __|/ _` / __|  / _` / __| __) |
    \__ \ (_| \__ \ | (_| \__ \/ __/
    |___/\__,_|___/  \__,_|___/_____|

    */

    proc ds2 ansimode;
    data want_ds2;
       declare double miss;
       method run();
          set have;
          miss=missings;
          output;
       end;
    enddata;
    run;
    quit;

    /*___              _
    |  _ \   ___  __ _| |
    | |_) | / __|/ _` | |
    |  _ <  \__ \ (_| | |
    |_| \_\ |___/\__, |_|
                    |_|
    */

    /(----    R numeric  missings                            ----*/
    /*----    NaN  = not a number  (1/0, -1/0)               ----*/
    /*----    NA   = not available (subscript out of range)  ----*/
    /*----           numeric(as.numeric("A") )               ----*/
    /*----    Inf  =  1/0                                    ----*/
    /*----    -Inf = -1/0                                    ----*/

    %utl_submit_r64('
    library(haven);
    library(sqldf);
    library(SASxport);
    want<-read_sas("d:/sd1/have.sas7bdat");
    want;
    want_sql<-sqldf("
         select
           -1/0 as NEG_ONE_OVER_0
          , 1/0 as ONE_OVER_0
          , 0/0 as O_OVER_0
          ,-0/0 as NEG_0_OVER_0
       from
          want
       limit 1
       ");
    want_sql;
    ');

    libname xpt xport "d:/xpt/want.xpt";
    data want;
      set xpt.want;
    run;quit;
    libname xpt clear;


    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  Very consistent using SQL                                                                                             */
    /*                                                                                                                        */
    /*     PROBLEM           R SQL Solutiom                                                                                   */
    /*                                                                                                                        */
    /*     -1/0 = -inf         -1/0 = NA                                                                                      */
    /*      1/0 = inf           1/0 = NA                                                                                      */
    /*      0/0 = nan           0/0 = NA                                                                                      */
    /*     -0/0 = nan          -0/0 = NA                                                                                      */
    /*                                                                                                                        */
    /*  pyreadstat maps all 28 sas mssings to NA                                                                              */
    /*                                                                                                                        */
    /*    ._   = NA    .N   = NA                                                                                              */
    /*    .A   = NA    .O   = NA                                                                                              */
    /*    .B   = NA    .P   = NA                                                                                              */
    /*    .C   = NA    .Q   = NA                                                                                              */
    /*    .D   = NA    .R   = NA                                                                                              */
    /*    .E   = NA    .S   = NA                                                                                              */
    /*    .F   = NA    .T   = NA                                                                                              */
    /*    .G   = NA    .U   = NA                                                                                              */
    /*    .H   = NA    .V   = NA                                                                                              */
    /*    .I   = NA    .W   = NA                                                                                              */
    /*    .J   = NA    .X   = NA                                                                                              */
    /*    .K   = NA    .Y   = NA                                                                                              */
    /*    .L   = NA    .Z   = NA                                                                                              */
    /*    .M   = NA                                                                                                           */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*           _   _                             _
     _ __  _   _| |_| |__   ___  _ __    ___  __ _| |
    | `_ \| | | | __| `_ \ / _ \| `_ \  / __|/ _` | |
    | |_) | |_| | |_| | | | (_) | | | | \__ \ (_| | |
    | .__/ \__, |\__|_| |_|\___/|_| |_| |___/\__, |_|
    |_|    |___/                                |_|

    %utl_submit_py64_310("

    import numpy as np;

    NEG_ONE_OVER_0 = np.float64(-1.0) / 0.0;print(NEG_ONE_OVER_0);
    ONE_OVER_0     = np.float64( 1.0) / 0.0;print(ONE_OVER_0    );
    O_OVER_0       = np.float64( 0.0) / 0.0;print(O_OVER_0      );
    NEG_0_OVER_0   = np.float64(-0.0) / 0.0;print(NEG_0_OVER_0  );
    ");

    proc datasets lib=work kill nodetails nolist;
    run;quit;

    %utlfkil(d:/xpt/res.xpt);

    %utl_pybegin;
    parmcards4;
    from os import path
    import pandas as pd
    import xport
    import xport.v56
    import pyreadstat
    import numpy as np
    from pandasql import sqldf
    mysql = lambda q: sqldf(q, globals())
    from pandasql import PandaSQL
    pdsql = PandaSQL(persist=True)
    sqlite3conn = next(pdsql.conn.gen).connection.connection
    sqlite3conn.enable_load_extension(True)
    sqlite3conn.load_extension('c:/temp/libsqlitefunctions.dll')
    mysql = lambda q: sqldf(q, globals())
    have, meta = pyreadstat.read_sas7bdat("d:/sd1/have.sas7bdat")
    print(have);
    res = pdsql("""
      select
         -1/0 as NEG_ONE_OVER_0
        , 1/0 as ONE_OVER_0
        , 0/0 as O_OVER_0
        ,-0/0 as NEG_0_OVER_0
      from
        have
      limit 1
    """)
    print(res);
    sas = pdsql("""
      select
         *
      from
        have
    """)
    print(sas);
    ds = xport.Dataset(res, name='res')
    with open('d:/xpt/res.xpt', 'wb') as f:
        xport.v56.dump(ds, f)
    ;;;;
    %utl_pyend;

    libname pyxpt xport "d:/xpt/res.xpt";

    proc contents data=pyxpt._all_;
    run;quit;

    proc print data=pyxpt.res;
    run;quit;

    data res;
       set pyxpt.res;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* pyreadstat is inconsistent with sql                                                                                    */
    /*                                                                                                                        */
    /* pyreadstat                                                                                                             */
    /*                                                                                                                        */
    /* pyreadstat maps all 28 sas mssings to NA                                                                               */
    /*                                                                                                                        */
    /*   PROBLEM                   SOLUTION  SQL                                                                              */
    /*                                                                                                                        */
    /*   ._   = NaaN               ._   = None                                                                                */
    /*   .A   = NaaN               .A   = None                                                                                */
    /*   .B   = NaaN               .B   = None                                                                                */
    /*   .C   = NaaN               .C   = None                                                                                */
    /*   .D   = NaaN               .D   = None                                                                                */
    /*   .E   = NaaN               .E   = None                                                                                */
    /*   .F   = NaaN               .F   = None                                                                                */
    /*   .G   = NaaN               .G   = None                                                                                */
    /*   .H   = NaaN               .H   = None                                                                                */
    /*   .I   = NaaN               .I   = None                                                                                */
    /*   .J   = NaaN               .J   = None                                                                                */
    /*   .K   = NaaN               .K   = None                                                                                */
    /*   .L   = NaaN               .L   = None                                                                                */
    /*   .M   = NaaN               .M   = None                                                                                */
    /*   .N   = NaaN               .N   = None                                                                                */
    /*   .O   = NaaN               .O   = None                                                                                */
    /*   .P   = NaaN               .P   = None                                                                                */
    /*   .Q   = NaaN               .Q   = None                                                                                */
    /*   .R   = NaaN               .R   = None                                                                                */
    /*   .S   = NaaN               .S   = None                                                                                */
    /*   .T   = NaaN               .T   = None                                                                                */
    /*   .U   = NaaN               .U   = None                                                                                */
    /*   .V   = NaaN               .V   = None                                                                                */
    /*   .W   = NaaN               .W   = None                                                                                */
    /*   .X   = NaaN               .X   = None                                                                                */
    /*   .Y   = NaaN               .Y   = None                                                                                */
    /*   .Z   = NaaN               .Z   = None                                                                                */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /*   -1/0 = -inf               -1/0 = None                                                                                */
    /*    1/0 = inf                 1/0 = None                                                                                */
    /*    0/0 = nan                 0/0 = None                                                                                */
    /*   -0/0 = nan                -0/0 = None                                                                                */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */

