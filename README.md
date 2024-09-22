# webserv

## Description

`webserv` is an HTTP/1.1 server written in C++98. It must be conditionally compliant with RFC 7230 to 7235.

## Usage

```shell
# Compile the sources
make
```

```shell
# Run the server
./webserv [options] [config_file]
```

#### Options
- `-h, --help`  : Display help text
- `-t, --test`  : Test the config file, dump it, and exit
- `-l, --log [LEVEL]`  : Set the log level (between 0 and 2)
- `-u, --uri`  : Keep location URI on routing (similar to nginx)

```shell
$ ./webserv -h

Usage: webserv [options] [config_file]

Options:
  -h, --help         : Display this help text
  -l, --log [LEVEL]  : Set log level (between 0 and 2)
  -t, --test         : Test config and exit
  -u, --uri          : Keep location URI on routing (similar to nginx)
```

## Configuration File

### Directives

#### `workers number;`

Sets the number of worker threads.

#### `location [ = | ~ | ~* | ^~ ] uri;`

Sets configuration depending on the given URI.

#### `listen address[:port];` or `listen port;`

Binds the given address to the port. If no address is given, binds to `0.0.0.0`. If no port is given, binds to `80`.

#### `server_name name ...;`

Sets names of a virtual server.

#### `root path;`

Sets the directory for requests.

#### `auth login:password;`

Restricts a route to a user.

#### `error_page code ... uri;`

Defines the URI that will be shown for the specified errors.

#### `upload directory;`

Defines a directory to upload files.

#### `autoindex on | off;` (default off)

Enables or disables the directory listing output.

#### `index file ...;`

Defines files that will be used as an index.

#### `cgi extension cgi_path;`

Defines a CGI binary that will be executed for the given extension.

#### `cgi-bin folder_path;` (default `./cgi-bin`)

Defines a folder where to search for CGI binaries.

### Example

```nginx
workers 4;

server {
  listen 80;

  root www;
  error_page 404 /my_errors/404.html;

  location / {
    index index.html;
  }
  
  location /data {
    cgi .php php-cgi;
  }

  location = /autoindex/ {
    root www/data;
    autoindex on;
  }
  
  location /my_errors {
    root www/errors;
  }
}
```
