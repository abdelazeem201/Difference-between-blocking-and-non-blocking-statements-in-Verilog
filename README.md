# Difference-between-blocking-and-non-blocking-statements-in-Verilog

Blocking and non-blocking are part of the procedural assignments in Verilog. Following are the differences between them:-

- A blocking statement will not block the execution of the statement that is in a parallel block, means it will execute sequentially while Nonblocking assignment allows scheduling of assignment that are executed in a sequential block.

- A blocking statement is a one-step process i.e evaluate the RHS of the expression and update the LHS without any delay while Nonblocking is a two-step process i.e. a) Evaluate the RHS expression at the beginning of the time step and b) Update the LHS at the end of the time step.
Let's take an example with some delay:-

Let's take an example with some delay:-

*Blocking:-*

````````````````
always @(posedge clk) 
begin 
 x = 1; // Evaluate and assign x  
 #10; 
 y = x + 1; //wait for 10 time units   then evaluate and assign 
end 
````````````````

*Nonblocking:-*

```````````````
always @(posedge clk) 
 begin 
  x <= 1; //Evaluate x but update it only at the end of time step 
  #10; 
  y <= x + 1; //wait for 10 time units then evaluate and then update/assign it at the end of time step. 
 end 
```````````````

- Blocking statements are executed in the Active region of Verilog Stratified Event Queue while Evaluation of RHS of Nonblocking statement occurred in Active Region and update of LHS side happens in NBA region.

The stratified Event Queue of Verilog will look like:-

![image](https://user-images.githubusercontent.com/58098260/163681763-4d5ee20f-be1a-4d15-917f-a9a8185c8d27.png)

- Blocking assignment always suffers from the problem of Race condition when the assignment happens to it from two processes concurrently. While in the Nonblocking statement, there is no such problem since the updated value is assigned after the time step.

Let's take an example to illustrate the problem:-

1- *Blocking:-*
`````````````````````
always @(posedge clk) 
 X = y; 
always @(posedge clk)  
 y = x; 
 ``````````````````````
Any one of the above always blocks can be executed in any order and instead of swapping the values both the register will get updated with the same value which is a race condition.

2- *Nonblocking:-*
```````````````````````
always @(posedge clk) 
 x <= y; 
always @(posedge clk) 
 y <= x; 
 ```````````````````````
 
So in the case of Nonblocking at the pos edge of the clock, the values of all RHS are evaluated and stored in a register, and then at the end of the time step the values stored in the register are assigned to LHS, and because of that, the values of x and y are swapped correctly.

While Modelling combinational logic, blocking statements are preferred while for sequential logic, nonblocking statements are always used.

To explain let's take an example of simple SISO.
![image](https://user-images.githubusercontent.com/58098260/163681847-949f879d-65fd-49dd-b557-665e46a203cf.png)

- *Using blocking:-*
`````````````````````
always @(posedge clk) 
 begin  
  q1 = d; 
  q2 = q1; 
  q3 = q2;  
 end 
`````````````````````` 
Now when you synthesized this code it wouldn't result in a 4 bit SISO as above, instead, it will result in a single Flip Flop where q4 = d.
![image](https://user-images.githubusercontent.com/58098260/163681872-822e2ff0-d5c0-4971-bfb2-eac590006223.png)

- *Using Nonblocking:-*

```````````````````````
always @(posedge clk) 
 begin 
  q1 <= d; 
  q2 <= q1; 
  q3 <= q2;  
 end 
```````````````````````
![image](https://user-images.githubusercontent.com/58098260/163681887-cde40e41-6638-4f34-a651-889fc6cfa1a4.png)

And when you synthesize the above code the actual SISO of each stage output you will get as above.

- Blocking statements can be used in initial, always blocks, and assign statements while Nonblocking can only be used in initial and always block but assign statements can not be used.

# BIBLIOGRAPHIC NOTES

1- [Blocking and Nonblocking Assignment](https://www.youtube.com/watch?v=MsOTLTTE8sQ&list=PL-iIOnHwN7NXw01eBDR7wI8KzGK4mu8Sr&index=27&t=4s)
2- Blocking and Non-blocking Assignments in Explicit and Implicit Style Verilog Synthesis by Mark ArnoldJerry,J CupalJames, andd D Shuler


