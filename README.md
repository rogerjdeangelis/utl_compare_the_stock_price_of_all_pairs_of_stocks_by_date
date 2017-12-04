# utl_compare_the_stock_price_of_all_pairs_of_stocks_by_date
Compare the stock price of all pairs of stocks by date
    Compare the stock price of all pairs of stocks by date

    INPUT
    =====

    WORK.STOCKS_T total obs=40          |   RULES all pairs = 5 chose 2 = n!/m!(n-m)! = 5!/(2!*3!)  =10
                                        |
          DATE       STOCK       VALUE  |      DATE       STOCK1    STOCK2    VAL1    VAL2    CLOSNESS
                                        |
        04DEC2017      S1         18    |    04DEC2017      S1        S5       18      92        74  (92-18)=72
        04DEC2017      S2         97    |    04DEC2017      S1        S4       18      25         7
        04DEC2017      S3         39    |    04DEC2017      S1        S3       18      39        21
        04DEC2017      S4         25    |    04DEC2017      S1        S2       18      97        79
        04DEC2017      S5         92    |    04DEC2017      S2        S5       97      92         5
                                        |    04DEC2017      S2        S4       97      25        72
        05DEC2017      S1         96    |    04DEC2017      S2        S3       97      39        58
        05DEC2017      S2         54    |    04DEC2017      S3        S5       39      92        53
        05DEC2017      S3         53    |    04DEC2017      S3        S4       39      25        14
        05DEC2017      S4          4    |    04DEC2017      S4        S5       25      92        67
        05DEC2017      S5          6    |   ....
                                        |
        06DEC2017      S1         81    |
       ...

    WORKING CODE
    ============

      select
        l.date format=date9.
        ,l.stock  as stock1
        ,r.stock  as stock2
        ,l.value  as val1
        ,r.value  as val2
        ,abs(l.value - r.value)  as closness
      from
        have as l, have as r
      where
        l.stock < r.stock  and
        l.date  =  r.date

    OUTPUT
    ======

    Obs      DATE       STOCK1    STOCK2    VAL1    VAL2    CLOSNESS

      1    04DEC2017      S1        S5       18      92        74
      2    04DEC2017      S1        S4       18      25         7
      3    04DEC2017      S1        S3       18      39        21
      4    04DEC2017      S1        S2       18      97        79
      5    04DEC2017      S2        S5       97      92         5
      6    04DEC2017      S2        S4       97      25        72
      7    04DEC2017      S2        S3       97      39        58
      8    04DEC2017      S3        S5       39      92        53
      9    04DEC2017      S3        S4       39      25        14
     10    04DEC2017      S4        S5       25      92        67

     11    05DEC2017      S1        S5       96       6        90
     12    05DEC2017      S1        S4       96       4        92
     13    05DEC2017      S1        S3       96      53        43
     14    05DEC2017      S1        S2       96      54        42
     15    05DEC2017      S2        S5       54       6        48
     16    05DEC2017      S2        S4       54       4        50
     17    05DEC2017      S2        S3       54      53         1
     18    05DEC2017      S3        S5       53       6        47
     19    05DEC2017      S3        S4       53       4        49
     20    05DEC2017      S4        S5        4       6         2

     21    06DEC2017      S1        S5       81      95        14
    ....

    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;

    data have;
      input  DATE date9. STOCK$ VALUE;
    cards4;
    04DEC2017 S1 18
    04DEC2017 S2 97
    04DEC2017 S3 39
    04DEC2017 S4 25
    04DEC2017 S5 92
    05DEC2017 S1 96
    05DEC2017 S2 54
    05DEC2017 S3 53
    05DEC2017 S4 4
    05DEC2017 S5 6
    06DEC2017 S1 81
    06DEC2017 S2 52
    06DEC2017 S3 85
    06DEC2017 S4 6
    06DEC2017 S5 95
    07DEC2017 S1 29
    07DEC2017 S2 27
    07DEC2017 S3 68
    07DEC2017 S4 97
    07DEC2017 S5 22
    08DEC2017 S1 68
    08DEC2017 S2 41
    08DEC2017 S3 55
    08DEC2017 S4 28
    08DEC2017 S5 47
    09DEC2017 S1 84
    09DEC2017 S2 63
    09DEC2017 S3 59
    09DEC2017 S4 58
    09DEC2017 S5 37
    10DEC2017 S1 72
    10DEC2017 S2 50
    10DEC2017 S3 93
    10DEC2017 S4 92
    10DEC2017 S5 58
    11DEC2017 S1 29
    11DEC2017 S2 39
    11DEC2017 S3 47
    11DEC2017 S4 67
    11DEC2017 S5 16
    ;;;;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __  ___
    / __|/ _ \| | | | | __| |/ _ \| '_ \/ __|
    \__ \ (_) | | |_| | |_| | (_) | | | \__ \
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|___/

    ;

    proc sql;
      create
         table want as
      select
        l.date format=date9.
        ,l.stock  as stock1
        ,r.stock  as stock2
        ,l.value  as val1
        ,r.value  as val2
        ,abs(l.value - r.value)  as closness
      from
        have as l, have as r
      where
        l.stock < r.stock  and
        l.date  =  r.date
    ;quit;

    proc print data=want width=min;
    run;quit;

