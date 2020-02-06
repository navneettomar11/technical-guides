# How Does the Internet Work?

## Introduction
The internet's growth  has become explosive and it seems impossible to escape the bombardment of www.com's seen constantly on television, heard on radio and seen in magazines. Beacause the Internet has become such a large part of our lives, a good understanding is needed to use thid new tool most effectively.

## Where to Begin? Internet Address
Because the Internet is a global network of computers each computer connected to the Internet must have a unique address. Internet addresses are in the form **nnn.nnn.nnn.nnn** where *nnn* must be a number from 0-255. This address is known as an IP address (IP stands for Internet Protocol).

The picture below illustrates two computer connected to the Internet; your computer with IP address 1.2.3.4 and the another computer with IP address 5.6.7.8. The internet is represented as an abstract object in-between.

[!Diagram-1](images/ruswp_diag1.gif)

If you connect to the Internet through an Internet Service Provider(ISP), you are usually assigned a temporary IP address for the duration of your dial-in session. If you connect to the Internet from a local area network(LAN) your computer might have a permanent IP address or it might obtain temporary one from a DHCP(Dynamic Host Configuration Protocol) server. In any case, if you are connected to the Internet, your computer has a unique IP address.

|Check It out - The Ping Program|
|-------------------------------|
|If you're using Microsoft Windows or a flavor of Unix and have a connection to the Internet, there is a handy program to see if a computer on the Internet is alive. It's called **ping**, probably after the sound made by older submarine sonar systems. If you are using Window, start a command prompt window. If you're using a flavor of Unix get to a command prompt. Type *ping www.yahoo.com*. The ping program will send a 'ping' (actually an ICMP (Internet Control Message Protocol) echo request message) to the named computer. The pinged  computer will respond with a reply. The ping program will count the time expired until the reply comes back (if it does). Also, if you enter a domain name (i.e. www.yahoo.com) instead of an IP Address, ping will resolve the domain name and display the computer's IP Address.|

## Protocol Stacks and Packets
So your computer is connected to the Internet and has a unique address. How does it 'talk' to other computers connected to the Internet? An example should serve here: Let's say your IP address is 1.2.3.4 and you want to send a message to the computer 5.6.7.8. The message you want to send is "Hello Computer 5.6.7.8!". Obviously, the message must be transmitted over whatever kind of wire connects your computer to the Internet. Let's say you've dialed into your ISP from home and the message must be transmitted over line. Therefore the message must be translated from alphabetic text into electronic signals, transmitted over the Internet, then translated back into alphabetic text. How is this accomplished? Through the use of a **protocol stack**. Every computer needs one to communicate on the Internet and it is usually built into the computer operating system(i.e. Window, Unix, etc). The protocol stack used on the Internet is referred to as the TCP/IP protocol stack because of the two major communication protocol used. The TCP/IP stack looks like this:

|Protocol Layer| Comments |
|--------------|----------|
|Application Protocol Layer| Protocol specific to applications such as WWW, email, FTP, etc.|
|Transmission Control Protocal Layer|TCP directs packet to a specific application on a computer using a port number.|
|Internet Protocol Layer|IP directs packet to a specific computer using an IP address.|
|Hardware Layer|Converts binary packet data to network signals and back (E.g. ethernet network card, modem for phone lines, etc.)|

If we were to follow the path that the message "Hello computer 5.6.7.8!" took from our computer to the computer with IP Address 5.6.7.8, i would happen something like this:

[!Diagram-2](images/ruswp_diag2.gif)

1. The message would start at the top of the protocol stack on your computer and work it's way downward.
2. If the message to be sent is long, each stack layer that the message passes through may break the message up into smaller chunks of data. This is because data sent over the Internet(and most computer networks) are sent in manageable chunks. On the Internet, these chunks of data are known as **packet**.
3. The packets would go through the Application Layer and continue to the TCP layer. Each packet is assigned a **port number**. Many programms may be using the TCP/IP stack and sending messages. We need to know which program on the destination computer needs to receive the message because it will be listening on a specific port.
4. After going through the TCP layer, the packets processed to the IP layer. This is where each packet receives it's destination address 5.6.7.8.
5. Now that our message packets have a port number and an IP address, they are ready to be sent over the Internet. The hardware layer takes care of tunning our packets containing the alphabetic text to our message into electronic signals and transmitting them over the phone line.
6. On the other end of the phone line your ISP has a direct connection  to the Internet. The ISP router examines the destination address in each packet and determines wher to send it. Often, the packet's next stop is another router.
7. Eventually, the packets reach computer 5.6.7.8. Here, the packets start at the bottom of the destination computer TCP/IP stack and work upwards.
8. As the packet go upward through the stack, all routing data that the sending computer's stack added(such as IP address and port number) is stripped from the packets.
9. When the data reaches the top of the stack, the packets have been re-assembled into their original form, "Hello computer 5.6.7.8!".