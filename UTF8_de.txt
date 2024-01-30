.data
input: .word 0x009eaae8     ## Your input in register $a3

.text

lw   $a3, input           ## Load the input into register $t0

andi $t4, $a3, 0xff       ## Contains furthest right two hex units
sll  $t4, $t4, 16	  ## Contains furthest right two hex units

andi $t5, $a3, 0xff00     ## Contains middle two hex units

srl  $t6, $a3, 16         ## Contains furthest left two hex units

andi $a3, $a3, 0x000000   ## Clears register a3

or   $a3, $t4, $t5
or   $a3, $a3, $t6        ## Changes input to correct order

srl  $t0, $a3, 16         ## Extract the first 8 bits
andi $t0, $t0, 0x0f       ## Ensure only the first four bits remain

srl  $t1, $a3, 8          ## Extract the next 8 bits
andi $t1, $t1, 0x3f       ## Ensure only the first six bits remain

srl  $t2, $a3, 0          ## Extract the last 8 bits
andi $t2, $t2, 0x3f       ## Ensure only the first six bits remain

sll $t0, $t0, 6           ## Move the first 4 bits left by 6 to make space for the next 6 bits
or  $t3, $t0, $t1         ## Combine the bits from the first 4 and 6 bits

sll $t3, $t3, 6     ## Move the combined bits up by 6 to make space for the next 6 bits
or  $a2, $t3, $t2   ## Combine the first 10 bits with the next 6 bits and place in R6


li $v0, 10
syscall