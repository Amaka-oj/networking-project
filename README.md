# Networking Capstone Project

This project demonstrates the design, configuration, and testing of a departmental network using Cisco Packet Tracer. It focuses on router interface configuration, Access Control Lists (ACLs), DHCP, and validating network connectivity, HTTP access, and FTP authentication while enforcing departmental access restrictions.

## Table of Contents

- [Project Overview](#project-overview)
- [Network Topology](#network-topology)
- [Tools and Technologies](#tools-and-technologies)
- [Configuration Steps](#configuration-steps)
- [Results and Findings](#results-and-findings)
- [Challenges and Solutions](#challenges-and-solutions)
- [Author](#author)

## Project Overview

The objective of this project was to configure and validate a simulated network connecting three departments: **IT, Sales, and HR**.

The project demonstrates practical networking skills, including:

- Configuring and verifying router interfaces.
- Connecting departmental networks using a router and switches.
- Implementing **Access Control Lists (ACLs)** to restrict unauthorised interdepartmental traffic.
- Configuring and verifying DHCP client addressing.
- Testing connectivity using ICMP ping.
- Validating HTTP webpage access and FTP authentication.
- Interpreting router command output and connectivity test results.

The project uses Cisco Packet Tracer to simulate a departmental network and verify whether its configuration behaves as intended.

## Network Topology

The network consists of one router connected to three departmental switches. Each switch connects the devices within its respective department.

### Network architecture

- **Router:** Provides connectivity between the IT, Sales, and HR networks.
- **IT department switch:** Connects the IT workstations and server.
- **Sales department switch:** Connects the Sales workstations.
- **HR department switch:** Connects the HR workstations.
- **Server:** Provides the HTTP webpage and FTP service used in the tests.
- **Client PCs:** Generate traffic for connectivity, webpage access, and FTP login testing.

### IP addressing scheme

The router interface verification output identifies three departmental gateways.

| Department | Router interface | Gateway IP address | Subnet mask | Network address |
|---|---|---|---|---|
| IT | `GigabitEthernet0/0` | `192.168.10.1` | `255.255.255.0` | `192.168.10.0/24` |
| Sales | `GigabitEthernet0/1` | `192.168.20.1` | `255.255.255.0` | `192.168.20.0/24` |
| HR | `GigabitEthernet0/2` | `192.168.30.1` | `255.255.255.0` | `192.168.30.0/24` |

The `/24` subnet mask is `255.255.255.0`. The interface output explicitly confirms this mask for the Sales gateway, while the HR client configuration confirms it for the HR network. The same mask for IT is consistent with the addressing scheme shown in the topology and test results.

### Devices and addressing evidence

| Device | Department or role | IP address | Subnet mask | Default gateway |
|---|---|---|---|---|
| Router `GigabitEthernet0/0` | IT gateway | `192.168.10.1` | `255.255.255.0` | Not applicable |
| Router `GigabitEthernet0/1` | Sales gateway | `192.168.20.1` | `255.255.255.0` | Not applicable |
| Router `GigabitEthernet0/2` | HR gateway | `192.168.30.1` | `255.255.255.0` | Not applicable |
| `Server0` | IT server | `192.168.10.2` | Not explicitly shown | Not explicitly shown |
| IT client | IT workstation | `192.168.10.3` | Not explicitly shown | Not explicitly shown |
| IT client | IT workstation | `192.168.10.4` | Not explicitly shown | Not explicitly shown |
| `PC5` | HR workstation | `192.168.30.2` | `255.255.255.0` | `192.168.30.1` |
| `PC2` | HTTP test client | Not explicitly shown | Not explicitly shown | Not explicitly shown |
| `PC3` | Sales-to-IT ping and FTP test client | Not explicitly shown | Not explicitly shown | Not explicitly shown |

The source document also labels `PC0`, `PC1`, `PC4`, and other departmental PCs in the topology. Their individual IP addresses are not legible or explicitly recorded in the extracted configuration evidence.

### Access control configuration

The router's access-list output shows an extended ACL named `Sales-ACL`:

```text
Extended IP access list Sales-ACL
10 deny icmp 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255
20 permit ip any any
```

This configuration:

- Denies ICMP traffic from the Sales subnet (`192.168.20.0/24`) to the IT subnet (`192.168.10.0/24`).
- Permits other IP traffic according to the displayed rules, subject to any other applicable network policies.
- Is shown as an inbound access list on `GigabitEthernet0/1`, the Sales-facing router interface.

The ACL is intended to prevent Sales devices from reaching IT devices using ping while allowing other traffic, including the demonstrated HTTP and FTP tests, subject to the relevant service and routing configurations.

## Tools and Technologies

- **Cisco Packet Tracer:** Network simulation and device configuration.
- **Cisco router CLI:** Router interface inspection and ACL verification.
- **Layer 2 switches:** Connecting departmental devices.
- **Router interfaces:** Interconnecting the IT, Sales, and HR networks.
- **IPv4 addressing and subnetting:** Configuring and identifying network segments.
- **Extended ACLs:** Filtering interdepartmental ICMP traffic.
- **DHCP:** Automatic client IP configuration.
- **ICMP:** Testing network reachability with `ping`.
- **HTTP:** Testing webpage accessibility.
- **FTP:** Verifying FTP server connectivity and authentication.
- **Command Prompt:** Running client-side network commands, including `ipconfig`, `ping`, and `ftp`.

## Configuration Steps

### 1. Build the network topology

1. Open Cisco Packet Tracer and create the network topology shown in the project document.
2. Add a router and three switches representing the IT, Sales, and HR departments.
3. Connect the router to the three departmental switches.
4. Connect the departmental PCs to their respective switches.
5. Connect `Server0` to the IT department switch.
6. Verify that the physical and logical connections match the intended topology.

### 2. Configure the router interfaces

Configure the router interfaces to serve as the default gateways for the three departmental networks.

The addressing shown in the verification output is:

- `GigabitEthernet0/0`: `192.168.10.1` for IT.
- `GigabitEthernet0/1`: `192.168.20.1` for Sales.
- `GigabitEthernet0/2`: `192.168.30.1` for HR.

The following commands illustrate the interface configuration consistent with the documented addresses. They are provided as a reproducible example; the source document records verification output rather than the complete original configuration commands.

```text
enable
configure terminal

interface gigabitEthernet0/0
ip address 192.168.10.1 255.255.255.0
no shutdown
exit

interface gigabitEthernet0/1
ip address 192.168.20.1 255.255.255.0
no shutdown
exit

interface gigabitEthernet0/2
ip address 192.168.30.1 255.255.255.0
no shutdown
exit

end
```

Verify the interface status and assigned IP addresses:

```text
show ip interface brief
```

The documented output shows the three GigabitEthernet interfaces with their respective IP addresses and both the interface and line protocol reported as `up`.

### 3. Configure and verify the Sales ACL

The project uses an extended ACL named `Sales-ACL` to block ICMP traffic from Sales to IT.

The documented ACL entries are:

```text
deny icmp 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255
permit ip any any
```

The first entry denies ICMP traffic from `192.168.20.0/24` to `192.168.10.0/24`. The second permits other IP traffic that is not denied by an earlier rule.

The following is an example of how the documented ACL could be configured and applied to the Sales-facing interface. It is illustrative rather than a verbatim record of the original commands.

```text
enable
configure terminal

ip access-list extended Sales-ACL
10 deny icmp 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255
20 permit ip any any
exit

interface gigabitEthernet0/1
ip access-group Sales-ACL in
exit

end
```

Verify the ACL entries:

```text
show access-lists
```

Inspect the Sales-facing interface and its access-list settings:

```text
show ip interface gigabitEthernet 0/1
```

The project evidence shows `Sales-ACL` applied as the inbound access list on `GigabitEthernet0/1`.

### 4. Configure and verify DHCP

The document includes a DHCP configuration section and an `ipconfig` output from `PC5`.

The recorded HR client configuration is:

- **IPv4 address:** `192.168.30.2`
- **Subnet mask:** `255.255.255.0`
- **Default gateway:** `192.168.30.1`

To validate client addressing in Packet Tracer:

1. Select the relevant PC.
2. Open the **Desktop** tab.
3. Open **Command Prompt**.
4. Run:

   ```text
   ipconfig
   ```

5. Check the displayed IPv4 address, subnet mask, and default gateway.

The `PC5` screenshot confirms that the HR client has an address within the `192.168.30.0/24` subnet and uses `192.168.30.1` as its gateway.

The document does not expose the complete DHCP pool configuration or lease details, so the exact pool commands and allocation range cannot be reproduced reliably.

### 5. Test blocked connectivity from Sales to IT

The project tests whether the ACL prevents Sales clients from pinging IT devices.

The documented test targets include `192.168.10.3` and `192.168.10.4`.

From the Sales client command prompt, run:

```text
ping 192.168.10.3
ping 192.168.10.4
```

The captured results show repeated messages indicating that the destination host is unreachable, with four packets sent and zero received for each test.

This outcome is consistent with the ACL's ICMP deny rule for traffic originating in `192.168.20.0/24` and destined for `192.168.10.0/24`.

### 6. Test HTTP webpage access

The HTTP test verifies that a client can access the webpage hosted on the IT server.

The server address shown in the project is `192.168.10.2`.

To perform the test:

1. Select the HTTP test client, identified as `PC2` in the screenshot.
2. Open the **Desktop** tab.
3. Open **Web Browser**.
4. Enter the following address:

   ```text
   http://192.168.10.2
   ```

5. Select **Go** and inspect the webpage.

The screenshot shows the Cisco Packet Tracer welcome webpage loading successfully, demonstrating HTTP access to the server.

### 7. Test FTP connectivity and authentication

The FTP test checks whether a client can connect to the server and authenticate successfully.

The project uses the server address `192.168.10.2`.

From the client command prompt, run:

```text
ftp 192.168.10.2
```

The captured session shows the following sequence:

1. The client attempts to connect to `192.168.10.2`.
2. The server responds with an FTP welcome message.
3. The client supplies a username.
4. The server requests the password.
5. The server returns a successful login response.
6. The client reaches the `ftp>` prompt.

The screenshot shows a successful login using the username `technova`. The password is masked in the evidence and is therefore not reproduced here.

### 8. Test permitted connectivity from HR to IT

The final connectivity test checks communication from the HR network to IT.

The screenshot shows `PC5` attempting to ping `192.168.10.5`, followed by tests against `192.168.10.4`.

Run the documented tests from the HR client:

```text
ping 192.168.10.5
ping 192.168.10.4
```

The screenshot records a timeout when testing `192.168.10.5`. The first test against `192.168.10.4` receives three replies out of four packets, while a subsequent test receives four replies out of four packets.

The successful second test demonstrates connectivity between the HR client and `192.168.10.4`. The initial timeout indicates that connectivity was not immediately successful, but the document does not identify the cause or record a specific corrective action.

### 9. Review and document the results

1. Review the router interface status using `show ip interface brief`.
2. Confirm the configured ACL entries using `show access-lists`.
3. Inspect the inbound ACL application using `show ip interface gigabitEthernet 0/1`.
4. Verify the client addressing using `ipconfig`.
5. Compare the Sales-to-IT and HR-to-IT ping results.
6. Confirm successful HTTP access and FTP authentication.
7. Retain the screenshots as evidence of the network's configuration and observed behaviour.

## Results and Findings

The project documented the following results:

- **Router interfaces:** The three GigabitEthernet interfaces were reported as operational, with the expected departmental gateway addresses.
- **ACL verification:** `Sales-ACL` was present and configured to deny ICMP traffic from Sales to IT while permitting other IP traffic.
- **DHCP/client addressing:** `PC5` displayed IPv4 address `192.168.30.2`, subnet mask `255.255.255.0`, and default gateway `192.168.30.1`.
- **Sales-to-IT connectivity:** Ping requests to `192.168.10.3` and `192.168.10.4` failed, consistent with the intended ICMP restriction.
- **HTTP:** The webpage hosted at `192.168.10.2` loaded successfully.
- **FTP:** The client connected to `192.168.10.2` and completed FTP login successfully.
- **HR-to-IT connectivity:** A subsequent ping test to `192.168.10.4` received four replies out of four packets, confirming successful connectivity during that test.

Overall, the results demonstrate the practical use of router interfaces, subnetting, ACL-based traffic control, client IP configuration, and application-service verification in a simulated departmental network.

## Challenges and Solutions

The source document records connectivity failures and successful verification tests but does not provide a written troubleshooting log. The following challenges are therefore limited to issues visible in the captured evidence; potential solutions are distinguished from fixes explicitly documented.

### 1. Restricting unauthorised interdepartmental traffic

**Challenge:** Sales clients needed to be prevented from reaching IT devices through ICMP while preserving access to other network services.

**Solution:** The router's `Sales-ACL` contains a deny rule for ICMP traffic from `192.168.20.0/24` to `192.168.10.0/24`, followed by a permit rule for other IP traffic. The blocked ping tests provide evidence consistent with the intended restriction.

### 2. Interpreting unsuccessful ping results

**Challenge:** The Sales-to-IT tests returned destination-unreachable messages. The HR-to-IT test also initially returned timeouts before a subsequent test succeeded.

**Solution and verification:** The Sales results were consistent with the configured ACL. For the HR test, the later four successful replies confirmed that connectivity to `192.168.10.4` was working at that point. The source document does not record what caused the initial HR timeout or what, if anything, was changed to resolve it.

In a similar troubleshooting scenario, useful checks would include verifying the client IP configuration, checking the destination address, inspecting router interfaces and ACLs, and repeating the connectivity test.

### 3. Ensuring application services remained accessible

**Challenge:** Network access restrictions must be validated alongside the availability of services such as HTTP and FTP.

**Solution:** The project tested HTTP access through the browser at `http://192.168.10.2` and FTP access using `ftp 192.168.10.2`. Both tests were successful in the captured evidence, demonstrating that these services were accessible during testing.

### 4. Verifying client IP configuration

**Challenge:** Connectivity depends on clients having appropriate IP addresses, subnet masks, and default gateways.

**Solution:** The project used `ipconfig` on `PC5` to inspect its network settings. The output showed `192.168.30.2/24` with gateway `192.168.30.1`, consistent with the HR network addressing scheme.

## Author

**Chiamaka Ojimba**

- **Project:** Networking Capstone Project
- **Date:** 10 October 2026
- **Location:** United Kingdom
