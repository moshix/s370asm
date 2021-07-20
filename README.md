S370Asm assembler by Jim Salvino with some changes by me
=======================================================

Jim Salvino created an amazing yet surprisingly simple S/370 assembler in Python3. It is probably the best S/370 assembler learning system out there. 

Once he announced it, I immediately started playing with it. We found a few small issues which Jim fixed immediately. 

then I added some very small mods to the assembler and created a simple report writer from my youtube videos and here is the repo. 

Great learning tool. Thank you Jim!


How to use this assembler
========================

write your assembler program without saving / restoring the caller's registers on entry/exit. There are no macros, only SVCs (see 
the report.s370 example). 

All S/370 instructions are correclty assembled and emulated. 

assembe with the proviced asm script but first adapt to your directory structure

then run with the script runasm script, after adpating for your directory structure. 

 The first two statements in the source code MUST 
 establish your base register as follows:

 BALR  R12,0    (base reg can be anything you want) 
 USING *,R12

 Do NOT save registers 14-12 at beginning
 or restore them at the end as you would normally do.

 A branch to register 14 is required at the end
 of your program (see the sample below). If you 
 plan on using reg 14 in your program then you MUST
 save it prior to using it, then restore it before
 issuing the branch to reg 14 to exit.

 Multiple base registers are NOT supported.

 Currently DSECTs are NOT supported.

 Conditional assembly is NOT supported.

 The only macros supported are: WTO, OPEN, CLOSE, GET, PUT
 
 
 

Example program
==============
<pre>
         PRINT ON,NOGEN
REPORT   CSECT
*                                                 EXAMPLE BY MOSHIX
         BALR  R12,0                              SET UP MY                       
         USING *,R12                              BASE REGISTER         
*---------------------------------------------------------------------*
INOPEN   OPEN  (INDCB,INPUT)
*
OUOPEN   OPEN  (OUTDCB,OUTPUT)
*
* WRITE OUTPUT RECORD
WTITLE   PUT   OUTDCB,PTITLE
         WTO   PTITLE
*---------------------------------------------------------------------*
READREC  GET   INDCB,PAYREC                       READ IN EMPLOYEE REC
*---------------------------------------------------------------------*
* PRINT ALSO ON SCREEN 
         WTO   PAYREC
* 
CPYSTUFF MVC   PEMPID,EMPID                                            
         MVC   PEMPLOYE,EMPLOYEE                                       
         MVC   PSALARY,SALARY                                          
*                                                                      
PACKIT   PACK  ZSALARY,SALARY                     PACK SALARY                     
         AP    ZTOTSAL,ZSALARY                    ADD MONTHLY WAGE TO
WRITEPR  PUT   OUTDCB,OUTAREA                     WRITE TO PRINTER
         B     READREC                            AND REPEAT TILL FILE
*---------------------------------------------------------------------*
INCLOS   MVC   ATOTAL,EDWD                                              
*        LA    R1,ATOTAL                                                
         ED    ATOTAL,ZTOTSAL                                           
         PUT   OUTDCB,TOTALLNE                    PRINT TOTAL LINE
* PRINT TOTAL ALSO ON SCREEN
         WTO   TOTALLNE
*
CLSALL   CLOSE (INDCB)                            WE GET HERE FROM EODAD
         CLOSE (OUTDCB)
*
         BR    R14
         LTORG                                                         
*---------------------------------------------------------------------*
INDCB    DCB   MACRF=GM,DDNAME=INDD,DSORG=PS,EODAD=INCLOS                
OUTDCB   DCB   MACRF=PM,DDNAME=OUTDD,DSORG=PS                            
*                          PAYROLL REPORT STRUCTURE                    
PAYREC   DS    0CL80                              HANDLE FOR THE STRU
EMPID    DS    CL4                                EMPLOYEE ID                                 
         DS    CL6                                FILLER TO POSITION10                       
EMPLOYEE DS    CL21                               NAME OF EMPLOYEE                            
         DS    CL2                                FILLER TO POSITION34                       
SALARY   DS    CL4                                MONTHLY SALARY                              
TOEND    DS    CL43                               80 BYTES SO FAR                             
*--------S-T-A-R-T----O-F----O-U-T-P-U-T----S-T-R-U-C-T-U-R-E---------*
PTITLE   DS    0CL121 
         DC    CL27' P A Y R O L L  R E P O R T'
         DC    CL20'  -  B I M  C O R P.'
         DC    CL74' '
OUTAREA  DS    0CL133
EMPTY    DC    CL1' '                                                  
PEMPID   DC    CL4' '                                                  
         DC    CL6' '                                                  
PEMPLOYE DC    CL20' '                                                 
         DC    CL2' '                                                  
PDOLLAR  DC    CL1' '                                                  
PSALARY  DC    CL5' '                                                  
OFILLER  DC    CL94' '                                                 
*                                                                      
ZSALARY  DC    PL3'0'                             INITIALIZE SLARY PAC
ZTOTSAL  DC    PL05'0'                            INITIALIZE TOTAL WA
EDWD     DC    X'402020202020206B202020'
TOTALLNE DS    0CL133                                                  
SKIP     DC    CL1'0'                                                  
TFILL1   DC    CL09' '                                                 
TFILL2   DC    CL15' '                                                 
TDOLLAR  DC    CL1'$'                                                  
ATOTAL   DC    CL11' '                                                 
TFILL3   DC    CL5' '                                                 
TOTMSG   DC    CL61'TOTAL MONTHLY WAGES'                               
TFILL4   DC    CL30' '  
*
R0       EQU   0                                                       
R1       EQU   1                                                       
R2       EQU   2                                                       
R3       EQU   3                                                       
R4       EQU   4                                                       
R5       EQU   5                                                       
R6       EQU   6                                                       
R7       EQU   7                                                       
R8       EQU   8                                                       
R9       EQU   9                                                       
R10      EQU   10                                                      
R11      EQU   11                                                      
R12      EQU   12                                                      
R13      EQU   13                                                      
R14      EQU   14                                                      
R15      EQU   15                                                      
         END                                                           
</pre>

then export INDD and OUTDD with:
<pre>
export INDD=SALARIES.INPUT
export OUTDD=REPORT.OUTPUT 
</pre>


Assembler with the provided asm script. Then run with the provided runasm script, after adapting both for your directory structure. 

Runs correctly on Windoz, Linuchs, FreeBSD and Macoz
<p><p>
Moshix

July 20, 2021

