# Transport-Layer-Security-1
Transport Layer Security (TLS) Lab
Task1:TLS Client
Task 1.a: TLS handshake
Use the following code, in order to print out the various algorithms used by TLS, you can add the pprint.pprint(ssock.cipher()) statement on the basis of the experimental manual code:
#!/usr/bin/python3
import socket, ssl, sys, pprint
hostname = sys.argv[1]
port = 443
cadir = '/etc/ssl/certs'
# Set up the TLS context 
context = ssl.SSLContext(ssl.PROTOCOL_TLSv1_2)      # For Ubuntu 16.04 VM

context.load_verify_locations(capath=cadir)
context.verify_mode = ssl.CERT_REQUIRED
context.check_hostname = True

# Create TCP connection 
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.connect((hostname, port))
input("After making TCP connection. Press any key to continue ...")

# Add the TLS
ssock = context.wrap_socket(sock, server_hostname=hostname, 
								do_handshake_on_connect=False)
ssock.do_handshake()   # Start the handshake
print("=== Cipher used: {}".format(ssock.cipher()))
print("=== Server hostname: {}".format(ssock.server_hostname))
print("=== Server certificate:")
pprint.pprint(ssock.getpeercert())
pprint.pprint(context.get_ca_certs())
input("After TLS handshake. Press any key to continue ...")

# Close the TLS Connection
ssock.shutdown(socket.SHUT_RDWR)
ssock.close()
TLS requires four steps to provide secure communication for sockets.
•	The first step is to create a TLS context object. The object saves our preference settings for certificate authentication and encryption algorithm selection. The context part of the code.
•	The second step is to establish a TCP handshake protocol, make a TCP connection, and the code sock part.
•	The third step is to call the context object created in the first step and use the wrap_socket() method on it, which means that the OpenSLL library is responsible for controlling our TCP connection. Then exchange the necessary handshake information with the communicating party and establish an encrypted link. The ssock =context.wrap_socket... part of the code
•	The last step is to use the ssl_sock object returned by the wrap_socket() call for all subsequent communications. Subsequent communication needs to use a format similar to ssock.method name.
