Name: ADITYA JANGIR<br />
Company: CODTECH IT SOLUTIONS<br />
ID: :CT08DS2335<br />
Domain: VLSI<br />
Duration: (4 Weeks) JUNE 15th,2024 to JULY 15th, 2024<br />
Mentor: NEELA SANTHOSH KUMAR<br />

# Overview of the Project
# TASK FOUR: SPI (SERIAL PERIPHERAL INTERFACE) CONTROLLER DESIGN

Design an SPI controller module in Verilog or VHDL using the
VLSI software. Simulate the SPI controller module to verify its
operation. Use the VLSI software’s debugging features to
observe SPI data transactions.

## SPI module

```
module spi_state(
input wire clk,  //System clock
input wire reset, //Asynchronous system reset
input wire [15:0] datain,  //Binary input vector
output wire spi_cs_L, //SPI Active-Low chip select
output wire spi_sclk, //SPI bus clock
output wire spi_data, //SPI bus data
output [4:0]counter
);
//reg dclk
reg [15:0] MOSI;
reg [4:0] count;
reg cs_L;
reg sclk;
reg [2:0] state;

always @(posedge clk or posedge reset)
if (reset)
begin 
MOSI <= 16'b0;
count <= 5'd16;
cs_L <= 1'b1;
sclk <= 1'b0;
end

else begin
case (state)
0:begin
sclk <= 1'b0;
cs_L <= 1'b1;
state <= 1;
end

1:begin
sclk <= 1'b0;
cs_L <= 1'b0;
MOSI <= datain[count-1];
count <= count-1;
state <= 2;
end

2:begin
sclk <= 1'b1;
if(count>0)
state <= 1;
else begin
count <= 16;
state <= 0;
end
end

default:state<=0;

endcase
end
assign spi_cs_L = cs_L;
assign spi_sclk = sclk;
assign spi_data = MOSI;
assign counter = count;
endmodule
```

## SPI testbench
```
module spi_tb;

// Inputs
reg clk;
reg reset;
reg [15:0]datain;


// Outputs
wire spi_cs_l;
wire spi_sclk;
wire spi_data;
wire [4:0]counter;

spi_state dut (
    .clk(clk),
    .reset(reset),
    .counter(counter),
    .datain(datain),
    .spi_cs_L(spi_cs_l),
    .spi_sclk(spi_sclk),
    .spi_data(spi_data)
);

initial begin
    clk = 0;
    reset = 1;
    datain = 0;
end

always #5 clk=~clk;
initial begin
    #10 reset=1'b0;
    #10 datain=16'hA569;
    #335 datain=16'h2563;
    #335 datain=16'h9B63;
    #335 datain=16'h6A61;
    #335 datain=16'hA265;
    #335 datain=16'h7564;
end
endmodule
```
![Screenshot (145)](https://github.com/harris8099/CODTECH-Task2/assets/108947643/720baf69-c31a-4964-bc81-7508f09052a1)


