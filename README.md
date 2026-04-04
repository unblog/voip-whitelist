# proposal of a firewall rule using GDMS UCMRC

This article describes the configuration of a firewall filter rule using GDMS, UCMRC and CloudUCM, in interactions with Wave communication, UCM-GDMS communication, phone and proxy server, UCM-endpoint communication.

## Description

The necessary services and ports are identified to provide a rule on the WAN interface, especially when GeoIP and similar country-managed geographical address ranges control and limit global reachability.

## Configuration

Instructions on how to configure and set up the filter rule.

Here on a FortiGate, go to Policy & Objects -> Services -> Create New.

Enter a name, for example: UCMRC-nat-b, and add the UDP and TCP ports as shown.

![Edit Service](images/edit-service-ucmrc.png)

Next, go to IPv4 Policy and right-click, then select from the context menu -> Insert Empty Policy -> Above (This rule must be before the GeoIP blocking rule).

Edit the policy as shown in the image.

![IPv4 Policy](images/edit-policy.png)

The new policy should look something like this in the policy overview.

![Edit Policy](images/policy-lan-wan.png)

Finaly, create an identical rule in reverse order from WAN to LAN with the same services, this rule must also be placed before the GeoIP blocking rule.

![Edit Policy](images/policy-wan-lan.png)

## Usage

Use your Wave App and call a participant and try to invite participants to conferences, making sure that the audio sound transmission is successful for all participants and that everyone can understand each other.

## Cause

Peer Blocking:

After your device obtains its public IP address via the STUN server, it attempts to send and receive data packets directly to and from the peer.

Even if your device sends the first packet (hole punching), many firewalls with GeoIP filters block the peer's response at the WAN interface if the peer's IP address originates from a restricted country.

In this case, the data packet (e.g., the audio signal during a phone call) never reaches your LAN device, even though the connection should technically be established.

Stateful Inspection vs. GeoIP:

Modern firewalls are stateful, meaning they automatically allow responses to outgoing requests.

The problem: GeoIP filters often intervene before the connection status has been checked. If a GeoIP rule states "Block everything from country X", the packet from the remote end is often immediately discarded, even before the firewall recognizes that your LAN device actually requested this packet.

## Contributing

Everyone is free to use and distribute this post without restriction; however, all use is at your own risk, and any liability is excluded.

## License

unblog/voip-whitelist is licensed under the GNU General Public License v3.0.