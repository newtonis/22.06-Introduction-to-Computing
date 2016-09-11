################################################################################
#                                                                              #
#    Micro Series 6801 Assembler V2.00/DOS              11/Sep/16  12:02:40    #
#                                                                              #
#       Source   =   ej7.msa                                                   #
#       List     =   ej7.lst                                                   #
#       Object   =   ej7.r07                                                   #
#       Options  =                                                             #
#                                                                              #
#                                               (c) Copyright IAR Systems 1990 #
################################################################################


                      ** Definimos los rangos a contar e la forma [A,B)
       1000           R1_A    EQU     $1000
       2000           R1_B    EQU     $2000
                      
       4000           R2_A    EQU     $4000
       4040           R2_B    EQU     $4040
                      
                      ** Memorias donde se almacena la respuesta
       3001           LMEM    EQU     $3001
       3000           HMEM    EQU     $3000
                      
                      ** Programa principal
       0000                   p68H11                  
       2000                   ORG     $2000      
                      *cargamos mitad de pares
       2000 C600              ldab    #0
       2002 CE1000            ldx     #$1000
       2005 E700      tstart  stab    0,X
       2007 5C                incb
       2008 08                inx
       2009 8C2001            cpx     #$2001
       200C 26F7              BNE     tstart
                       
                      *cargamos mitad de pares 2
       200E C600              ldab    #0
       2010 CE4000            ldx     #$4000
       2013 E700      bstart  stab    0,X
       2015 5C                incb
       2016 08                inx
       2017 8C4040            cpx     #$4040
       201A 26F7              BNE     bstart
                      
                      
                      * borramos memorias donde se almacena la respuesta
       201C 7F3001            clr     LMEM
       201F 7F3000            clr     HMEM
                      * Cargamos rango de comienzo de lectura
       2022 CE1000            ldx     #R1_A
                      
                      ** Iniciamos a contar
       2025 A600      inicio  ldaa    0,X
       2027 47                asra
       2028 2508              bcs     incxl   ** si hay carry puedo saltear aumentar en uno la respuesta (es impar)
                      
                      ** Como es par, entonces tengo que sumar uno a la respuesta
       202A 7C3001            inc     LMEM    **incremento memoria menos significativa
       202D 2603              bne     incxl   **si no se fue de rango, entonces no necesito incrementar HMEM
       202F 7C3000            inc     HMEM    ** incremento memoria mas significativa
                      
       2032 08        incxl   inx
       2033 8C2000            cpx     #R1_B   ** revisamos si estamos en R1_B y necesitamos saltear a R2_A
       2036 2605              BNE     chkfnl  ** Si pasamos este checkeo, ahora vamos a revisar si hay que terminar el programa
       2038 CE4000            ldx     #R2_A   **Si no lo pasmos, entonces saltamos de R1_B a R2_A
       203B 20E8              BRA     inicio
       203D 8C4040    chkfnl  cpx     #R2_B   ** revisamos si estamos en R2_B, y por ende tenemos que terminar el programa
       2040 26E3              BNE     inicio  ** Si no sucede esto podemos volver a empezar
       2042 3F                swi
                              
       2043                   END
     Errors:  None           #######
     ytes:   67             # ej7 #
     RC:     5127           #######
 