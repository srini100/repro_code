There have been many issues reported where a gRPC Python/C++ client fails to verify a certificate while gRPC GO client, Curl, browsers etc. seem to do fine. The typical error seen is "Handshake failed with fatal error SSL_ERROR_SSL: error:14089086:SSL routines:ssl3_get_client_certificate:certificate verify failed". Some issues reporting this problem:

https://github.com/grpc/grpc/issues/17685

https://github.com/grpc/grpc/issues/16305

https://github.com/grpc/grpc/issues/15261

https://github.com/grpc/grpc/issues/10701

https://github.com/grpc/grpc/issues/17203

To reproduce this, run the server and client sample code with certs signed by CA but CA cert is not available in client store. Client uses the CA signed cert (not self signed) that matches the Server presented cert. But gRPC Core that uses BoringSSL does not accept certs that cannot be chained back to root CA cert. To generate certs used in this example, use https://github.com/square/certstrap with following commands:

bin/certstrap-master-linux-amd64 init --common-name "caname"

bin/certstrap-master-linux-amd64 request-cert --common-name "somedomain"

bin/certstrap-master-linux-amd64 sign somedomain --CA caname


Delete caname.* files. You should see the handshake fail with above error.

The fix is to add X509_V_FLAG_PARTIAL_CHAIN and X509_V_FLAG_TRUSTED_FIRST flags to BoringSSL to allow trusted intermediate and leaf certs. See the fix in https://github.com/grpc/grpc/pull/17868
