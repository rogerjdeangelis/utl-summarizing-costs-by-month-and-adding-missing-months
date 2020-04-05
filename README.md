# utl-summarizing-costs-by-month-and-adding-missing-months
Summarizing costs by month and adding missing months
    Summarizing costs by month and adding missing months

    github
    https://tinyurl.com/vmlekxz
    https://github.com/rogerjdeangelis/utl-summarizing-costs-by-month-and-adding-missing-months


    related to(perhaps more general)
    https://tinyurl.com/utmlvj6
    https://communities.sas.com/t5/SAS-Programming/Summarizing-costs-by-month-and-parallely-add-missing-months-to/m-p/637469

    *_                   _
    (_)_ __  _ __  _   _| |_
    | | '_ \| '_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    ;

    * split up date into parts;
    data have;
     retain id month;
     lenght month $3.;
     input ID MthSas :monyy7. Cost;
     format mthSas monyy7.;
     month=put(month(mthsas),z2.);
     year=year(mthSAS);
    cards4;
    1 Jan-07 5
    1 Jan-07 5
    1 Mar-07 5
    1 May-07 5
    2 Aug-08 6
    2 Aug-08 6
    2 Dec-08 13
    ;;;;
    run;quit;

    proc format;
        value $mon2mon
    "01"="JAN"
    "02"="FEB"
    "03"="MAR"
    "04"="APR"
    "05"="MAY"
    "06"="JUN"
    "07"="JUL"
    "08"="AUG"
    "09"="SEP"
    "10"="OCT"
    "11"="NOV"
    "12"="DEC"
    ;
    run;quit;


    WORKHAVE total obs=7

      ID    MONTH   YEAR   MTHSAS     COST

       1     01     2007   JAN2007      5
       1     01     2007   JAN2007      5
       1     03     2007   MAR2007      5
       1     05     2007   MAY2007      5
       2     08     2008   AUG2008      6
       2     08     2008   AUG2008      6
       2     12     2008   DEC2008     13

    *            _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| '_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    ;
    This code does full year for each ID

                               | RULES
    WORK.WANT OBS=24           |
                               |
     ID    MONTH     TOT       |
                               | SUM  ID    MONTH    MTHSAS     COST
      1     JAN  2008     10   |       1     01      JAN2007      5
                                       1     01      JAN2007      5   (5+5)=10

      1     FEB  2008      0   | Fill in missing Feb with 0

      1     MAR  2008      5   |       1     03      MAR2007      5

      1     APR  2008      0   | Fill in missing Feb with 0

      1     MAY  2008      5   |       1     05      MAY2007      5

      1     JUN  2007      0   | Fill in
      1     JUL  2007      0   | Fill in
      1     AUG  2007      0   | Fill in
      1     SEP  2007      0   | Fill in
      1     OCT  2007      0   | Fill in
      1     NOV  2007      0   | Fill in
      1     DEC  2007      0   | Fill in

      2     JAN  2008      0   | Start over with new ID
      2     FEB  2008      0   |
      2     MAR  2008      0   |
      2     APR  2008      0   |
      2     MAY  2008      0   |
      2     JUN  2008      0   |
      2     JUL  2008      0   |
      2     AUG  2008     12   |
      2     SEP  2008      0   |
      2     OCT  2008      0   |
      2     NOV  2008      0   |
      2     DEC  2008     13   |


    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;


    options missing="0";
    proc summary data=have completeTypes nway order=data;
      format month $mon2mon.;
      by id year;
      class month / preloadfmt;
      var cost;
      output out=want(drop=_:) sum=tot;
    run;

    proc print data=want;
    run;quit;

    Obs    ID    YEAR    MONTH    TOT

      1     1    2007     JAN      10
      2     1    2007     FEB       0
      3     1    2007     MAR       5
      4     1    2007     APR       0
      5     1    2007     MAY       5
      6     1    2007     JUN       0
      7     1    2007     JUL       0
      8     1    2007     AUG       0
      9     1    2007     SEP       0
     10     1    2007     OCT       0
     11     1    2007     NOV       0
     12     1    2007     DEC       0
     13     2    2008     JAN       0
     14     2    2008     FEB       0
     15     2    2008     MAR       0
     16     2    2008     APR       0
     17     2    2008     MAY       0
     18     2    2008     JUN       0
     19     2    2008     JUL       0
     20     2    2008     AUG      12
     21     2    2008     SEP       0
     22     2    2008     OCT       0
     23     2    2008     NOV       0
     24     2    2008     DEC      13



