# Random-number-generation-using-LFSR
Random number generation using linear feedback shift register
**#VERILOG CODE :**
module LFSR(input clk, rst, output reg [3:0] op);
  // Module declaration for a 4-bit Linear Feedback Shift Register (LFSR)
  // Inputs: clk (clock), rst (reset)
  // Output: op (4-bit output register)

  always @(posedge clk) begin
    // Trigger this always block on the rising edge of the clock signal

    if (rst) 
      op <= 4'hf;
      // If reset signal (rst) is high, initialize the output register (op) to 4'hf (hexadecimal 15, binary 1111)

    else 
      op = {op[2:0], (op[3] ^ op[2])};
      // Otherwise, shift the output register to the left by 1 bit
      // Concatenate the 3 least significant bits (op[2:0]) with the XOR of the 2 most significant bits (op[3] and op[2])
      // This creates the new value for the output register (op), implementing the feedback mechanism
  end
endmodule


**TESTBENCH CODE :**
module TB;
  // Testbench module declaration for testing the LFSR

  reg clk, rst;
  // Declare clk and rst as reg type (to allow them to be driven by the testbench)

  wire [3:0] op;
  // Declare op as wire type (to connect to the output of the LFSR)

  LFSR lfsr1(clk, rst, op);
  // Instantiate the LFSR module with clk, rst, and op signals

  initial begin
    // Initial block starts, runs once at the beginning of the simulation

    $monitor("op=%b", op);
    // Monitor and print the value of op in binary format whenever it changes

    clk = 0; rst = 1;
    // Initialize clk to 0 and rst to 1 (activate reset)

    #5 rst = 0;
    // After 5 time units, deactivate the reset (rst = 0)

    #50; $finish;
    // After 50 more time units, end the simulation
  end

  always #2 clk = ~clk;
  // Always block to generate a clock signal
  // Toggle clk every 2 time units to create a clock with a period of 4 time units

  initial begin
    // Another initial block, runs once at the beginning of the simulation

    $dumpfile("dump.vcd"); $dumpvars;
    // Generate a dump file (dump.vcd) and record all signal changes for waveform viewing
  end
endmodule

