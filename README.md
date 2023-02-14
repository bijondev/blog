# how to monitor all network request and response in pc using python

You can use the Scapy library in Python to monitor network requests and responses on a PC. Scapy allows you to capture, analyze, and manipulate network packets in real-time. Here's a simple example to get you started:

```
# Import the Scapy library
from scapy.all import *

# Define a callback function to handle incoming packets
def handle_packet(packet):
    # Check if the packet is a TCP packet
    if TCP in packet:
        # Print the source and destination IP addresses and ports
        print("Source: {}:{}".format(packet[IP].src, packet[TCP].sport))
        print("Destination: {}:{}".format(packet[IP].dst, packet[TCP].dport))
        # Print the payload of the packet
        print("Payload: {}".format(packet[TCP].payload))

# Start capturing packets with the defined callback function
sniff(prn=handle_packet)

```

This code will capture all incoming network packets, filter for TCP packets, and print out the source and destination IP addresses and ports, as well as the payload of each packet. You can modify the code to suit your specific requirements, such as filtering for specific IP addresses or ports, or storing the captured packets in a file for later analysis.

# show request and response data

Here's a modified version of the code that separates the request and response data and prints them:

```
# Import the Scapy library
from scapy.all import *

# Define a callback function to handle incoming packets
def handle_packet(packet):
    # Check if the packet is a TCP packet
    if TCP in packet and packet[TCP].payload:
        # Print the source and destination IP addresses and ports
        src_ip = packet[IP].src
        src_port = packet[TCP].sport
        dst_ip = packet[IP].dst
        dst_port = packet[TCP].dport

        # Determine if the packet is a request or a response
        # Requests are sent from source to destination, responses are sent from destination to source
        if src_ip == '192.168.0.100' and dst_ip == 'www.google.com':
            print("Request:")
            print("Source: {}:{}".format(src_ip, src_port))
            print("Destination: {}:{}".format(dst_ip, dst_port))
            # Print the payload of the packet
            print("Payload: {}".format(packet[TCP].payload))
        elif src_ip == 'www.google.com' and dst_ip == '192.168.0.100':
            print("Response:")
            print("Source: {}:{}".format(src_ip, src_port))
            print("Destination: {}:{}".format(dst_ip, dst_port))
            # Print the payload of the packet
            print("Payload: {}".format(packet[TCP].payload))

# Start capturing packets with the defined callback function
sniff(prn=handle_packet)

```

Note that in the example above, the code checks if the source IP address is 192.168.0.100 and the destination IP address is www.google.com to determine if the packet is a request, and if the source IP address is www.google.com and the destination IP address is 192.168.0.100 to determine if the packet is a response. You should replace 192.168.0.100 and www.google.com with your own IP addresses and hostnames as appropriate.

# Here's a sample output of the code:

```
Request:
Source: 192.168.0.100:54205
Destination: www.google.com:80
Payload: b'GET / HTTP/1.1\r\nHost: www.google.com\r\nConnection: keep-alive\r\nUser-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/89.0.4389.82 Safari/537.36\r\nAccept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9\r\nReferer: https://www.google.com/\r\nAccept-Encoding: gzip, deflate\r\nAccept-Language: en-US,en;q=0.9\r\n\r\n'

Response:
Source: www.google.com:80
Destination: 192.168.0.100:54205
Payload: b'HTTP/1.1 200 OK\r\nDate: Mon, 07 Jun 2021 22:57:39 GMT\r\nExpires: -1\r\nCache-Control: private, max-age=0\r\nContent-Type: text/html; charset=ISO-8859-1\r\nP3P: CP="This is not a P3P policy! See g.co/p3phelp for more info."\r\nServer: gws\r\nX-XSS-Protection: 0\r\nX-Frame-Options: SAMEORIGIN\r\nSet-Cookie: 1P_JAR=2021-06-07-22; expires=Wed, 07-Jul-2021 22:57:39 GMT; path=/; domain=.google.com\r\nSet-Cookie: NID=215=eNQxNuvzM7bNX9KztCcQlSjKtACiLk1_dVxjBKAdG4fE35nX9gqqV7aLJJszljV7Rg-LNCGx7KjMvZcdT30TQq3Tb2yK7o0Ng-Lcn0Mb1N4A1DP4bKpK0P7IeX9qv0P7e_DlRmV7syu-pKjx7ckB1i; expires=Tue, 08-Dec-2021 22:57:39 GMT; path=/; domain=.google.com; HttpOnly\r\nAlt-Svc: h3-29=":443"; ma=2592000,h3-27=":443"; ma=2592000,h3-25=":443"; ma=2592000,h3-T050=":443"; ma=2592000,quic=":443"; ma=2592000; v="121,120,119,114,113,109,108,104,103,98,97,96,95,93,92,91

```
# Here's an example of how to show JSON request and response data:
```

import socket
import json

def get_json_payload(payload):
    try:
        payload = payload.decode()
        start = payload.index("{")
        end = payload.rindex("}") + 1
        json_payload = json.loads(payload[start:end])
        return json.dumps(json_payload, indent=4)
    except ValueError:
        return None

def parse_data(source, destination, payload):
    print("Request:")
    print("Source:", source)
    print("Destination:", destination)
    json_payload = get_json_payload(payload)
    if json_payload:
        print("Payload:")
        print(json_payload)
    else:
        print("Payload (Not a JSON):")
        print(payload)

# ... rest of the code to capture network traffic ...

if __name__ == "__main__":
    conn = socket.socket(socket.AF_INET, socket.SOCK_RAW, socket.IPPROTO_TCP)
    while True:
        data = conn.recvfrom(65535)
        packet = data[0]
        source_address = data[1][0]
        destination_address = socket.inet_ntoa(packet[16:20])
        source_port, destination_port = socket.ntohs(packet[0:2]), socket.ntohs(packet[2:4])
        source = (source_address, source_port)
        destination = (destination_address, destination_port)
        payload = packet[40:]
        parse_data(source, destination, payload)

```


