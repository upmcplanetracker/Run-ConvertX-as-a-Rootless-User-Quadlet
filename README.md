ConvertX Podman User Quadlet
============================

**Disclaimer:** I am not affiliated with, associated, authorized, endorsed by, or in any way officially connected with the ConvertX project.

This guide covers deploying **ConvertX**—a powerful, multi-format file converter—as a **User (Rootless) Quadlet** on Ubuntu 25.10.

System Information
------------------

*   **Tested Environment:** Ubuntu 25.10
*   **Podman Version:** 5.4.x
*   **Type:** User-level Quadlet (Rootless)

1\. The Quadlet File (convertx.container)
-----------------------------------------

This configuration uses the `%h` variable, which ensures the container data is stored safely in your user's home directory (`~/convertx/data`).

**Security Note:** It is highly recommended to change the `JWT_SECRET` in the container file to a unique random string before deploying.

2\. Installation & Setup
------------------------

### Step A: Create the User Config Path

If you haven't already, create the directory where user quadlets are stored:

`mkdir -p ~/.config/containers/systemd`

### Step B: Move the File

`mv convertx.container ~/.config/containers/systemd/`

### Step C: Enable Linger

This ensures your user-level containers start on boot and stay running after you logout:

`sudo loginctl enable-linger $USER`

3\. Activation
--------------

Because this is a user quadlet, all `systemctl` commands must include the `--user` flag:

`systemctl --user daemon-reload`
`systemctl --user start convertx`
`systemctl --user enable convertx`

4\. Verification
----------------

### Check Service Status

`systemctl --user status convertx`

### Check Container Logs

`journalctl --user -u convertx -f`

Once running, access the converter at `http://[Your-IP]:3002`.

5\. Maintenance
---------------

To update the image via the registry (as defined in the `AutoUpdate` key):

`podman auto-update`
