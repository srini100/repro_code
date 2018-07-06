Code to reproduce https://github.com/grpc/grpc/issues/15892
Copy greeter_client.cc to github:grpc/grpc/examples/cpp/helloworld to replace
the existing file. This example creates multiple channels in multiple threads
and does unary RPC. When round robin LB policy is used, the core gets stuck
while sending RPCs. Comment out [this
line](https://github.com/srini100/repro_code/blob/0d649f476e410962a819720f5e028e460db52774/rr_lb_issue/greeter_client.cc#L88) and uncomment next line to see default LB policy works fine.

On the server side ensure that there are at least two backend addresses.
Typically, you'll have a v4 and a v6 address if you have both stacks enabled.
