Copy greeter_client.cc to github:grpc/grpc/examples/cpp/helloworld to replace
the existing file. This example creates multiple channels in multiple threads
and does unary RPC. When round robin LB policy is used, the core gets stuck
while sending RPCs. Comment out line 88 and uncomment 89 to see default LB
policy works fine.

On the server side ensure that there are at least two backend addresses.
Typically, you'll have a v4 and a v6 address if you have both stacks enabled.
