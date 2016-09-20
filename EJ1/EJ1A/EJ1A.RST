################################################################################
#                                                                              #
#    Micro Series 6801 Assembler V2.00/DOS              08/Sep/16  22:21:43    #
#                                                                              #
#       Source   =   ej1.msa                                                   #
#       List     =   ej1.lst                                                   #
#       Object   =   ej1.r07                                                   #
#       Options  =                                                             #
#                                                                              #
#                                               (c) Copyright IAR Systems 1990 #
################################################################################


                      * Este programa lee un dato de la variable IN, 
                      * le realiza dos shift y lo vuelve a escribir en OUT            
       2021           IN  EQU $2021 
       2020           OUT EQU $2020
       0000                   p68H11                  
       2000                   ORG     $2000          
       2000 B62021            LDAA IN
       2003 48                ASLA
       2004 48                ASLA
       2005 B72020            STAA OUT
       2008                   END
     Errors:  None           #######
     ytes:   8              # ej1 #
     RC:     986E           #######
 