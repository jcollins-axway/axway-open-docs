{
    "title":"Unattended installation",
    "linkTitle":"Unattended installation",
    "weight":"100",
    "date":"2019-08-09",
    "description":"Install or uninstall API Portal in unattended or silent mode."
}

## Install API Portal in unattended mode

To install the API Portal software in unattended mode:

1. Download the installation package for your OS from [Axway Support](https://support.axway.com/) and upload it to your host machine.
2. Log in to the host machine as the `root` user.
3. Extract the installation package:

    ```
    tar xpvzf <package_name>.tgz
    ```

4. Run the install script with the appropriate script options. For example:

    ```
    ./apiportal_install.sh --mysql-ssl=n --install-path="/opt/axway/apiportal/htdoc" --mysql-database=joomla --mysql-host=localhost --mysql-port=3306 --mysql-username=apiportal --mysql-password=password --weak-mysql-password=y --initial-ha-instance=n --php-ini="/etc/" --apache-config="/etc/httpd/conf.d/" --use-encryption-key=n --use-ssl=y --ssl-type=2 --restart-apache=y
    ```

    The following describes the script options used in this example:

    * `--mysql-ssl`: Accepts yY/nN. Flag indicating whether to use MySQL in SSL mode.
    * `--install-path`: The install path for API Portal. Example: `/opt/axway/apiportal/htdoc`
    * `--mysql-database`: The database to use for API Portal.  
    * `--mysql-host`: Database host.
    * `--mysql-port`: Database port. Example: `3306`.
    * `--mysql-username`: Database user.
    * `--mysql-password`: Database password.
    * `--weak-mysql-password`: Accepts yY/nN. Flag indicating whether installation should continue when MySQL password is weak.
    * `--ha-instance`        : Accepts yY/nN. Flag indicating whether to use HA setup. Use only for instances that are not initial. For the initial instance use the     `--initial-ha-instance` option.
    * `--initial-ha-instance`: Accepts yY/nN. Flag indicating whether this is the initial instance for HA setup.
    * `--php-ini`            : The directory where `php.ini` file is located. Example: `/etc`.
    * `--apache-config`      : The directory where the Apache configuration files are located. Example: `/etc/httpd/conf.d`.
    * `--use-encryption-key` : Accepts yY/nN. Flag indicating whether to use an encryption key. This option is required for public API mode.
    * `--use-ssl`            : Accepts yY/nN. Flag indicating whether API Portal will be served by SSL.
    * `--ssl-type`           : Accepts 1 (use custom certificate and private key) or 2 (use self-signed certificate). Indicates what SSL type to use.

To see the complete list of options that you can use during the installation, run the following command:

```
.apiportal_install.sh --help
```

## Uninstall API Portal in unattended mode

To uninstall the API Portal software in unattended mode:

1. Log in to the host machine as the `root` user.
2. Change to the directory containing the API Portal installation package.
3. Run the uninstall script with the appropriate script options. For example:

    ```
    ./apiportal_uninstall.sh --mysql-database=testDB --mysql-host=localhost --mysql-port=3306 --mysql-username=root --mysql-password=Admin@123
    ```

    The following describes the script options used in this example:

    * `--mysql-database`: The database to use for API Portal.
    * `--mysql-host`: Database host.
    * `--mysql-port`: Database port.
    * `--mysql-username`: Database user.
    * `--mysql-password`: Database password.

To see the complete list of options that you can use when uninstalling API Portal, run the following command:

```
.apiportal_uninstall.sh --help
```

Watch this video to learn how to install and uninstall API Portal using unattended mode:

{{< youtube 1yijU63_z6g >}}

## Encrypt the Public API user password in unattended mode

To encrypt the Public API mode user password in unattended mode:

1. Log in to the host machine as the `root` user.
2. Change to the directory containing the API Portal installation package.
3. Run the encryption script with the appropriate script options. For example:

    ```
    ./apiportal_encryption.sh --encryption-key="/publicapi/encryption/user.key"
    ```

    Install options:

    * `--encryption-key`: The place where the encryption key is stored. Example: `/home/encryption/key`. The last segment is the file name (in this example it is called `key`).
    * `-h` or `--help`: Prints all available options for the script.
