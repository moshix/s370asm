S370Asm assembler by Jim Salvino with some changes by me
=======================================================

Jim Salvino created an amazing yet surprisingly simple S/370 assembler in Python3. 

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


Moshix

July 17, 2021

