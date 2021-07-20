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
         BALR  R12,0 
         USING *,R12
         LA    R3,AREA1
         LA    R4,4
LOOP     MVI   0(R3),C'0'
         LA    R3,1(,R3)
         BCT   R4,LOOP
         LA    R15,0
         BR    R14
AREA1    DC    XL4'FFFFFFFF'
R0       EQU   0
...
R15      EQU   15
         END            

</pre>


Moshix

July 20, 2021

