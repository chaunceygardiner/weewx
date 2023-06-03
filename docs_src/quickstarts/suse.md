# Installation on SUSE systems 

This is a guide to installing WeeWX from an RPM package systems based on SUSE,
such as openSUSE Leap.

WeeWX V5 requires Python 3.7 or greater, which is only available on SUSE-15 or
later.  For older SUSE systems, either use WeeWX V4, or install Python 3.7 then
install WeeWX V5 using pip.


## Configure zypper

The first time you install WeeWX, you must configure `zypper` so that it will
trust weewx.com, and know where to find the WeeWX releases.

1. Tell your system to trust weewx.com:

    ```shell
    sudo rpm --import https://weewx.com/keys.html
    ```

2. Tell `zypper` where to find the WeeWX repository.

    ```shell
    curl -s https://weewx.com/suse/weewx-suse15.repo | \
        sudo tee /etc/zypp/repos.d/weewx.repo
    ```


## Install

Install WeeWX using `zypper`. When you are done, WeeWX will be running the
`Simulator` in the background as a daemon.

```
sudo zypper install weewx
```


### Verify

After 5 minutes, copy the following and paste into a web browser:

    /var/www/html/index.html

You should see simulated data.

Check the system log `/var/log/messages` for problems.


## Configure

To switch from the `Simulator` to real hardware, reconfigure the driver.

```shell
# Stop the daemon:
sudo systemctl stop weewx
# Reconfigure to use your hardware:
sudo weectl station reconfigure
# Remove the old database:
sudo rm /var/lib/weewx/weewx.sdb
# Start the daemon:
sudo systemctl start weewx
```


## Customize

To enable uploads or to customize reports, modify the configuration file
`/etc/weewx/weewx.conf`. See the [*User Guide*](../../usersguide) and
[*Customization Guide*](../../custom) for details.

WeeWX must be restarted for configuration file changes to take effect.

```shell
sudo systemctl restart weewx
```


## Uninstall

To uninstall WeeWX, removing configuration files but retaining data:

```shell
sudo zypper remove weewx
```
To remove data:

```shell
sudo rm -r /var/lib/weewx
sudo rm -r /var/www/html/weewx
```