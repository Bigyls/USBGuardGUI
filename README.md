
# USBGuardGUI

USBGuardGUI is a graphic overlay of USBGuard which allows to authorize or reject a device.

![](/doc/USBGuardGUIPopup.png)

![](/doc/USBGuardGUIDiagram.png)

⚠️ USBGuardGUI has no responsibility for USBGuard bugs or non-detection of devices managed by USBGuard. USBGuardGUI is just a GUI that interacts with USBGuard and DBus.

## Installation

Installation can be done via `.deb` file.

Go to the [latest release](https://github.com/Bigyls/USBGuardGUI/releases/latest) for download `.deb` file.

Run command (where `DEB_PACKAGE` is the downloaded file):

```shell
sudo apt install  ./<DEB_PACKAGE>
```

Do not get attention to `Download is performed unsandboxed`.

**Don't forget to logout/login after install.**

## Contributing

You can create issues and PRs by yourself if you experienced a bug or if you have an idea for a new feature.

Please respect [Conventional Commits specification](https://www.conventionalcommits.org/en/v1.0.0/) for all of your commits.
