# rtl8821AU

Linux Driver for USB WiFi Adapter using Realtek 8821AU

This is my personal attempt to let [Buffalo WI-U2-433DHP](http://buffalo.jp/product/wireless-lan/client/wi-u2-433dhp/) working on my Raspberry Pi 2 and CentOS 7.3.1611. And, it works!

## Known Linux Distros

* CentOS 7.3.1611

## Known Devices

* [Buffalo WI-U2-433DHP](http://buffalo.jp/product/wireless-lan/client/wi-u2-433dhp/)
* [Edimax EW-7811UAC](http://www.edimax.com/edimax/merchandise/merchandise_detail/data/edimax/in/wireless_adapters_ac600_dual-band/ew-7811uac/)

## Eduroam

For connecting to "eduroam" network, use the following configuration in `/etc/wpa_supplicant/wpa_supplicant.conf`.

```
ctrl_interface=/var/run/wpa_supplicant
ctrl_interface_group=wheel

network={
    ssid="eduroam"
    scan_ssid=1
    key_mgmt=WPA-EAP
    eap=TLS
    pairwise=CCMP
    group=CCMP
    phase2="auth=MSCHAPV2"
    identity="<Your Email Address>"
    ca_cert="<CA Certificate>"
    client_cert="<Your Ceritificate>"
    private_key="<Your Decrypted Private Key>"
}
```

You often get the certificate in form of PKCS12 in a `.p12` file, for example `youremail.p12`. To get the required files for the above configuration, you can use the following command on Linux.

To get the "CA Certificate",

```
openssl pkcs12 -in youremail.p12 -nokeys -cacerts -out ca.pem
```

To get "Your Ceritificate",

```
openssl pkcs12 -in youremail.p12 -nokeys -clcerts -out youremail.crt.pem
```

To get "Your Decrypted Private Key",

```
openssl pkcs12 -in youremail.p12 -nocerts -out youremail.key.pem

openssl rsa -in youremail.key.pem -out youremail.dkey.pem
```