In this example, the function get_json_payload attempts to extract a JSON object from the payload data. If it is successful, it returns a pretty-printed JSON string. If not, it returns None. The parse_data function then prints the source and destination information, along with the JSON payload if it was able to extract one.

# Here's an example of what the output might look like:

```
Request:
Source: ('192.168.1.100', 60002)
Destination: ('127.0.0.1', 5000)
Payload:
{
    "name": "John Doe",
    "age": 30,
    "email": "johndoe@example.com"
}
```

In this example, the source of the request is ('192.168.1.100', 60002) and the destination is ('127.0.0.1', 5000). The payload is a JSON object with three key-value pairs: "name", "age", and "email".

# Here's an example of what the response output might look like:

```
Response:
Source: ('127.0.0.1', 5000)
Destination: ('192.168.1.100', 60002)
Payload:
{
    "status": "success",
    "message": "Request processed successfully."
}

```


In this example, the source of the response is ('127.0.0.1', 5000) and the destination is ('192.168.1.100', 60002). The payload is a JSON object with two key-value pairs: "status" and "message". The "status" field is set to "success" and the "message" field is a message indicating that the request was processed successfully.

To show the source and destination as domain names instead of IP addresses, you'll need to perform a reverse DNS lookup. You can use the socket library in Python to do this:

```
import socket
import json

def get_json_payload(payload):
    try:
        payload = payload.decode()
        start = payload.index("{")
        end = payload.rindex("}") + 1
        json_payload = json.loads(payload[start:end])
        return json.dumps(json_payload, indent=4)
    except ValueError:
        return None

def parse_data(source, destination, payload):
    print("Request:")
    print("Source:", socket.gethostbyaddr(source[0])[0], source[1])
    print("Destination:", socket.gethostbyaddr(destination[0])[0], destination[1])
    json_payload = get_json_payload(payload)
    if json_payload:
        print("Payload:")
        print(json_payload)
    else:
        print("Payload (Not a JSON):")
        print(payload)

# ... rest of the code to capture network traffic ...

if __name__ == "__main__":
    conn = socket.socket(socket.AF_INET, socket.SOCK_RAW, socket.IPPROTO_TCP)
    while True:
        data = conn.recvfrom(65535)
        packet = data[0]
        source_address = data[1][0]
        destination_address = socket.inet_ntoa(packet[16:20])
        source_port, destination_port = socket.ntohs(packet[0:2]), socket.ntohs(packet[2:4])
        source = (source_address, source_port)
        destination = (destination_address, destination_port)
        payload = packet[40:]
        parse_data(source, destination, payload)

```

In this example, the parse_data function now uses socket.gethostbyaddr to perform a reverse DNS lookup on the source and destination IP addresses. The resulting domain names, along with the source and destination ports, are printed in the output.

# Here's an example of what the output might look like:

```
Request:
Source: example.com 60002
Destination: api.example.com 5000
Payload:
{
    "name": "John Doe",
    "age": 30,
    "email": "johndoe@example.com"
}

```

In this example, the source of the request is example.com:60002 and the destination is api.example.com:5000. The payload is a JSON object with three key-value pairs: "name", "age", and "email".

# Here's an example of what the response output might look like:

```
Response:
Source: api.example.com 5000
Destination: example.com 60002
Payload:
{
    "status": "success",
    "message": "Request processed successfully."
}

```

In this example, the source of the response is api.example.com:5000 and the destination is example.com:60002. The payload is a JSON object with two key-value pairs: "status" and "message". The "status" field is set to "success" and the "message" field is a message indicating that the request was processed successfully.

