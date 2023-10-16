![image](https://github.com/udayM-design/RISC-V/assets/93391726/b7ec083c-5a90-4552-87b6-3c18ed6fc02e)

<details>
<summary>DAY-3 : Digital Logic with TL-Verilog in Makerchip IDE</summary>
<br>

## Task-1 : Logic Gates
![image](https://github.com/udayM-design/RISC-V/assets/93391726/ae239f62-c7a5-4ea1-a1a8-7e47f02c7805)

## Task-2 : Lab - Makerchip platfrom
To use Makerchip IDE, you need to visit makerchip website at http://makerchip.com/ and launch Makerchip IDE To access a specific example.
![image](https://github.com/udayM-design/RISC-V/assets/93391726/68f522e8-b80d-4d56-bebf-d45e1a93facc)

###  Load FGPA Multiplier Example
![image](https://github.com/udayM-design/RISC-V/assets/93391726/175a6b3b-ef6d-4eb1-8075-5d431ffc2beb)

## Task-3 : Lab - Combitional logic
#### A) Inverter
![image](https://github.com/udayM-design/RISC-V/assets/93391726/617da42b-631e-4bdc-bc79-25c5ba6a66fe)

#### B) XOR Gate
```
$out = ! $in;
$out1 = ($in1 ^ $in2);
```
![image](https://github.com/udayM-design/RISC-V/assets/93391726/4a81213d-f772-4d76-9077-94988bc86158)

#### C) Vectors
```
$out[4:0] = $in1[3:0] + $in2[3:0];
```
![image](https://github.com/udayM-design/RISC-V/assets/93391726/cbadfd08-1dd6-4ae1-8eef-f3c02e0ba3f4)
#### D) Mux without vector & with vectors

```
$out = $sel ? $in1 : $in2;
```
![image](https://github.com/udayM-design/RISC-V/assets/93391726/f695341a-519c-4df3-a429-5fa00da4b33b)
```
$out[7:0] = $sel ? $in1[7:0] : $in2[7:0];
```
![image](https://github.com/udayM-design/RISC-V/assets/93391726/145c6561-01dc-4224-b564-8b0af7c1fb9e)

#### E) Simple Claculator
```
$val1[31:0] = $rand1[3:0]; 
$val2[31:0] = $rand2[3:0];
$sum[31:0] = $val1 + $val2;
$diff[31:0] = $val1 - $val2;
$prod[31:0] = $val1 * $val2;
$qut[31:0] = $val1 / $val2;
$out[31:0] = $op[1] ? ($op[0] ? $qut: $prod): ($op [0] ? $diff: $sum);
```
![image](https://github.com/udayM-design/RISC-V/assets/93391726/3c5476f9-e404-4395-8436-48d3023111d1)

## Task-4 : Sequential logic
![image](https://github.com/udayM-design/RISC-V/assets/93391726/8e49d935-d0af-45c4-87d4-0fbe26dfb4e4)

#### A) Fibonacci series
```
$fib[31:0] = $reset ? 1 : (>>1$fib + >>2$fib);
```
![image](https://github.com/udayM-design/RISC-V/assets/93391726/748b8fea-42d7-4c73-a0ae-b77fd82073ad)
#### B) Up-Counter
```
$num[2:0] = $reset ? 0 : (>>1$num + 1); 
```
![image](https://github.com/udayM-design/RISC-V/assets/93391726/3617f782-54c7-48d5-90e2-001d4f98ab34)

#### C) Sequential Calculator
```
$val1[31:0] = (>>1$out); 
$val2[31:0] = $rand2[3:0]; 
$sum[31:0] = $val1 + $val2;
$diff[31:0] = $val1 - $val2;
$prod[31:0] = $val1 * $val2;
$qut[31:0] = $val1 / $val2;
$out[31:0] = $op[1] ? ($op[0] ? $qut: $prod): ($op [0] ? $diff: $sum);
```
![image](https://github.com/udayM-design/RISC-V/assets/93391726/fd4e76fa-1a16-4ab4-a442-0145cd11b75c)

## Task-5 : Pipelined logic
### A) A simple pipeline through Pythagorean example
```
`include "sqrt32.v"
|calc
      @1
         $aa_sq[31:0] = $aa[3:0] * $aa;
         $bb_sq[31:0] = $bb[3:0] * $bb;
      @2
         $cc_sq[31:0] = $aa_sq + $bb_sq;
      @3
         $cc[31:0] = sqrt($cc_sq);
```
![image](https://github.com/udayM-design/RISC-V/assets/93391726/f0b49fd9-1e12-4aed-94c3-62f467012e66)
#### B) Pipeline Implementation
```
|comp
      @1
         $err1 = $bad_input || $illegal_op;
      @2
         $err2 = $err1 || $over_flow;
      @3
         $err3 = $div_by_zero || $err2;
```
![image](https://github.com/udayM-design/RISC-V/assets/93391726/b6eaeb81-6049-4c6e-aee3-b4dd24d296ef)
## Task-6 : Validity
#### A) 2 cycle calculator with validity
```
|calc
      @0
         $reset = *reset;
         
      @1
         $val1 [31:0] = >>2$out [31:0];
         $val2 [31:0] = $rand2[3:0];
         
         $valid = $reset ? 1'b0 : >>1$valid + 1'b1;
         $valid_or_reset = $valid || $reset;
         
      ?$valid_or_reset
      @1
         $sum [31:0] = $val1 + $val2;
         $diff[31:0] = $val1 - $val2;
         $prod[31:0] = $val1 * $val2;
         $qut [31:0] = $val1 / $val2;
         
      @2
         $out [31:0] = $reset ? 32'b0 :
                      ($op[1:0] == 2'b00) ? $sum :
                      ($op[1:0] == 2'b01) ? $diff :
                      ($op[1:0] == 2'b10) ? $prod :
                                              $qut ;
```
![image](https://github.com/udayM-design/RISC-V/assets/93391726/9b7d79de-8256-4598-89b0-551b8ce2a1ac)
#### B) Distance Calculator
```
|calc
      @1
         $reset = *reset;
         
      ?$valid
         @1
            $aa_sq[31:0] = $aa[3:0] * $aa;
            $bb_sq[31:0] = $bb[3:0] * $bb;;
         @2
            $cc_sq[31:0] = $aa_sq + $bb_sq;;
         @3
            $cc[31:0] = sqrt($cc_sq);
      @4
         $total_distance[63:0] =
            $reset ? 0 :
            $valid ? >>1$total_distance + $cc :
                     >>1$total_distance;
```
![image](https://github.com/udayM-design/RISC-V/assets/93391726/c85d0374-2e8a-4f08-aa21-fc13b95a251f)
#### A) Calulator Memory
```
|calc
      @0
         $reset = *reset;
         
      @1
         $val1 [31:0] = >>2$out [31:0];
         $val2 [31:0] = $rand2[3:0];
         
         $valid = $reset ? 1'b0 : >>1$valid + 1'b1;
         $valid_or_reset = $valid || $reset;
         
      ?$valid_or_reset
      @1
         $sum [31:0] = $val1 + $val2;
         $diff[31:0] = $val1 - $val2;
         $prod[31:0] = $val1 * $val2;
         $qut [31:0] = $val1 / $val2;
         
      @2
         $mem[31:0] = $reset ? 32'b0 :
                      ($op[2:0] == 3'b101) ? $val1 : >>2$mem ;
         
         $out [31:0] = $reset ? 32'b0 :
                      ($op[2:0] == 3'b000) ? $sum :
                      ($op[2:0] == 3'b001) ? $diff :
                      ($op[2:0] == 3'b010) ? $prod :
                      ($op[2:0] == 3'b011) ? $qut  :
                      ($op[2:0] == 3'b100) ? >>2$mem : >>2$out ;
```

![image](https://github.com/udayM-design/RISC-V/assets/93391726/7afdda98-acca-4e5b-9ec2-fb5a403194e0)

[Back To Top](https://github.com/udayM-design/RISC-V/blob/main/README.md#task-1--logic-gates)
</details>


