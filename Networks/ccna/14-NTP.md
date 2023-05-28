
# NTP

![[Pasted image 20230527211952.png]]
^ Higher stratum level = less accurate the clock
^ NTP servers examples: `time.windows.com` or `time.google.com`


## Reference Clocks

![[Pasted image 20230527212048.png]]


## NTP Hierarchy

![[Pasted image 20230527212116.png]]
^ Device peering is called **Symmetric active** mode

Thus, Cisco devices can operate in 3 modes:
1. Server mode
2. Client mode
3. Symmetric active mode

**NOTE**: An NTP client can sync to multiple servers

### Primary servers

![[Pasted image 20230527212304.png]]

### Secondary servers

![[Pasted image 20230527212316.png]]


## The importance of time in networks

![[Pasted image 20230527211809.png]]

`Syslog` is the protocol to keep device logs


# Manual Time configuration

**Note**: Calendar means device's hardware clock, while Clock refers to the device's software clock

![[Pasted image 20230527222430.png]]


## Hardware Clock (Calendar) configuration

![[Pasted image 20230527222510.png]]

![[Pasted image 20230527222539.png]]


## Configure Timezone

![[Pasted image 20230527222558.png]]


## Configuring Daylight Saving Time (Summer time)

![[Pasted image 20230527222758.png]]


## Summary of Commands

![[Pasted image 20230527222831.png]]



# NTP Configuration

![[Pasted image 20230527212939.png]]
^  Its best to select multiple servers as NTP servers, in case 1 goes down
^ we can choose a "preferred" ntp server, however, it is not necessary

![[Pasted image 20230527213024.png]]
^ * (asterisk) represents the NTP server that R1 is currently syncing to
- this may change from time to time
^ The + sign in front of others mean they are candidates, but currently not being used
^ a server marked as outlyer or falseticker will not be synced with
^ note: ref clock .GOOG. is stratum 0 google clock
^ st = 1 shows that these are secondary servers

![[Pasted image 20230527213315.png]]
^ note stratum = 2
- because R1 is syncing its time to Google's NTP servers, it automatically becomes an NTP server itself (stratum level 1 higher than Google's NTP servers). Now other devices can sync their time to R1
^ note: reference clock address

![[Pasted image 20230527213432.png]]
^ after setting the timezone, we update the hardware clock (aka calendar) with the time learned via NTP
- The hardware clock tracks the date and time on the device even if it restarts, power is lost etc. When the system is restarted, the hardware clock is used to initialize the software clock


Now we configure a loopback address on R1 and set R2's NTP server to R1 below:

![[Pasted image 20230527213705.png]]
**Note**: st = 2, coz Google's reference clock = st 0, Google NTP servers = st 1, hence R1 = st2, thus R2 = st 3

Next, R3 is synced to use R1 and R2 as its NTP servers:
![[Pasted image 20230527213849.png]]
**Note**: R1 becomes the preferred server because of lower stratum level



## NTP Server mode configuration

![[Pasted image 20230527214331.png]]
^ **NOTE**: default stratum is 8
^ **NOTE**: loopback address used as the address of R1's NTP server



## NTP Symmetric Active mode config

![[Pasted image 20230527221953.png]]


## NTP Authentication

![[Pasted image 20230527222111.png]]

![[Pasted image 20230527222119.png]]


## NTP Commands Review


![[Pasted image 20230527222159.png]]

---


# Quiz

![[Pasted image 20230527223005.png]]
(c)


![[Pasted image 20230527223038.png]]
(d)


![[Pasted image 20230527223122.png]]
(a) - since the stratum of R1's NTP server is shown as 8, `ntp master 9` must have been used


![[Pasted image 20230527223438.png]]
(c)


![[Pasted image 20230527223516.png]]
(c, d, f, g)


![[Pasted image 20230527224447.png]]
(a)








