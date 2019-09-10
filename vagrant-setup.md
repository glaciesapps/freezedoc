# Setup Vagrant

IceHrm development environment is packaged as a Vagrant box. It includes php7, nginx, phpunit and other software required for running icehrm.It is easy to setup. 


Setup IceHrm Development Environment
------------------------------------
- Clone freeze from https://github.com/glaciesapps/freeze.git or download the source

- Install Vagrant [https://www.vagrantup.com/downloads.html](https://www.vagrantup.com/downloads.html)

- Run vagrant up in freeze root directory (this will download freeze vagrant image)

```
~ $ vagrant up
```

- Run vagrant ssh to login to the Virtual machine

```
~ $ vagrant ssh
```

- Add following entries to the end of the host file to map freeze domains to VagrantBox (on MacOS and Linux this is /etc/hosts | on windows this is Windows\System32\Drivers\etc\hosts)

```
192.168.40.41   app.freeze.test
192.168.40.41   clients.freeze.test
```

- Navigate to [http://clients.freeze.test/dev](http://clients.freeze.test/dev) to load application from VM. (user:admin/pass:admin)

### Notes to Developers

- When ever you have done a change to JavaScript or CSS files in freeze/web rebuild the frontend
```
~ $ cd /vagrant
~ $ gulp
