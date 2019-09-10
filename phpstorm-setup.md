# Setup IDE (PhpStorm)

First we need to make a database connection.
In the Database tool window (View -> Tool Windows -> Database), click the Data Source Properties icon.

In the Data Sources and Drivers dialog, click the Add icon (The Add icon) and select MySQL.

Then click SSH/SSL tab and put a tick on Use SSH tunnel checkbox. Provide the details .
192.168.40.41 as the Proxy host.
vagrant as both Proxy user and Proxy password.

In the General tab provide dev as Host.User and Password.

Now you can test the connection.
Then apply the changes.


Next we need to setup remote debugging with xdebug

Download the Xdebug extension compatible with your PHP version and install it
In the Settings/Preferences dialog (Ctrl+Alt+S), click PHP under Languages & Frameworks.

On the PHP page that opens, click the Browse button next to the CLI Interpreter field.
