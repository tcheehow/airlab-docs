# Edison as Access Point

References: [https://github.com/ArduPilot/companion](https://github.com/ArduPilot/companion)

1. In `etc/default/hostapd`, uncomment `DAE_CONF` and add `/etc/hostapd/hostapd.conf` to it, it should look like

```text
DAE_CONF="/etc/hostapd/hostapd.conf"
```

2. Add the following to the end of /etc/rc.local

```text
ifdown wlan0
/etc/init.d/hostapd restart
ifup wlan0
```

3. in `/etc/init.d/hostapd` add `/etc/hostapd/hostapd.conf` to `DAE_CONF`

4. in `/etc/network/interfaces`,

```text
# uncomment to 4 lines in /etc/network/interfaces for hostapd and 
# comment out the wlan0 setup for usual internet access
```

5. in `/etc/hostapd/hostapd.conf` , customise the AP password by modifying the `wpa_psk` with the `wpa_passphrase`

6. install access point package

```text
apt-get install -y hostapd isc-dhcp-server
```



