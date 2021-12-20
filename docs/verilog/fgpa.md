## fgpa

## always

持续运行

并发

敏感信号列表

## rst

```verilog
input rst
...
wire rst_n = ~rst
always @ (posedge clk or negedge rst_n) begin
	if (~rst_n) begin
          ...
	end
end
```

```verilog
input rst
...
always @ (posedge clk or posedge rst) begin
	if (rst) begin
          ...
	end
end
```

## 记录按键开关状态

```verilog
reg stage;

always @ (posedge clk or posedge rst) begin
    if (rst) begin stage <= 1'b0; end  // 复位 关闭状态
        else if (button) begin stage <= 1'b1; end  //开启
end
```

如果按下复位按钮，则 stage 变为 0，状态复位；

如果没有按下复位按钮，且按下 button 按钮，则 stage 变为 1，表示开始亮灯。

## 计数器

```verilog
localparam CNT_END = 32'd99_999_999;

reg [31:0] cnt; // 定义一个时间计数寄存器

always @ (posedge clk or posedge rst) begin
	if (rst) begin cnt <= 32'b0; end // 复位 变为 0
	    else if (cnt == CNT_END) begin cnt <= 32'b0; end // 当到达限值时,也变为 0
         else begin cnt <= cnt + 32'b1; end // 计数
end
```

输入的时钟频率为 100MHz，即 clk 信号在 1s 内变化 100兆($10^8$)次，周期为 $10^{-8}$s。

我们预定了 1s，1s 内 clk 信号变化了 $10^8$ 次。在 always 块中每一次 clk 信号变化就 cnt 增 1，直到自增到 $10^8 - 1$ (因为是从 0 开始的)就重新变回 0；另外按下复位信号后也变为 0，即重新计时。
