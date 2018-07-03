Copy greeter_client.cc to github:grpc/grpc/examples/cpp/helloworld to replace
the existing file. This example creates multiple channels in multiple threads
and does unary RPC. When round robin LB policy is used, the core gets stuck
while sending RPCs
