# utl-area-of-intersection-of-arbitrary-polygons-R-AI
Area of intersection of arbitrary polygons R AI.

    Area of intersection of arbitrary polygons R AI

    Most of the R code is get data in and out of R.

    github
    https://tinyurl.com/yb8ckhsy
    https://github.com/rogerjdeangelis/utl-area-of-intersection-of-arbitrary-polygons-R-AI
    
    
    R graphic
    https://tinyurl.com/yd3ybhkx
    https://github.com/rogerjdeangelis/utl-area-of-intersection-of-arbitrary-polygons-R-AI/blob/master/squares.png

    StackOverflow R
    https://tinyurl.com/ycuxaurk
    https://stackoverflow.com/questions/54234895/calculate-area-overlap-for-pairs-of-polygons-in-matrix-format-in-r

    Wimpel profile
    https://stackoverflow.com/users/6356278/wimpel

    INPUT
    =====

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.hav1st;
      * roud trip unit square;
      input x y;
    cards4;
    0 0
    1 0
    1 1
    0 1
    0 0
    ;;;;
    run;quit;

    data sd1.hav2nd;
      * roud trip unit square;
      input x y;
    cards4;
    .5 .5
    .5 1.5
    1.5 1.5
    1.5 .5
    .5 .5
    ;;;;
    run;quit;

    EXAMPLE OUTPUT
    --------------

    WORK.WANT total obs=1

    Obs    ST_INTER

     1       0.25    *** intersection area



    1.5 +              +----------------------------+
        |              |                            |
        |              |                            |
        |              |                            |
        |              |                            |
        |              |                            |
        |              |                            |
    1.0 +--------------+--------------+             |
        |              |              |             |
        |              |              |             |
        |              |   25%        |             |
        |              |              |             |
        |              |              |             |
        |              |              |             |
    0.5 +              +--------------+-------------+
        |           0.5,0.5           |
        |                             |
        |                             |
        |                             |
        |                             |
        |                             |
        +--------------+--------------+
         0            0.5            1.0


    PROCESS
    =======


    %utlfkil(d:/png/squares.png);
    %utl_submit_r64('
    library( sf);
    library(haven);
    library(SASxport);
    hav1st<-unlist(read_sas("d:/sd1/hav1st.sas7bdat"));
    hav1st<-list(matrix(hav1st, ncol=2, nrow=5));
    hav2nd<-unlist(read_sas("d:/sd1/hav2nd.sas7bdat"));
    hav2nd<-list(matrix(hav2nd, ncol=2, nrow=5));
    a = st_polygon(hav1st);
    b = st_polygon(hav2nd);
    png("d:/png/squares.png");
    plot(st_sfc(a, b));
    want<-as.data.frame(st_intersection(a, b ) %>% st_area());
    write.xport(want,file="d:/xpt/want.xpt");
    ');

    libname xpt xport "d:/xpt/want.xpt";
    data want;
      set xpt.want;
    run;quit;
    libname xpt clear;


    OUTPUT
    ======

    * create ascii plot;
    * you need to edit it;
    data havAll;
      set sd1.hav1st sd1.hav2nd;
    run;quit;

    options ls=64 ps=32;
    proc plot data=havall;
    plot y*x=' ' / box href=1 .5 vref=.5 1;
    run;quit;




