# tweaks
Tweaks I've made to my stock Ubuntu OS install 


# audio

Add the ability to support [IEC958 (S/PDIF)](https://askubuntu.com/q/1098666/3188) by installing the following packages:

```shell
sudo apt install libavresample-dev pavucontrol libasound2-plugins-extra
```

Control the settings via the Advanced tab of the relevant Output Device in the `pavucontrol` utility.
