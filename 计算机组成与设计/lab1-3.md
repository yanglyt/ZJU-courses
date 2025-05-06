![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-03-28 200238.png)

| 课程名称： | 计算机组成与设计     |
| ---------- | -------------------- |
| 姓  名：   | 杨吉祥               |
| 学  院：   | 计算机科学与技术学院 |
| 系：       | 竺可桢学院图灵班     |
| 专  业：   | 计算机科学与技术     |
| 学  号：   | 3230106222           |
| 指导教师： | 刘海风               |

<div style="page-break-after:always;"></div>

**浙江大学实验报告**

课程名称：计算机组成与设计       实验类型： 综合   

实验项目名称：简单模块设计（ALU / RegFile / 有限状态机）

学生姓名：  杨吉祥  专业：  计算机科学与技术  学号： 3230106222 

报告日期：10.3

## 一、操作方法与实验步骤

## 1.1 ALU

- ALU.v

  ```verilog
  `timescale 1ns / 1ps
  module ALU (
    input [31:0]  A,
    input [31:0]  B,
    input [3:0]   ALU_operation,
    output[31:0]  res,
    output        zero
  );
  reg [31:0]C;
  always @(*)begin
      case(ALU_operation)
      4'd0:C<=A+B;
      4'd1:C<=A-B;
      4'd2:C<=A<<B[4:0];
      4'd3:C<=($signed(A)<$signed(B))?32'b1:32'b0;
      4'd4:C<=(A<B)?32'b1:32'b0;
      4'd5:C<=A^B;
      4'd6:C<=A>>B[4:0];
      4'd7:C<=$signed(A)>>>B[4:0];
      4'd8:C<=A|B;
      4'd9:C<=A&B;
      default:C<=32'bx;
      endcase
  end
  assign res=C;
  assign zero=~res;
  endmodule
  ```

- ALU_tb.v

  ```verilog
  `timescale 1ns / 1ps
  module ALU_tb;
      reg [31:0]  A, B;
      reg [3:0]   ALU_operation;
      wire[31:0]  res;
      wire        zero;
      ALU ALU_u(
          .A(A),
          .B(B),
          .ALU_operation(ALU_operation),
          .res(res),
          .zero(zero)
      );
      initial begin
          A=32'hA5A5A5A5;
          B=32'h5A5A5A5A;
          ALU_operation =4'b1000;
          #100;
          ALU_operation =4'b1001;
          #100;
          ALU_operation =4'b0111;
          #100;
          ALU_operation =4'b0110;
          #100;
          ALU_operation =4'b0101;
          #100;
          ALU_operation =4'b0100;
          #100;
          ALU_operation =4'b0011;
          #100;
          ALU_operation =4'b0010;
          #100;
          ALU_operation =4'b0001;
          #100;
          ALU_operation =4'b0000;
          #100;
          A=32'h01234567;
          B=32'h76543210;
          ALU_operation =4'b0111;
          #100;
          A=32'b1;
          B=32'hFFFFFFFF;
          ALU_operation =4'b0000;
          #100;
          ALU_operation =4'b0001;
          #100;
          ALU_operation =4'b1111;
      end
  endmodule
  ```

  ## 1.2 Register Files

  - Regs.v

    ```verilog
    `timescale 1ns / 1ps
    module Regs(
        input clk,
        input rst,
        input [4:0] Rs1_addr, 
        input [4:0] Rs2_addr, 
        input [4:0] Wt_addr, 
        input [31:0]Wt_data, 
        input RegWrite, 
        output [31:0] Rs1_data, 
        output [31:0] Rs2_data,
        output [31:0] Reg00,
        output [31:0] Reg01,
        output [31:0] Reg02,
        output [31:0] Reg03,
        output [31:0] Reg04,
        output [31:0] Reg05,
        output [31:0] Reg06,
        output [31:0] Reg07,
        output [31:0] Reg08,
        output [31:0] Reg09,
        output [31:0] Reg10,
        output [31:0] Reg11,
        output [31:0] Reg12,
        output [31:0] Reg13,
        output [31:0] Reg14,
        output [31:0] Reg15,
        output [31:0] Reg16,
        output [31:0] Reg17,
        output [31:0] Reg18,
        output [31:0] Reg19,
        output [31:0] Reg20,
        output [31:0] Reg21,
        output [31:0] Reg22,
        output [31:0] Reg23,
        output [31:0] Reg24,
        output [31:0] Reg25,
        output [31:0] Reg26,
        output [31:0] Reg27,
        output [31:0] Reg28,
        output [31:0] Reg29,
        output [31:0] Reg30,
        output [31:0] Reg31
    );
    reg [31:0]regs[31:0];
    integer i;
    always @(posedge clk or posedge rst)begin
            if(rst) begin
                for(i=1;i<32;i=i+1)begin
                    regs[i]<=32'b0;
                end
            end
    		else if(Wt_addr!=5'd0&&RegWrite)
    			regs[Wt_addr]<=Wt_data;
    end
    assign Rs1_data=(Rs1_addr!=5'b0)?regs[Rs1_addr]:32'b0;
    assign Rs2_data=(Rs2_addr!=5'b0)?regs[Rs2_addr]:32'b0;
    assign Reg00=32'b0;
    assign Reg01=regs[1];
    assign Reg02=regs[2];
    assign Reg03=regs[3];
    assign Reg04=regs[4];
    assign Reg05=regs[5];
    assign Reg06=regs[6];
    assign Reg07=regs[7];
    assign Reg08=regs[8];
    assign Reg09=regs[9];
    assign Reg10=regs[10];
    assign Reg11=regs[11];
    assign Reg12=regs[12];
    assign Reg13=regs[13];
    assign Reg14=regs[14];
    assign Reg15=regs[15];
    assign Reg16=regs[16];
    assign Reg17=regs[17];
    assign Reg18=regs[18];
    assign Reg19=regs[19];
    assign Reg20=regs[20];
    assign Reg21=regs[21];
    assign Reg22=regs[22];
    assign Reg23=regs[23];
    assign Reg24=regs[24];
    assign Reg25=regs[25];
    assign Reg26=regs[26];
    assign Reg27=regs[27];
    assign Reg28=regs[28];
    assign Reg29=regs[29];
    assign Reg30=regs[30];
    assign Reg31=regs[31];
    endmodule
    ```

  - regs_tb.v

```verilog
`timescale 1ns / 1ns
module Regs_tb;
    reg clk;
    reg rst;
    reg [4:0] Rs1_addr; 
    reg [4:0] Rs2_addr; 
    reg [4:0] Wt_addr; 
    reg [31:0]Wt_data; 
    reg RegWrite; 
    wire [31:0] Rs1_data; 
    wire [31:0] Rs2_data;
    Regs Regs_U(
        .clk(clk),
        .rst(rst),
        .Rs1_addr(Rs1_addr),
        .Rs2_addr(Rs2_addr),
        .Wt_addr(Wt_addr),
        .Wt_data(Wt_data),
        .RegWrite(RegWrite),
        .Rs1_data(Rs1_data),
        .Rs2_data(Rs2_data)
    );

    always #10 clk = ~clk;

    initial begin
        clk = 0;
        rst = 1;
        RegWrite = 0;
        Wt_data = 0;
        Wt_addr = 0;
        Rs1_addr = 0;
        Rs2_addr = 0;
        #100
        rst = 0;
        RegWrite = 1;
        Wt_addr = 5'b00101;
        Wt_data = 32'ha5a5a5a5;
        #50
        Wt_addr = 5'b01010;
        Wt_data = 32'h5a5a5a5a;
        #50
        RegWrite = 0;
        Rs1_addr = 5'b00101;
        Rs2_addr = 5'b01010;
        #50
        Wt_addr=5'b1;
        Wt_data=32'b1;
        #50
        Rs1_addr = 5'b1;
        #50
        RegWrite=1;
        Wt_addr=5'b0;
        Wt_data=32'b1;
        #50
        Rs1_addr = 5'b0;
        #100 $stop();
    end

endmodule
```

## 1.3 Finite State Machine

- SomeFSM.v

  ```verilog
  `timescale 1ns / 1ps
  module SomeFSM(
      input clk,
      input in,
      input reset,
      output out
      );
  reg [1:0]curr_state,next_state;
  initial curr_state=2'b0;
  always @(posedge clk or posedge reset) begin
          if(reset)curr_state=2'b0;
          else curr_state=next_state;
  end
  always @(*) begin 
      case(curr_state)
        2'b00: begin
          if(1'b0 == in) next_state=2'b0;
          else next_state=2'b1;
        end
        2'b01: begin
          if(1'b0 == in) next_state=2'b0;
          else next_state=2'b10;
        end
        2'b10: begin
          if(1'b0 == in) next_state=2'b1;
          else next_state=2'b11;
        end
        default: begin
          if(1'b0 == in) next_state=2'b10;
          else next_state=2'b11;
        end
      endcase
  end
  assign out=(curr_state==2'b10)|(curr_state==2'b11);
  endmodule
  ```

- SomeFSM_tb.v

  ```verilog
  `timescale 1ns / 1ps
  module SomeFSM_tb;
  reg clk;
  reg in;
  reg reset;
  wire out;
  SomeFSM uut(
  .clk(clk),
  .in(in),
  .reset(reset),
  .out(out)
  );
  initial begin
      in=1;
      reset=0;
      #300 reset=1;
      #100 reset=0;
      #300
      in=0; 
  end
  always begin
      clk=0;#30;
      clk=1;#30;
  end
  endmodule
  ```

  # 二、实验结果与分析

  ## 2.1 ALU

  <img src="C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-18 164054.png" style="zoom: 200%;" />

​													(a)

<img src="C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-18 164546.png" style="zoom:200%;" />

​													(b)

- (a)图波形分析

  两个操作数分别为A=a5a5a5a5=1010 0101 1010 0101 1010 0101 1010 0101

  ​			       B=5a5a5a5a=0101 1010 0101 1010 0101 1010 0101 1010

  - 当ALU_operation=8时表示OR操作，res=A|B=32‘hFFFFFFFF

  - 当ALU_operation=9时表示AND操作，res=A|B=32‘h0

  - 当ALU_operation=7时表示SRA操作，res=$signed(A)>>>B[4:0]=32'hFFFFFFE9

  - 当ALU_operation=6时表示SRL操作，res=A>>B[4:0]=32’h00000029

  - 当ALU_operation=5时表示XOR操作，res=A^B=32'hFFFFFFFF

  - 当ALU_operation=4时表示SLTU操作，将A和B当作无符号数比大小，由于A>B,所以res=32‘b0

    当ALU_operation=3时表示SLT操作，将A和B当作有符号数比大小，由于A<B,所以res=32’b1

  - 当ALU_operation=2时表示SLL操作，res=A<<B[4:0]=32'h94000000

  - 当ALU_operation=1时表示SUB操作，res=A-B=32'h4b4b4b4b

  - 当ALU_operation=0时表示ADD操作，res=A+B=32'hFFFFFFFF

- (b)图波形分析

  两个操作数分别为A=00000001

  ​			       B=FFFFFFFF

  主要是用来进行边界测试

  - 当ALU_operation=0时表示ADD操作，A+B=33'h100000000，由于res是32位，所以res=32‘b0

  - 当ALU_operation=1时表示SUB操作，rses=A-B=32’b1+32'b1=32'd2

  - 当ALU_operation=F时不在范围内，res=32‘bx表示未知操作

    ## 2.2 Register Files

    <img src="C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-18 190023.png" style="zoom:200%;" />

- 一开始rst=1,将所有寄存器初始值设为0
- 100-150ns,RegWrite=1,Wt_addr=5,Wt_data=32'ha5a5a5a5,更新五号寄存器
- 150-200ns,RegWrite=1,Wt_addr=a,Wt_data=32'h5a5a5a5a,更新十号寄存器
- 200-250ns,Rs1_addr=5,Rs2_addr=a,读取五号寄存器和十号寄存器储存的数据,由Rs1_data=32'ha5a5a5a5,Rs2_data=32'h5a5a5a5a可知之前向五号和十号寄存器写入数据成功
- 250-300ns,RegWrite=0,Wt_addr=1,Wt_data=32'h1,尝试往一号寄存器中写入数据
- 300-350ns,Rs1_addr=1,读取一号寄存器储存的数据,由Rs1_data=32'b0可知当Reg_Write=0时无法写入数据

- 350-400ns,RegWrite=1,Wt_addr=0,Wt_data=32'h1,尝试往零号寄存器中写入数据

- 400-500ns,Rs1_addr=0,读取零号寄存器储存的数据,由Rs1_data=32'b0可知无法往零号寄存器写入数据

## 2.3 Finite State Machine

<img src="C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-18 191838.png" style="zoom:200%;" />

- 0-3分别表示“不可信”,“可疑”,“可信”,“非常可信”,测谎仪一开始是设为不可信,当in=1时,当前的状态会在每个时钟上升沿往可信的方向改变,可以看到curr_state由0增加到3后不变,当前状态为2或3时,输出为1,不然为0。300-400ns,reset=1,当前状态被重置为0。之后curr_state由0增加到3后不变,此过程与之前相同。当in=0时，当前的状态会在每个时钟上升沿往不可信的方向改变,可以看到curr_state由3下降到0后不变。

# 三、讨论、心得

## 思考题

- ALU中的思考题我并没有想出来原因,然后也搜不出来,最后就去问同学了。最终得到一个推测,也不确定是否正确。先将代码的第一个三目运算符注释掉,这时候得出的结果仍是往高位补0,然后将32‘d0变为有符号数32’sd0,这时候得到的结果就是往高位补1。

  ![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-18 231329.png)

  ![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-18 231346.png)

- 同理推断之前往高位补零的原因是A>>B得到的是无符号数,所以将A>>B变为$signed(A>>B),最终得到的结果确实就是往高位补1

![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-18 232303.png)

![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-18 232133.png)

- 最后我又将32‘sd0变回32'd0,得到的结果是往高位补0。我推断原因可能是在三目运算符中,如果操作数中既有有符号数也有无符号数的话,最终会按照无符号数进行处理

![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-18 232356.png)

![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-18 232351.png)

- FSM思考题的图如下

  <img src="C:\Users\杨吉祥\Pictures\B70222E925FD222D0DEA4B821EA46584.jpg" style="zoom:50%;" />
  
  ## 心得
  
  第一次实验要实现的东西相对来说还是比较简单的,没遇到什么问题,很快就完成了,希望之后也能这么顺利。
  
  


<div style="page-break-after:always;"></div>

![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-03-28 200238.png)

| 课程名称： | 计算机组成与设计     |
| ---------- | -------------------- |
| 姓  名：   | 杨吉祥               |
| 学  院：   | 计算机科学与技术学院 |
| 系：       | 竺可桢学院图灵班     |
| 专  业：   | 计算机科学与技术     |
| 学  号：   | 3230106222           |
| 指导教师： | 刘海风               |

<div style="page-break-after:always;"></div>

**浙江大学实验报告**

课程名称：计算机组成与设计       实验类型： 综合   

实验项目名称：使用提供的 IP core 搭建测试框架

学生姓名：  杨吉祥  专业：  计算机科学与技术  学号： 3230106222 

报告日期：10.10

## 一、操作方法与实验步骤

- CSSTE.v

  ```verilog
  `timescale 1ns / 1ps
  module CSSTE(
      input         clk_100mhz,
      input         RSTN,
      input    BTN_y,
      input  [15:0] SW,
      output   Blue,
      output   Green,
      output   Red,
      output        HSYNC,
      output        VSYNC,
      output [15:0] LED_out,
      output [7:0] AN,
      output [7:0] segment
  );
  wire U1_MemRW;
  wire [31:0]U1_Addr_out;
  wire [31:0]U1_Data_out;
  wire [31:0]U1_PC_out;
  
  wire [31:0]U2_spo;
  
  wire [31:0]U3_douta;
  
  wire [31:0]U4_Cpu_data4bus;
  wire [31:0]U4_ram_data_in;
  wire [9:0]U4_ram_addr;
  wire U4_data_ram_we;
  wire U4_GPIOf0000000_we;
  wire U4_GPIOe0000000_we;
  wire U4_counter_we;
  wire [31:0]U4_Peripheral_in;
  
  wire [7:0]U5_point_out;
  wire [7:0]U5_LE_out;
  wire [31:0]U5_Disp_num;
  
  wire [7:0]U6_AN;
  wire [7:0]U6_segment;
  
  wire [1:0]U7_counter_set;
  wire [15:0]U7_LED_out;
  
  wire [31:0]U8_clkdiv;
  wire U8_Clk_CPU;
  
  wire [3:0]U9_BTN_OK;
  wire [15:0]U9_SW_OK;
  wire U9_rst;
  
  wire U10_counter0_OUT;
  wire U10_counter1_OUT;
  wire U10_counter2_OUT;
  wire [31:0]U10_counter_out;
  
  SCPU U1(
      .clk(U8_Clk_CPU),
      .rst(U9_rst),
      .Data_in(U4_Cpu_data4bus),
      .inst_in(U2_spo),
      .MemRW(U1_MemRW),
      .Addr_out(U1_Addr_out),
      .Data_out(U1_Data_out),
      .PC_out(U1_PC_out)
  );
  
  ROM_D U2(
      .a(U1_PC_out[11:2]),
      .spo(U2_spo)
  );
  
  RAM_B U3(
      .clka(~clk_100mhz),
      .wea(U4_data_ram_we),
      .addra(U4_ram_addr),
      .dina(U4_ram_data_in),
      .douta(U3_douta)
  );
  
  MIO_BUS U4(
      .clk(clk_100mhz),
      .rst(U9_rst),
      .BTN(U9_BTN_OK),
      .SW(U9_SW_OK),
      .mem_w(U1_MemRW),
      .Cpu_data2bus(U1_Data_out),
      .addr_bus(U1_Addr_out),
      .ram_data_out(U3_douta),
      .led_out(U7_LED_out),
      .counter_out(U10_counter_out),
      .counter0_out(U10_counter0_OUT),
      .counter1_out(U10_counter1_OUT),
      .counter2_out(U10_counter2_OUT),
      .Cpu_data4bus(U4_Cpu_data4bus),
      .ram_data_in(U4_ram_data_in),
      .ram_addr(U4_ram_addr),
      .data_ram_we(U4_data_ram_we),
      .GPIOf0000000_we(U4_GPIOf0000000_we),
      .GPIOe0000000_we(U4_GPIOe0000000_we),
      .counter_we(U4_counter_we),
      .Peripheral_in(U4_Peripheral_in)
  );
  
  Multi_8CH32 U5(
      .clk(~U8_Clk_CPU),
      .rst(U9_rst),
      .EN(U4_GPIOe0000000_we),
      .Test(U9_SW_OK[7:5]),
      .point_in({U8_clkdiv.U8_clkdiv}),
      .LES(64'b0),
      .Data0(U4_Peripheral_in),
      .data1({2'b0,U1_PC_out[31:2]}),
      .data2(U2_spo),
      .data3(U10_counter_out),
      .data4(U1_Addr_out),
      .data5(U1_Data_out),
      .data6(U4_Cpu_data4bus),
      .data7(U1_PC_out),
      .point_out(U5_point_out),
      .LE_out(U5_LE_out),
      .Disp_num(U5_Disp_num)
  );
  
  Seg7_Dev U6(
      .disp_num(U5_Disp_num),
      .point(U5_point_out),
      .les(U5_LE_out),
      .scan({U8_clkdiv[18],U8_clkdiv[17],U8_clkdiv[16]}),
      .AN(AN),
      .segment(segment)
  );
  
  SPIO U7(
      .clk(~U8_Clk_CPU),
      .rst(U9_rst),
      .Start(U8_clkdiv[20]),
      .EN(U4_GPIOf0000000_we),
      .P_Data(U4_Peripheral_in),
      .counter_set(U7_counter_set),
      .LED_out(U7_LED_out)
  );
  
  clk_div U8(
      .clk(clk_100mhz),
      .rst(U9_rst),
      .SW2(U9_SW_OK[2]),
      .SW8(U9_SW_OK[8]),
      .STEP(U9_SW_OK[10]),
      .clkdiv(U8_clkdiv),
      .Clk_CPU(U8_Clk_CPU)
  );
  
  SAnti_jitter U9(
      .clk(clk_100mhz),
      .RSTN(RSTN),
      .Key_y(BTN_y),
      .SW(SW),
      .BTN_OK(U9_BTN_OK),
      .SW_OK(U9_SW_OK),
      .rst(U9_rst)
  );
  
  Counter_x U10(
      .clk(~U8_Clk_CPU),
      .rst(U9_rst),
      .clk0(U8_clkdiv[6]),
      .clk1(U8_clkdiv[9]),
      .clk2(U8_clkdiv[11]),
      .counter_we(U4_counter_we),
      .counter_val(U4_Peripheral_in),
      .counter_ch(U7_counter_set),
      .counter0_OUT(U10_counter0_OUT),
      .counter1_OUT(U10_counter1_OUT),
      .counter2_OUT(U10_counter2_OUT),
      .counter_out(U10_counter_out)
  );
  
  VGA U11(
      .clk_25m(U8_clkdiv[1]),
      .clk_100m(clk_100mhz),
      .rst(U9_rst),
      .pc(U1_PC_out),
      .inst(U2_spo),
      .alu_res(U1_Addr_out),
      .mem_wen(U1_MemRW),
      .dmem_o_data(U3_douta),
      .dmem_i_data(U4_ram_data_in),
      .dmem_addr(U1_Addr_out),
      .hs(HSYNC),
      .vs(VSYNC),
      .vga_r(Red),
      .vga_g(Green),
      .vga_b(Blue)
  );
  
  endmodule
  ```

- 根据vga_debugger修改后的VGA.v如下

  ```verilog
  `timescale 1ns / 1ps
  module VGA(
      input wire clk_25m,
      input wire clk_100m,
      input wire rst,
      input wire [31:0] pc,
      input wire [31:0] inst,
      input wire [4:0] rs1,
      input wire [31:0] rs1_val,
      input wire [4:0] rs2,
      input wire [31:0] rs2_val,
      input wire [4:0] rd,
      input wire [31:0] reg_i_data,
      input wire reg_wen,
      input wire is_imm,
      input wire is_auipc,
      input wire is_lui,
      input wire [31:0] imm,
      input wire [31:0] a_val,
      input wire [31:0] b_val,
      input wire [3:0] alu_ctrl,
      input wire [2:0] cmp_ctrl,
      input wire [31:0] alu_res,
      input wire cmp_res,
      input wire is_branch,
      input wire is_jal,
      input wire is_jalr,
      input wire do_branch,
      input wire [31:0] pc_branch,
      input wire mem_wen,
      input wire mem_ren,
      input wire [31:0] dmem_o_data,
      input wire [31:0] dmem_i_data,
      input wire [31:0] dmem_addr,
      input wire csr_wen,
      input wire [11:0] csr_ind,
      input wire [1:0] csr_ctrl,
      input wire [31:0] csr_r_data,
      input wire [31:0] x0,
      input wire [31:0] ra,
      input wire [31:0] sp,
      input wire [31:0] gp,
      input wire [31:0] tp,
      input wire [31:0] t0,
      input wire [31:0] t1,
      input wire [31:0] t2,
      input wire [31:0] s0,
      input wire [31:0] s1,
      input wire [31:0] a0,
      input wire [31:0] a1,
      input wire [31:0] a2,
      input wire [31:0] a3,
      input wire [31:0] a4,
      input wire [31:0] a5,
      input wire [31:0] a6,
      input wire [31:0] a7,
      input wire [31:0] s2,
      input wire [31:0] s3,
      input wire [31:0] s4,
      input wire [31:0] s5,
      input wire [31:0] s6,
      input wire [31:0] s7,
      input wire [31:0] s8,
      input wire [31:0] s9,
      input wire [31:0] s10,
      input wire [31:0] s11,
      input wire [31:0] t3,
      input wire [31:0] t4,
      input wire [31:0] t5,
      input wire [31:0] t6,
      input wire [31:0] mstatus_o,
      input wire [31:0] mcause_o,
      input wire [31:0] mepc_o,
      input wire [31:0] mtval_o,
      input wire [31:0] mtvec_o,
      input wire [31:0] mie_o,
      input wire [31:0] mip_o,
  
      output wire hs,
      output wire vs,
      output wire [3:0] vga_r,
      output wire [3:0] vga_g,
      output wire [3:0] vga_b
      );
      wire [9:0] vga_x;
      wire [8:0] vga_y;
      wire video_on;
      VgaController vga_controller(
             .clk          (clk_25m      ),
             .rst          (rst          ),
             .vga_x        (vga_x        ),
             .vga_y        (vga_y        ),
             .hs           (hs           ),
             .vs           (vs           ),
             .video_on     (video_on     )
        );
   wire display_wen;
   wire [11:0] display_w_addr;
   wire [7:0] display_w_data;
   VgaDisplay vga_display(
            .clk          (clk_100m      ),
            .video_on     (video_on      ),
            .vga_x        (vga_x         ),
            .vga_y        (vga_y         ),
            .vga_r        (vga_r         ),
            .vga_g        (vga_g         ),
            .vga_b        (vga_b         ),
            .wen          (display_wen   ),
            .w_addr       (display_w_addr),
            .w_data       (display_w_data)
        );
   VgaDebugger vga_debugger(
           .clk           (clk_100m      ),
           .display_wen   (display_wen   ),
           .display_w_addr(display_w_addr),
           .display_w_data(display_w_data),
           .pc            (pc             ),
           .inst          (inst           ),
           .rs1           (rs1            ),
           .rs1_val       (rs1_val        ),       
           .rs2           (rs2            ),
           .rs2_val       (rs2_val        ),    
           .rd            (rd             ),
           .reg_i_data    (reg_i_data     ),
           .reg_wen       (reg_wen        ),
           .is_imm        (is_imm         ),
           .is_auipc      (is_auipc       ),
           .is_lui        (is_lui         ),
           .imm           (imm            ),
           .a_val         (a_val          ),
           .b_val         (b_val          ),
           .alu_ctrl      (alu_ctrl       ),
           .cmp_ctrl      (cmp_ctrl       ),
           .alu_res       (alu_res        ),
           .cmp_res       (cmp_res        ),
           .is_branch     (is_branch      ),
           .is_jal        (is_jal         ),
           .is_jalr       (is_jalr        ),
           .do_branch     (do_branch      ),
           .pc_branch     (pc_branch      ),
           .mem_wen       (mem_wen        ),
           .mem_ren       (mem_ren        ),
           .dmem_o_data   (dmem_o_data    ),
           .dmem_i_data   (dmem_i_data    ),
           .dmem_addr     (dmem_addr      ),
           .csr_wen       (csr_wen        ),
           .csr_ind       (csr_ind        ),
           .csr_ctrl      (csr_ctrl       ),
           .csr_r_data    (csr_r_data     ),
           .x0            (x0             ),
           .ra            (ra             ),
           .sp            (sp             ),
           .gp            (gp             ),
           .tp            (tp             ),
           .t0            (t0             ),
           .t1            (t1             ),
           .t2            (t2             ),
           .s0            (s0             ),
           .s1            (s1             ),
           .a0            (a0             ),
           .a1            (a1            ),
           .a2            (a2             ),
           .a3            (a3             ),
           .a4            (a4             ),
           .a5            (a5             ),
           .a6            (a6             ),
           .a7            (a7             ),
           .s2            (s2             ),
           .s3            (s3             ),
           .s4            (s4             ),
           .s5            (s5             ),
           .s6            (s6             ),
           .s7            (s7             ),
           .s8            (s8             ),
           .s9            (s9             ),
           .s10           (s10            ),
           .s11           (s11            ),
           .t3            (t3             ),
           .t4            (t4             ),
           .t5            (t5             ),
           .t6            (t6             ),
           .mstatus_o     (mstatus_o      ),
           .mcause_o      (mcause_o       ),
           .mepc_o        (mepc_o         ),
           .mtval_o       (mtval_o        ),
           .mtvec_o       (mtvec_o        ),
           .mie_o         (mie_o          ),
           .mip_o         (mip_o          )
       );
  endmodule
  ```

- I_mem.pdf实现了求前31个斐波那契数列的功能。该程序一开始执行`x1=x0+1`,给x1初始值置为1,接着执行`slt x2 x0 x1`,因为x0小于x1,所以x2被置为1。因为x1与x2相同,所以`x3=x2+x2=x2+x1`,之后执行的都是$$xi=xi-1+xi-2$$,这也就是斐波那契数列的定义,求完x31之后执行`beq x0 x0 -124`,因为x0=x0,所以跳转到PC-124,也就是PC=0,重新进行上述过程。之后上板发现确实是一个重复求斐波那契数列的过程。

  ![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-20 102418.png)

## 二、实验结果与分析

- `SW[8]SW[2]=00`: CPU 全速时钟 100MHZ。
- `SW[8]SW[2]=01`: CPU 自动单步时钟。
- `SW[8]SW[2]=1X`: CPU 手动单步时钟。

- `SW[7:5]=001`: 显示 CPU 指令字地址 `PC_out[31:2]`，即我们下一条要执行的指令的地址。

- `SW[7:5]=010`: 显示 ROM 指令输出 `Inst_in`，即我们正在执行的指令。

- `SW[7:5]=100`: 显示 CPU 数据存储地址 `addr_bus`，在 Lab4 中你将会了解到 `addr_bus` 实际上就是 ALU 的运算结果。

- `SW[7:5]=101`: 显示 CPU 数据输出 `Cpu_data2bus`，在 Lab4 中你将会了解到 `Cpu_data2bus` 实际上就是寄存器 B 的值（假设每个周期从寄存器堆中读取 A、B 寄存器）。

- `SW[7:5]=110`: 显示 CPU 数据输入 `Cpu_data4bus`，在 Lab4 中你将会了解到 `Cpu_data4bus` 实际上就是 RAM 的输出，即从内存中读出来的值。

- `SW[7:5]=111`: 显示 CPU 指令字地址 `PC_out`。

  - 执行第一条指令,让SW[7:5]=010,得到当前指令的机器码为0x00100093。让SW[7:5]=100,得到ALU的运算结果为1,即x1=1。让SW[7:5]=111,得到指令字地址为0x0

    | ![img](file://C:/Users/%E6%9D%A8%E5%90%89%E7%A5%A5/Pictures/F92197270559348C52500DDA85B64AFB.jpg?lastModify=1729422424) | ![img](file://C:/Users/%E6%9D%A8%E5%90%89%E7%A5%A5/Pictures/2B0633F6320DFE71F273C3AB44B7FE06.jpg?lastModify=1729422424) | ![img](file://C:/Users/%E6%9D%A8%E5%90%89%E7%A5%A5/Pictures/1A53E53C9929FB5D93CFCB20123B087F.jpg?lastModify=1729422424) |
    | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
    |                                                              |                                                              |                                                              |

  - 执行第二条指令,让SW[7:5]=010,得到当前指令的机器码为0x00102133。让SW[7:5]=100,得到ALU的运算结果为1,即x2=1。让SW[7:5]=111,得到指令字地址为0x4

    | ![](C:\Users\杨吉祥\Pictures\9BEF6DEE5CAFC23B49C83E56A98298FF.jpg) | ![](C:\Users\杨吉祥\Pictures\1495F41E2CF16E883A34BF419359D4C1.jpg) | ![](C:\Users\杨吉祥\Pictures\9A8AE4FE9DFD55BC5BC96CFD5EC49921.jpg) |
    | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |

  - 中间过程让SW[7:5]=100,截取了几个数据,如x7=0xd,x8=0x15,x9=0x22,以上结果都与附件I_mem.pdf中的数据相符(附件中的数据从x17开始出错了,一开始还以为是程序问题,后面才发现是附件数据给错了)

| ![](C:\Users\杨吉祥\Pictures\3412DE55ADF6EF07AAAC4C535A7A3EB3.jpg) | ![](C:\Users\杨吉祥\Pictures\413CDF02F3C9F69E214B1C1C3DFAC051.jpg) | ![](C:\Users\杨吉祥\Pictures\0FB3383749ADCE2AE967E54ACEAA0E83.jpg) |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |

# 三、讨论、心得

- 无思考题
- 这实验看图连线是最折磨人的,花了一个多小时才将线连好,太费眼睛了。然后就是花了好长时间才理解这个实验要干什么,学会了lab0的ip调用以及ROM核和RAM核的生成,收获满满。

<div style="page-break-after:always;"></div>

![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-03-28 200238.png)

| 课程名称： | 计算机组成与设计     |
| ---------- | -------------------- |
| 姓  名：   | 杨吉祥               |
| 学  院：   | 计算机科学与技术学院 |
| 系：       | 竺可桢学院图灵班     |
| 专  业：   | 计算机科学与技术     |
| 学  号：   | 3230106222           |
| 指导教师： | 刘海风               |

<div style="page-break-after:always;"></div>

**浙江大学实验报告**

课程名称：计算机组成与设计       实验类型： 综合   

实验项目名称：乘法器、除法器、浮点加法器

学生姓名：  杨吉祥  专业：  计算机科学与技术  学号： 3230106222 

报告日期：10.17

## 一、操作方法与实验步骤

## 1.1 乘法器

- multiplier.v

  ```verilog
  `timescale 1ns / 1ps
  module multiplier(
    input           clk,      // 时钟信号
    input           start,     // 开始运算
    input [31:0]    A,        // 两个 32-bit 输入值
    input [31:0]    B,
    output reg         finish,   // 当结束计算时置 1，你可能需要将它改为 `output reg`
    output reg[63:0]    res       // 64-bit 输出，你可能需要将它修改为 `output reg[63:0]`
  );
  reg sign;
  reg state;
  reg [5:0]count;
  wire [31:0]a;
  wire [31:0]b;
  assign a=A[31]?(~A+1):A;
  assign b=B[31]?(~B+1):B;
  initial begin
    state<=0;
    count<=0;
    sign<=0;
    res<=0;
    finish<=0;
  end
  always @(posedge clk) begin
        if(~state&&start)begin
          state<=1;
          count<=0;
          sign<=A[31]^B[31];
          res<={32'b0,b};
          finish<=0;
        end
        else if(state)begin
          if(count<32)begin
            if(res[0])begin
              res[63:32]=res[63:32]+a;
            end
            res=res>>1;
            count=count+1;
          end
          else begin
            if(sign)begin
              res=~res+1;
            end
            state<=0;
            finish<=1;
          end
      end
  end
  endmodule
  ```

- multiplier_tb.v

  ```verilog
  `timescale 1ns / 1ps
  module multiplier_tb;
  
    reg clk, start;
    reg[31:0] A;
    reg[31:0] B;
  
    wire finish;
    wire[63:0] res;
  
    multiplier m0(.clk(clk), .start(start), .A(A), .B(B), .finish(finish), .res(res));
  
    initial begin
      $dumpfile("multiplier_signed.vcd");
      $dumpvars(0, multiplier_tb);
      clk = 0;
      start = 0;
      #10;
      A = 32'd1;
      B = 32'd0;
      #10 start = 1;
      #10 start = 0;
      #200;
      A = 32'd10;
      B = 32'd30;
      #10 start = 1;
      #10 start = 0;
      #200;
      A = 32'hFFFFFFFB;//-5
      B = 32'hFFFFFFFA;//-6
      #10 start = 1;
      #10 start = 0;
      #200;
      A = 32'hFFFFFFFB;//-5
      B = 32'd9;
      #10 start = 1;
      #10 start = 0;
      #200;
      A = 32'h7FFFFFFF;//2^31-1
      B = 32'h7FFFFFFF;//2^31-1
      #10 start = 1;
      #10 start = 0;
      #200;
      A = 32'hFFFFFFFF;//-1
      B = 32'hFFFFFFFF;//-1
      #10 start = 1;
      #10 start = 0;
      $finish();
    end
  
    always begin
      #2 clk = ~clk;
    end
  
  
  
  endmodule
  ```

  ## 1.2 除法器

  - divider.v

    ```verilog
    module divider(
        input   clk,
        input   rst,
        input   start,          // 开始运算
        input[31:0] dividend,   // 被除数
        input[31:0] divisor,    // 除数
        output reg divide_zero,     // 除零异常
        output reg finish,         // 运算结束信号
        output[31:0] res,       // 商
        output[31:0] rem        // 余数
    );
    reg[63:0]remainder;
    reg[5:0]count;
    reg state;
    assign res=remainder[31:0];
    assign rem={1'b0,remainder[63:33]};
    always @(posedge clk or posedge rst)begin
            if(rst)begin
                divide_zero<=0;
                finish<=0;
                state<=0;
                count<=0;
                remainder<=0;
            end
            else if(~state&&start)begin
                state<=1;
                finish<=0;
                count<=0;
                divide_zero<=0;
                remainder<={31'b0,dividend,1'b0};
            end
            else if(state)begin
                if(divisor==0)begin
                    divide_zero<=1;
                    finish<=1;
                    state<=0;
                    remainder<=64'bx;
                end
                else if(count<32)begin
                    if(remainder[63:32]>=divisor)begin
                        remainder[63:32]=remainder[63:32]-divisor;
                        remainder={remainder[62:0],1'b1};
                    end
                    else begin
                        remainder<={remainder[62:0],1'b0};
                    end
                    count<=count+1;
                end
                else begin
                    finish<=1;
                    state<=0;
                end
            end
    end
    endmodule
    ```

  - divider_tb.v

    ```verilog
    `timescale 1ns / 1ps
    module divider_tb();
          reg clk;
          reg rst;
          reg [31:0] dividend;
          reg [31:0] divisor;
          reg start;
          
          wire divide_zero;
          wire [31:0] res;
          wire [31:0] rem;
          wire finish;
          divider   u_div(
             .clk(clk),
             .rst(rst),
             .dividend(dividend),
             .divisor(divisor),
             .start(start),
             .divide_zero(divide_zero),
             .res(res),
             .rem(rem),
             .finish(finish)
          );
          always #5 clk = ~clk;
          
          initial begin
           clk =0;
           rst = 1;
           start = 0;
           #10
           rst = 0;
           start = 1;
               dividend = 32'd8;
               divisor  = 32'd4;
               #10 start=0;
               #335
               start=1;
               dividend = 32'd100;  
               divisor  = 32'd10;
               #10 start=0;   
               #335     
               start=1;
               dividend = 32'd9;
               divisor  = 32'd4; 
               #10 start=0;
               #340 
               start=1;           
               dividend = 32'd100; 
               divisor  = 32'd99;
                #10 start=0;
               #340
                start=1;
               dividend=32'd20;
               divisor=32'b0;
                #10 start=0;
               #340
                start=1;
               dividend=32'b0;
               divisor=32'd13;
                #10 start=0;
               #340
                start=1;
               dividend=32'hFFFFFFFF;
               divisor=32'd1;
                #10 start=0;
                #340
                start=1;
               dividend=32'hFFFFFFFF;
               divisor=32'h80000000;
                #10 start=0;
               #350 $stop();   
          
          end
    endmodule
    ```

    ## 1.3 浮点加法器

    - floatadder.v

      ```verilog
      `timescale 1ns / 1ps
      module floatadder(
        input clk,      // 时钟信号
        input rst,      // 复位信号
        input start,    // 开始运算
        input [31:0]A,  // 两个 32-bit 输入值
        input [31:0]B,
        output reg finish,   //结束信号
        output reg[31:0]res  
      );
      reg run;
      reg sign;
      reg [2:0]state;
      reg [7:0]A_exp;
      reg [7:0]B_exp;
      reg [7:0]C_exp;
      reg [24:0]A_frac;
      reg [24:0]B_frac;
      reg [24:0]C_frac;
      localparam 
      Q1=3'b000,//判断两个操作数中是否有不规则浮点数
      Q2=3'b001,//浮点数对阶
      Q3=3'b010,//浮点数对阶后的加法
      Q4=3'b011,//结果规则化
      Q5=3'b100;//结束
      always @(posedge clk or posedge rst)begin
          if(rst)begin
              finish<=0;
              sign<=0;
              C_exp<=0;
              C_frac<=0;
              run<=0;
          end
          else if(start)begin
              state<=Q1;
              finish<=0;
              sign<=0;
              A_exp<=A[30:23];
              B_exp<=B[30:23];
              A_frac<={2'b1,A[22:0]};
              B_frac<={2'b1,B[22:0]};
              C_exp<=0;
              C_frac<=0;
              run<=1;
          end
          else if(run)begin
              case(state) 
                  Q1:begin
                      if(A_exp==8'hFF||A_exp==8'h00)begin
                          sign<=A[31];
                          C_exp<=A_exp;
                          C_frac<=A_frac;
                          state<=Q5;
                      end
                      else if(B_exp==8'hFF||B_exp==8'h00)begin
                          sign<=B[31];
                          C_exp<=B_exp;
                          C_frac<=B_frac;
                          state<=Q5;
                      end
                      else begin
                          state<=Q2;
                      end
                  end
                  Q2:begin
                      if(A_exp>B_exp)begin
                          C_exp<=A_exp;
                          B_frac<=B_frac>>(A_exp-B_exp);
                          state<=Q3;
                      end
                      else begin
                          C_exp<=B_exp;
                          A_frac<=A_frac>>(B_exp-A_exp);
                          state<=Q3;
                      end
                  end
                  Q3:begin
                      if(A[31]==B[31])begin
                          sign<=A[31];
                          C_frac<=A_frac+B_frac;
                          state<=Q4;
                      end
                      else if(A_frac>B_frac)begin
                          sign<=A[31];
                          C_frac<=A_frac-B_frac;
                          state<=Q4;
                      end
                      else if(A_frac<B_frac)begin
                          sign<=B[31];
                          C_frac<=B_frac-A_frac;
                          state<=Q4;
                      end
                      else begin
                          sign<=0;
                          C_exp<=0;
                          C_frac<=0;
                          state<=Q5;
                      end
                  end
                  Q4:begin
                      if(C_frac[24])begin
                          C_exp<=C_exp+1;
                          C_frac<=C_frac>>1;
                          state<=Q5;
                      end
                      else if(C_frac[23]==0)begin
                          C_exp=C_exp-1;
                          C_frac=C_frac<<1;
                          state=Q4;
                      end
                      else begin
                          state<=Q5;
                      end
                  end
                  Q5:begin
                      res<={sign,C_exp,C_frac[22:0]};
                      finish<=1;
                      run<=0;
                  end
              endcase
          end
      end
      endmodule
      ```

    - floatadder_t.v

      ```verilog
      `timescale 1ns / 1ps
      module floatadder_t();
        reg clk;
        reg rst;   
        reg start;
        reg [31:0]A;
        reg [31:0]B;
        wire finish;  
        wire [31:0]res;
       floatadder uut(
          .clk(clk),
          .rst(rst),
          .start(start),
          .A(A),
          .B(B),
          .finish(finish),
          .res(res)
       );
      
       initial begin
       clk=0;
       rst=1;
       start=0;
       #10 rst=0;
       #10
       A=32'h40600000; //3.5
       B=32'h3FA00000; //1.25
       start = 1;#10;//32'h40980000//4.75
       start = 0;#80;
       A=32'hC0600000; //-3.5
       B=32'h3FA00000; //1.25
       start = 1;#10;//32'hC0100000//-2.25
       start = 0;#80;
       A=32'hC0200000; //-2.5
       B=32'hBFA00000; //-1.25
       start = 1;#10;//32'hC0700000//-3.75
       start = 0;#80;
       A=32'h7F800000; //指数全为1
       B=32'h3FA00000; //1.25
       start = 1;#10;
       start = 0;#80;
       A=32'h0; //指数全为0
       B=32'h3FA00000; //1.25
       start = 1;#10;
       start = 0;#80;
       A=32'hBFA00000;//-1.25
       B=32'h3FA00000; //1.25
       start = 1;#10;
       start = 0;#80;
       end  
      always begin
      #5 clk=~clk;
      end
      endmodule
      ```

      # 二、实验结果与分析

      ## 2.1 乘法器

      乘法器的实现过程：在每个时钟上升沿都会对当前状态进行判断,start作为开始信号,当start为1时,会将state变为1,表示正在计算。若state为1时,start再次为1,这时候不会进行新的计算,而是会继续进行当前的计算,计算完成后将finish置为1,state置为0,表示完成计算。将32位乘数放在64位res的低32位,高32位置0,如果res[0]=1,就将res的高32位加上被乘数,如果res[0]=0,res的高32位就不变。然后将res右移一位,重复这个过程32次,也就是在32个时钟周期后就完成了计算。

      - 乘数为0时

      ![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-19 103153.png)

      - 正数乘正数

      ![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-19 103223.png)

      - 负数乘负数

      ![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-19 103239.png)

      - 负数乘正数

      ![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-19 103254.png)

      - 边界测试大的正数乘大的正数

      ![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-19 103306.png)

      - 边界测试小的负数乘小的负数

      ![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-19 103327.png)

      根据res的结果可以看出波形符合预期

      ## 2.2 除法器

      除法器的实现过程：与乘法器类似,在每个时钟上升沿都会对当前状态进行判断,start作为开始信号,当start为1时,会将state变为1,表示正在计算。若state为1时,start再次为1,这时候不会进行新的计算,而是会继续进行当前的计算,计算完成后将finish置为1,state置为0,表示完成计算。将32位被除数放在64位remainder的低32位后左移一位,如果remainder的高32位大于等于除数,就将remainder左移一位,最低位补1,如果remainder的高32位小于除数,就将remainder左移一位,最低位补0。重复这个过程32次后,remainder的低32位就是商res,高32位右移一位就是余数rem。如果除数为0,divide_zero就会为1,表示除零异常,此时remainder置为64‘bx。

      - 刚好整除,余数为0

      ![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-19 124919.png)

      - 余数不为0

      ![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-19 124930.png)

      - 除数为0

      ![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-19 124947.png)

      - 被除数为0

      ![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-19 125003.png)

      - 商很大的情况

      ![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-19 125015.png)

      - 余数很大的情况

      ![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-19 125022.png)

      根据res和rem的结果可以看出波形符合预期

      ## 2.3 浮点加法器

      浮点加法器的实现过程：在每个时钟上升沿都会对当前状态进行判断,start作为开始信号,当start为1时,会将run变为1,表示正在计算。若run为1时,start再次为1,这时候不会进行新的计算,而是会继续进行当前的计算,计算完成后将finish置为1,run置为0,表示完成计算。因为浮点数的加法较为麻烦,所以新增了一个变量state将浮点数加法分为多个步骤。这里我们将浮点数加法分为5个步骤,分别为Q1-Q5。Q1判断两个操作数中是否有不规则化数,如果有就结束运算,结果就是不规则化数。Q2进行浮点数的对阶,就是将两个浮点数变成指数相同。Q3进行对阶后的加法。Q4将得到的结果规则化。Q5表示计算结束。

      * 正数加正数（40600000=0 10000000 11000000000000000000000,表示符号位为0,也就是正数,指数位为128,也就是指数为128-127=1,尾数为后23位,也就是0.11+1=1.11,这就表示$$1.11*2^1=3.5$$,类似的3fa00000=1.25,二者相加得到4.75,也就是40980000,之后的过程与此类似,不再赘述）

      ![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-19 132420.png)

      - 负数加正数（-3.5+1.25=-2.25）

        ![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-19 132433.png)

      - 负数加负数（-2.5-1.25=-3.75）

        ![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-19 133717.png)

      - 不规则化数,指数位全为1

        ![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-19 132455.png)

      - 不规则化数,指数位全为0

        ![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-19 132507.png)

      - 和为0的情况

  ![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-19 132517.png)

  根据res的结果可以看出波形符合预期

  # 三、讨论

  ## 思考题

  - (1)$$(x+y)+z=1.0$$

    (2)$$x+(y+z)=0$$

    (1)先进行x+y,因为x和y的指数相同,不需要进行移位,所以x+y=0,再加上z就得到1.0。

    (2)先进行y+z,因为y和z的指数不同,所以要先移位进行对齐,$$1.5e38$$ 大概相当于2的126次方,而1.0就是2的0次方,也就是在对齐指数的时候,z需要右移126次,从我们实现浮点加法器的过程可知,z的尾数加上1一共就53位,右移126次,会使得z变为0,所以y+z=y,而x+y=0,所以最终结果为0

  - (1)-0.000954

    (2)0.000000

    .![](C:\Users\杨吉祥\Pictures\Screenshots\屏幕截图 2024-10-19 141915.png)

​		C语言中浮点数的存储遵循IEEE 754标准,导致二者的计算结果不同

​		(1)中的0.1无法用浮点数准确表示出来,在内存中只能以一个近似的数进行存储,这就导致了最终的计算结果出现些许偏差

​		(2)中的0.125为2的-3次方,可以准确地表示出来,所以最终的计算结果不会出现偏差

## 心得

乘法器和除法器的实现相对比较容易,按照老师PPT上的内容按步实现就可以了。浮点加法器的实现相对麻烦,一开始感觉很乱,不知从何开始,后面将浮点加法分成多个过程后再去实现就相对清楚了,最后也是顺利实现了。希望之后的两个大实验也能顺利完成。

<div style="page-break-after:always;"></div>