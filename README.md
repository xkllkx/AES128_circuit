# AES circuit (Advanced Encryption Standard AES128) in Verilog

# Introduction:
The Advanced Encryption Standard (AES) specifies a FIPS-approved
cryptographic algorithm that can be used to protect electronic data. The AES algorithm is a
symmetric block cipher that can encrypt (encipher) and decrypt (decipher) information.
Encryption converts data to an unintelligible form called ciphertext; decrypting the ciphertext
converts the data back into its original form, called plaintext.
The AES algorithm is capable of using cryptographic keys of 128, 192, and 256 bits to encrypt
and decrypt data in blocks of 128 bits

<img src="https://github.com/xkllkx/AES-circuit/blob/main/AES128.png" width="50%" height="50%">

## Links
- [Wikipedia](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)

# Design
## 🔐AES Encryption：
```verilog
module AES(
    input clk,
    input rst,
    input [127:0] P,
    input [127:0] K,
    output [127:0] C,
    output valid
    );

// ------------------------
// Parameters
// ------------------------

wire [127:0] Sub_Key [9:0];
wire [127:0] State [8:0];

// ------------------------
// calculation 
// ------------------------

keyExpansion K1(.clk(clk), .Key(K), .round(4'd1), .Sub_Key(Sub_Key[0]));
keyExpansion K2(.clk(clk), .Key(Sub_Key[0]), .round(4'd2), .Sub_Key(Sub_Key[1]));
keyExpansion K3(.clk(clk), .Key(Sub_Key[1]), .round(4'd3), .Sub_Key(Sub_Key[2]));
keyExpansion K4(.clk(clk), .Key(Sub_Key[2]), .round(4'd4), .Sub_Key(Sub_Key[3]));
keyExpansion K5(.clk(clk), .Key(Sub_Key[3]), .round(4'd5), .Sub_Key(Sub_Key[4]));
keyExpansion K6(.clk(clk), .Key(Sub_Key[4]), .round(4'd6), .Sub_Key(Sub_Key[5]));
keyExpansion K7(.clk(clk), .Key(Sub_Key[5]), .round(4'd7), .Sub_Key(Sub_Key[6]));
keyExpansion K8(.clk(clk), .Key(Sub_Key[6]), .round(4'd8), .Sub_Key(Sub_Key[7]));
keyExpansion K9(.clk(clk), .Key(Sub_Key[7]), .round(4'd9), .Sub_Key(Sub_Key[8]));
keyExpansion K10(.clk(clk), .Key(Sub_Key[8]), .round(4'd10), .Sub_Key(Sub_Key[9]));

Encryption E1(.clk(clk), .State(P^K), .Sub_Key(Sub_Key[0]), .State_n(State[0]));
Encryption E2(.clk(clk), .State(State[0]), .Sub_Key(Sub_Key[1]), .State_n(State[1]));
Encryption E3(.clk(clk), .State(State[1]), .Sub_Key(Sub_Key[2]), .State_n(State[2]));
Encryption E4(.clk(clk), .State(State[2]), .Sub_Key(Sub_Key[3]), .State_n(State[3]));
Encryption E5(.clk(clk), .State(State[3]), .Sub_Key(Sub_Key[4]), .State_n(State[4]));
Encryption E6(.clk(clk), .State(State[4]), .Sub_Key(Sub_Key[5]), .State_n(State[5]));
Encryption E7(.clk(clk), .State(State[5]), .Sub_Key(Sub_Key[6]), .State_n(State[6]));
Encryption E8(.clk(clk), .State(State[6]), .Sub_Key(Sub_Key[7]), .State_n(State[7]));
Encryption E9(.clk(clk), .State(State[7]), .Sub_Key(Sub_Key[8]), .State_n(State[8]));
Encryption_out E10(.clk(clk), .rst(rst), .State(State[8]), .Sub_Key(Sub_Key[9]), .State_n(C), .valid(valid));

endmodule
```

