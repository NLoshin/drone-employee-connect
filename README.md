# DroneEmployee Connect #

**Ground station administrative tool stack**

File [wifi.py](wifi.py)

This file contains a set of functions for connecting wifi modules directly to the docker container of the drone. 

For example:
```Python
def spawn(name, wlan, ssid, password):
    c = from_env().containers.get(name)
    iwcall = 'iw phy phy{0} set netns {1}'.format(wlan[-1], c.attrs['State']['Pid'])
    wpacall = 'sh -c "wpa_passphrase {0} {1} > /tmp/wpa.conf"'.format(ssid, password)
    supcall = 'sudo wpa_supplicant -B -i {0} -c /tmp/wpa.conf'.format(wlan)
    print(system_call(iwcall))
    print(c.exec_run(wpacall))
    print(c.exec_run(supcall))
    print(c.exec_run('sudo dhcpcd {0}'.format(wlan)))
```

This function generates a system request to transfer the adapter to the docker container.

We use this function

```Python
...
   for name, wlan, ssid, password in links:
        spawn(name, wlan, ssid, password)
```
