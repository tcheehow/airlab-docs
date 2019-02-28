# GPIO Power Off

{% hint style="info" %}
\[1\] [https://developer.toradex.com/knowledge-base/gpio-\(linux\)\#GPIO\_Power\_Management\_Keys](https://developer.toradex.com/knowledge-base/gpio-%28linux%29#GPIO_Power_Management_Keys)

\[2\] [https://developer.toradex.com/knowledge-base/gpio-%28linux%29\#GPIO\_PowerOff](https://developer.toradex.com/knowledge-base/gpio-%28linux%29#GPIO_PowerOff)
{% endhint %}

####  GPIO Keyboard Driver

In `src/kernel/arch/arm/boot/dts/imx6qdl-var-dt6cb-buttons.dtsi` , configure the desired `GPIO` as a `KEY_POWER`. In this case, `GPIO[5]11` is configured.

```text
gpio-keys {
		compatible = "gpio-keys";

    power {
      label = "Power";
      gpios = <&gpio5 11 0>;
      linux,code = <KEY_POWER>; /* KEY_POWER */
      debounce-interval = <10>;
      gpio-key,wakeup;
    };
```

#### Tag the GPIO Input Device

Create the udev rule to add the power-switch tag to the GPIO key event source, using`sudo nano /etc/udev/rules.d/power-key.rules`

```text
ACTION=="remove", GOTO="power_switch_end"
SUBSYSTEM=="input", KERNEL=="event*", ENV{ID_PATH}=="platform-gpio-keys*", ATTRS{keys}=="*", TAG+="power-switch" 
LABEL="power_switch_end"
```

#### GPIO Power-Off

To power off the system completely after a shutdown, a GPIO can be configured as a signal to power management circuitry to switch off the system fully.

In `src/kernel/arch/arm/boot/dts/imx6qdl-var-dt6cb-buttons.dtsi`, append the following to configure `GPIO[1]11` as the desired `GPIO`.

```text
gpio-poweroff {
    compatible = "gpio-poweroff";
    gpios = <&gpio1 1 1>;
    };
```

#### Recompile 

The bootloader, kernel, modules and rootfs need to be recompiled and flash into the system. 

