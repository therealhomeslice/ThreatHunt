```python
import datetime
import ipaddress
from ping3 import ping

subnet = '192.168.102.0/29'
output_file = 'ping_results_single.txt'

def ping_sweep(subnet):
    network = ipaddress.ip_network(subnet, strict=False)

    with open(output_file, 'a') as f:
        for ip in network.hosts():
            ip_str = str(ip)
            timestamp = datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S')
            message = ""
            try:
                response_time = ping(ip_str, timeout=1)
                if response_time is not None:
                    message = f"{timestamp} - {ip_str} is reachable"
                else:
                    message = f"{timestamp} - {ip_str} is unreachable"
            except Exception:
                message = f"{timestamp} - Error pinging {ip_str}"
            print(message)
            f.write(message + '\n')

ping_sweep(subnet)
```

Multiple Subnets

```python
import datetime
import ipaddress
from ping3 import ping
import concurrent.futures
import threading

subnets = ['192.168.102.0/29', '192.168.107.0/29', '192.168.108.0/29']  # Add your subnets here
output_file = 'ping_results.txt'
output_lock = threading.Lock()

def ping_sweep(subnet):
    network = ipaddress.ip_network(subnet, strict=False)
    results = []

    for ip in network.hosts():
        ip_str = str(ip)
        timestamp = datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S')
        try:
            response_time = ping(ip_str, timeout=1)
            if response_time is not None:
                message = f"{timestamp} - {ip_str} is reachable"
            else:
                message = f"{timestamp} - {ip_str} is unreachable"
        except Exception:
            message = f"{timestamp} - Error pinging {ip_str}"

        results.append(message)

    with output_lock:
        print(f"Ping results for subnet {subnet}:")
        with open(output_file, 'a') as f:
            f.write(f"Ping results for subnet {subnet}:\n")
            for msg in results:
                print(msg)
                f.write(msg + '\n')
            f.write('\n')
        print()

with concurrent.futures.ThreadPoolExecutor() as executor:
    executor.map(ping_sweep, subnets)
```