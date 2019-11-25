# utl-is-proc-report-more-powerfull-than-proc-summary-or-proc-means
Is procreport more powerfull than proc summary or proc means 

    Is procreport more powerfull than proc summary or proc means                                                 
                                                                                                                 
     Problem: Produce this report and output dataset at with just one 'proc report'                              
                                                                                                                 
                                         Type                MSRP                                                
        MAKE           ORIGIN  TYPE       Seq      MSRP      SEQ                                                 
        Acura          Asia    SUV          1   $36,945        1                                                 
                               Sedan        2  $173,860        2                                                 
                               Sports       3   $89,765        3                                                 
        Acura                                  $300,570                                                          
                                                                                                                 
        Audi           Europe  Sedan        1  $534,400        4                                                 
                               Sports       2  $198,520        5                                                 
                               Wagon        3   $89,930        6                                                 
        Audi                                    $822,850                                                         
                                                                                                                 
     github                                                                                                      
     https://tinyurl.com/wfntuxl                                                                                 
     https://communities.sas.com/t5/SAS-Procedures/Can-I-show-row-line-number-in-PROC-REPORT/m-p/606838          
                                                                                                                 
     For production I suggest you precompute and remove the compute blocks,                                      
     But proc means/summary seem to require an additional 'proc sort' and                                        
     a datastep?                                                                                                 
                                                                                                                 
    *_                   _                                                                                       
    (_)_ __  _ __  _   _| |_                                                                                     
    | | '_ \| '_ \| | | | __|                                                                                    
    | | | | | |_) | |_| | |_                                                                                     
    |_|_| |_| .__/ \__,_|\__|                                                                                    
            |_|                                                                                                  
    ;                                                                                                            
                                                                                                                 
    data cars;                                                                                                   
      set sashelp.cars(where=(Make in ("Acura", "Audi")) keep=make type origin msrp);                            
    run;quit;                                                                                                    
                                                                                                                 
     WORK.CARS total obs=26                                                                                      
                                                                                                                 
       MAKE     TYPE      ORIGIN     MSRP                                                                        
                                                                                                                 
       Acura    SUV       Asia      36945                                                                        
       Acura    Sedan     Asia      23820                                                                        
       Acura    Sedan     Asia      26990                                                                        
       Acura    Sedan     Asia      33195                                                                        
       Acura    Sedan     Asia      43755                                                                        
       Acura    Sedan     Asia      46100                                                                        
       Acura    Sports    Asia      89765                                                                        
       Audi     Sedan     Europe    25940                                                                        
       Audi     Sedan     Europe    35940                                                                        
       Audi     Sedan     Europe    31840                                                                        
       Audi     Sedan     Europe    33430                                                                        
       Audi     Sedan     Europe    34480                                                                        
       Audi     Sedan     Europe    36640                                                                        
       Audi     Sedan     Europe    39640                                                                        
       Audi     Sedan     Europe    42490                                                                        
       Audi     Sedan     Europe    44240                                                                        
       Audi     Sedan     Europe    42840                                                                        
       Audi     Sedan     Europe    49690                                                                        
       Audi     Sedan     Europe    69190                                                                        
       Audi     Sedan     Europe    48040                                                                        
       Audi     Sports    Europe    84600                                                                        
       Audi     Sports    Europe    35940                                                                        
       Audi     Sports    Europe    37390                                                                        
       Audi     Sports    Europe    40590                                                                        
       Audi     Wagon     Europe    40840                                                                        
       Audi     Wagon     Europe    49090                                                                        
                                                                                                                 
    *            _               _                                                                               
      ___  _   _| |_ _ __  _   _| |_                                                                             
     / _ \| | | | __| '_ \| | | | __|                                                                            
    | (_) | |_| | |_| |_) | |_| | |_                                                                             
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                            
                    |_|                                                                                          
    ;                                                                                                            
                                                                                                                 
                                                                                                                 
    OUTPUT DATASET                                                                                               
                                                                                                                 
    WORK.WANT total obs=8                                                                                        
                                                                                                                 
      MAKE     ORIGIN    TYPE      TYPE_SEQ     MSRP     MSRP_SEQ    _BREAK_                                     
                                                                                                                 
      Acura    Asia      SUV           1        36945        1                                                   
      Acura    Asia      Sedan         2       173860        2                                                   
      Acura    Asia      Sports        3        89765        3                                                   
      Acura                                    300570                 TOTAL                                      
      Audi     Europe    Sedan         1       534400        4                                                   
      Audi     Europe    Sports        2       198520        5                                                   
      Audi     Europe    Wagon         3        89930        6                                                   
      Audi                                     822850                 TOTAL                                      
                                                                                                                 
     AND a report                                                                                                
                                                                                                                 
                                         Type                MSRP                                                
        MAKE           ORIGIN  TYPE       Seq      MSRP      SEQ                                                 
        Acura          Asia    SUV          1   $36,945        1                                                 
                               Sedan        2  $173,860        2                                                 
                               Sports       3   $89,765        3                                                 
        Acura                                  $300,570                                                          
                                                                                                                 
        Audi           Europe  Sedan        1  $534,400        4                                                 
                               Sports       2  $198,520        5                                                 
                               Wagon        3   $89,930        6                                                 
        Audi                                   $822,850                                                          
                                                                                                                 
    *                                                                                                            
     _ __  _ __ ___   ___ ___  ___ ___                                                                           
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|                                                                          
    | |_) | | | (_) | (_|  __/\__ \__ \                                                                          
    | .__/|_|  \___/ \___\___||___/___/                                                                          
    |_|                                                                                                          
    ;                                                                                                            
                                                                                                                 
    options missing=" ";                                                                                         
    proc report data=cars nowd missing out=want(rename=(                                                         
       Tobs=Type_Seq obs=MSRP_seq)) threads missing nowd                                                         
          out=myrptdata missing nowd;                                                                            
          column  Make origin  type tobs MSRP  obs;                                                              
          define Make / group;                                                                                   
          define Type / group;                                                                                   
          define origin / group;                                                                                 
          define MSRP / analysis sum;                                                                            
          compute tobs;                                                                                          
            tnobs+1;                                                                                             
            tobs=put(tnobs,1.);                                                                                  
            if type="" then do;                                                                                  
                tobs= " ";                                                                                       
                tnobs=0;                                                                                         
            end;                                                                                                 
          endcomp;                                                                                               
          compute obs;                                                                                           
            nobs+1;                                                                                              
            obs=put(nobs,1.);                                                                                    
            if type="" then do;                                                                                  
                obs= " ";                                                                                        
                nobs=nobs-1;                                                                                     
            end;                                                                                                 
          endcomp;                                                                                               
          break after make / summarize;                                                                          
    run;quit;                                                                                                    
                                                                                                                 
