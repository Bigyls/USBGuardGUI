# USBGuard

## What is USBGuard ?

[USBGuard](https://github.com/dkopecek/usbguard) offers a command-line white/black-listing mechanism for USB-devices. Inspiration for this is drawn from exploits like BadUSB. It makes use of a device blocking infrastructure included in the Linux kernel and consists of a daemon and some front-ends.
**USBGuard** should already be packaged for your favorite Linux distribution.

```ad-warning
Before you start using usbguard be sure to configure it first unless you know exactly what you are doing (all USB devices will get blocked).
```

## Installation

In our case, we can't use the mask command because USBGuard is not installed yet. But what mask actually does is just create a symlink. So all we have to do is create it manually:

```shell
sudo ln -s /dev/null /etc/systemd/system/usbguard.service
```

And now we can safely install USBGuard:

```shell
sudo apt install usbguard
```

## Configuration

First thing to do is create an initial policy that whitelists all of our usb devices. 
Now it's a good time to plug-in devices that you tend to use often and you already trust. You can of course whitelist devices at any point: `usbguard generate-policy`

The above command will display the list of your currently plugged devices with an `allow` keyword in the beginning. Let's save that to USBGuard's configuration file:

```shell
usbguard generate-policy > /etc/usbguard/rules.conf
```

Now it's safe to unmask, start and enable USBGuard daemon:

```shell
sudo systemctl unmask usbguard.service
sudo systemctl start usbguard.service
sudo systemctl enable usbguard.service
```

## Testing

To test this actually works try to plug a new device, not whitelisted yet. Let's a simple USB stick. Hopefully it will be blocked. To confirm that:

```
usbguard list-devices
```

This lists all your detected devices. The new device you just plugged-in should have a `block` keyword in the beginning. For a more filtered output:

```
usbguard list-devices | grep block
```

You should see something like this:

```
13: block id 0xxx:0xxx serial <...>
```

### Allowing devices

Now let's say you actually want to unblock this device, because it came from a friend you trust. The command we run above also contained an ID number. The first thing on that line. We can use that and allow that device:

```
usbguard allow-device 13
```

### Whitelisting devices

Using `allow-device` doesn't whitelist the device for ever. So let's say you bought a new external disk and you want to whitelist it. USBGuard has an `append-rule` command. You just need to paste the whole device line starting with an `allow` keyword.

Plug the device and see the USBGuard output:

```
usbguard list-devices | grep block
```

You should see something like this:

```
21: block id 0xxx:0xxx serial <...>
```

Copy the whole line starting from `id` and then use it _but_ prefix it with an `allow` keyword (mind the single quotes used to wrap the entire rule):

```
usbguard append-rule 'allow id 0xxx:0xxx serial <...>'
```

### Editing rules

At any point you can see the whitelisted devices:

```
usbguard list-rules
```

And you use the id number in the beginning of each line in order to interact with that specific rule. For example to remove a device:

```
usbguard remove-rule <id>
```
