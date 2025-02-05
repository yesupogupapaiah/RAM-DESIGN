### CODE


// Code your design here
module simple_sram (
    input clk,             // Clock input
    input reset,           // Reset input
    input [7:0] address,   // 8-bit address input (256 addresses)
    input [31:0] data_in,  // 32-bit data input for write operation
    input write_enable,    // Write enable signal
    input read_enable,     // Read enable signal
    output reg [31:0] data_out // 32-bit data output for read operation
);

// Memory with 256 locations, each 32-bits wide
reg [31:0] ram [255:0];

// Process to handle write and read operations
always @(posedge clk or posedge reset) begin
    if (reset) begin
        // Reset the output data to 0 when reset is high
        data_out <= 32'b0;
    end else begin
        if (write_enable) begin
            // Write data to memory at the specified address
            ram[address] <= data_in;
        end
        if (read_enable) begin
            // Read data from memory at the specified address
            data_out <= ram[address];
        end
    end
end

endmodule


### TESTBENCH


module testbench;

// Inputs
reg clk;
reg reset;
reg [7:0] address;
reg [31:0] data_in;
reg write_enable;
reg read_enable;

// Outputs
wire [31:0] data_out;

// Instantiate the synchronous RAM module
simple_sram uut (
    .clk(clk),
    .reset(reset),
    .address(address),
    .data_in(data_in),
    .write_enable(write_enable),
    .read_enable(read_enable),
    .data_out(data_out)
);

// Clock generation
always begin
    #5 clk = ~clk; // 10ns clock period
end

// Test scenario
initial begin
    // Initialize inputs
    clk = 0;
    reset = 1;
    write_enable = 0;
    read_enable = 0;
    address = 0;
    data_in = 0;

    // Enable waveform dumping to generate a VCD file
    $dumpfile("dump.vcd");   // Create a VCD file named dump.vcd
    $dumpvars(0, uut);       // Dump all variables for the module 'uut'

    // Apply reset
    #10 reset = 0;

    // Write data to memory at address 5
    #10 address = 8'h05;
        data_in = 32'hABCD1234;
        write_enable = 1;
        read_enable = 0;
    
    // Wait for write operation to complete
    #10 write_enable = 0;

    // Read data from memory at address 5
    #10 read_enable = 1;
    
    // Wait for read operation to complete
    #10 read_enable = 0;
    
    // Write another data to memory at address 10
    #10 address = 8'h0A;
        data_in = 32'h1234ABCD;
        write_enable = 1;
        read_enable = 0;

    // Wait for write operation to complete
    #10 write_enable = 0;

    // Read data from memory at address 10
    #10 read_enable = 1;

    // Wait for read operation to complete
    #10 read_enable = 0;

    // Finish the simulation
    #20 $finish;
end

// Monitor output
initial begin
    $monitor("Time=%0t | Address=0x%h | Data In=0x%h | Data Out=0x%h", $time, address, data_in, data_out);
end

endmodule
