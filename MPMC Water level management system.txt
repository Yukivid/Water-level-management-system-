ORG 0000H
CLR P3.0                     ;Relay
CLR P1.0                     ;Full
CLR P1.1                     ;Half
CLR P1.2                     ;Empty
ACALL INITIAL
ACALL HEAD

EMPTY: JNB P1.2,EMPTY
SETB P3.0
ACALL DELAY 				; give LCD some time
MOV A, #69                        ; display letter E
ACALL DATAWRT            ; call display subroutine
ACALL DELAY                  ; give LCD some time
MOV A, #77                        ; display letter M                        
ACALL DATAWRT            ; call display subroutine
ACALL DELAY                  ; give LCD some time
MOV A, #84                        ; display letter T
ACALL DATAWRT            ; call display subroutine
ACALL DELAY                  ; give LCD some time
MOV A, #89                        ; display letter Y
ACALL DATAWRT 			; call display subroutine
ACALL DELAY
MOV A, #' ' 
ACALL DATAWRT 			; call display subroutine
ACALL DELAY
MOV A,#'M'
ACALL DATAWRT 			; call display subroutine
ACALL DELAY
MOV A,#'1'
ACALL DATAWRT
ACALL DELAY

HALF: JNB P1.1,HALF;
ACALL DELAY 					; give LCD some time
MOV A, #72                             ; display letter H
ACALL DATAWRT                 ; call display subroutine
ACALL DELAY                       ; give LCD some time
MOV A, #65                             ; display letter A
ACALL DATAWRT                 ; call display subroutine
ACALL DELAY                       ; give LCD some time
MOV A, #76                             ; display letter L
ACALL DATAWRT                 ; call display subroutine
ACALL DELAY                       ; give LCD some time
MOV A, #70                             ; display letter F
ACALL DATAWRT                  ; call display subroutine
ACALL DELAY 					; give LCD some time
MOV A, #' ' 
ACALL DATAWRT 			; call display subroutine
ACALL DELAY
JNB P3.0,MOTOROFF
MOV A,#'M'
ACALL DATAWRT 			; call display subroutine
ACALL DELAY
MOV A,#'1'
ACALL DATAWRT
ACALL DELAY
SJMP FULL
MOTOROFF: MOV A,#'M'
ACALL DATAWRT 			; call display subroutine
ACALL DELAY
MOV A,#'0'
ACALL DATAWRT
ACALL DELAY
SJMP EMPTY
FULL: JNB P1.0,FULL;
CLR P3.0                            ;restricting the current flow
ACALL DELAY                 ; give LCD some time
MOV A, #70                       ; display letter F
ACALL DATAWRT           ; call display subroutine
ACALL DELAY                 ; give LCD some time
MOV A, #85                       ; display letter U
ACALL DATAWRT           ; call display subroutine
ACALL DELAY                 ; give LCD some time
MOV A, #76                       ; display letter L
ACALL DATAWRT           ; call display subroutine
ACALL DELAY                 ; give LCD some time
MOV A, #76                       ; display letter L
ACALL DATAWRT           ; call display subroutine
ACALL DELAY                 ; give LCD some time
MOV A, #' ' 
ACALL DATAWRT 			; call display subroutine
ACALL DELAY
MOV A,#'M'
ACALL DATAWRT 			; call display subroutine
ACALL DELAY
MOV A,#'0'
ACALL DATAWRT
ACALL DELAY
COMNWRT:                 ; send command to LCD
MOV P0 , A                   ; copy reg A to port 1
CLR P2.7                       ; RS=0 for command
CLR P2.6                       ; R/W’=0 for write
SETB P2.5                     ; E=1 for high pulse
ACALL DELAY            ; give LCD some time
CLR P2.5                       ; E=0 for H-to-L pulse
RET

DATAWRT:                    ; write data to LCD
MOV P0, A                     ; copy reg A to port 1
SETB P2.7                      ; RS=1 for data
CLR P2.6                        ; R/W=0 for write
SETB P2.5                      ; E=1 for high pulse
ACALL DELAY             ; give LCD some time
CLR P2.5                        ; E=0 for H-to-L pulse
RET
HEAD:
MOV A,#'W'
ACALL DATAWRT              
ACALL DELAY 				; give LCD some time
MOV A,#'A'
ACALL DATAWRT               
ACALL DELAY 				; give LCD some time
MOV A,#'T'
ACALL DATAWRT                
ACALL DELAY 				; give LCD some time
MOV A,#'E'
ACALL INITIAL                ; starting point command
ACALL DELAY 				; give LCD some time
MOV A,#'R'
ACALL DATAWRT               
ACALL DELAY 				; give LCD some time
MOV A,#'L'
ACALL DATAWRT                
ACALL DELAY 				; give LCD some time
MOV A,#'V'
ACALL DATAWRT                
ACALL DELAY 				; give LCD some time
MOV A,#'L'
ACALL DATAWRT                
ACALL DELAY 				; give LCD some time
MOV A,#':'
ACALL DATAWRT                
ACALL DELAY 				; give LCD some time
RET

DELAY: MOV R3, #50             ; 50 or higher for fast CPUs
HERE2: MOV R4, #255          ; R4 = 255
HERE: DJNZ R4, HERE            ; stay until R4 becomes 0
DJNZ R3, HERE2                      ; stay until R3 becomes 0
RET
INITIAL:                                    ;starting point command
MOV A, #38H                          ; INITIALIZE 2x16 LCD
ACALL COMNWRT                  ; call command subroutine
ACALL DELAY                           ; give LCD some time
MOV A, #0EH                          ; display on, cursor on
ACALL COMNWRT                  ; call command subroutine
ACALL DELAY                           ; give LCD some time
MOV A, #01                             ; clear LCD
ACALL COMNWRT                  ; call command subroutine
ACALL DELAY                           ; give LCD some time
MOV A, #06H                          ; shift cursor right
ACALL COMNWRT                  ; call command subroutine
ACALL DELAY                           ; give LCD some time
MOV A, #80H                          ; cursor at line 1, pos. 4
ACALL COMNWRT                  ; call command subroutine
RET
END