# Set up this project

## Dependencies

**Mac**:

(Get Homebrew)

```
brew install go node mysql
```

**Ubuntu**:

ubuntu install [go](http://www.hostingadvice.com/how-to/install-golang-on-ubuntu/) [node](http://www.hostingadvice.com/how-to/install-nodejs-ubuntu-14-04/) [mysql](https://support.rackspace.com/how-to/installing-mysql-server-on-ubuntu/)

### Go

*This project requires golang v1.8 and above to run*

Go requires an exported bash variable named `GOPATH` that points to the directory on your machine that will host **all** Go projects (weird, we know). The convention is to use `~/go` as the Go directory. Set it up by adding this line to your `~/.bashrc` (Ubuntu) or `~/.bash_profile` (Mac):

```
export GOPATH=$HOME/go
```

### MySQL

Run MySQL with `brew services start mysql` (Mac) or `sudo service mysql start` (Ubuntu).

Shawty expects a running MySQL on localhost:3306, with a database named `urlshortener`, and a user with access to that database with username `url` and password `password`. All this information is available in the file `service.go`.

Gain access to the `mysql` shell as the root user using:

```
mysql -u root
```

By default, the root user will not have a password configured. If you've already used MySQL, and set a password before, use the `-p` flag to enter the shell with a password. 

and execute the following commands:

```
mysql> CREATE DATABASE urlshortener;
mysql> SHOW databases; // verify new db is there
mysql> CREATE USER 'url'@'localhost' IDENTIFIED BY 'password';
mysql> GRANT ALL PRIVILEGES ON urlshortener . * TO 'url'@'localhost';
mysql> CREATE TABLE `mappings` (
  `id` smallint(5) unsigned NOT NULL AUTO_INCREMENT,
  `original_url` varchar(300) NOT NULL,
  `shortened_url` varchar(100) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=19 DEFAULT CHARSET=utf8;
```

To make sure these commands worked and the user `url` has access to the database, exit the shell and re-enter as the user `url` using this command:

```
mysql -u url -p
```

When prompted for a password, enter the password we set - `password`.

At this point, you should have your MySQL database and user set up for Shawty to use.

## Repository


- Clone this repository:

```
mkdir -p ~/go/src/github.com/olin-fse
cd !$
git clone https://github.com/olin-fse/shawty
cd shawty
```

- Install dependencies from root directory:

```
go get ./...

cd frontend/
npm install
```

- Build React code:

```
cd frontend/
npm run build
cd ..
```

- Run server from root directory:

```
go run *.go
```

## Tests

Javascript:

```
cd frontend/
npm install
npm test
```

Go:

```
go get ./...
ginkgo
```
