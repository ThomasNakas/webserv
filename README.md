# webserv
![Screenshot 2025-07-07 at 5 10 51 PM](https://github.com/user-attachments/assets/0a25aca9-c414-4f8b-9a3e-ad108ab61006)

**webserv** is a lightweight, high-performance HTTP web server implemented in C++. It aims to replicate essential features of modern web servers such as NGINX. It is the three team project of the 42 core curriculum and we worked on it two people with [Thalia](https://github.com/tsimitop).

## Features

- HTTP/1.1 support
- Static file serving
- CGI execution (Python)
- Configurable via a custom config file
- Basic error handling (404, 500, etc.)
- Support for multiple client non blocking connections using `poll()`
- Customizable routing(server, update, default_error_page) and server blocks
- siege stress test 100%-99.9% with:
  - 6 running servers
  - 100 clients / server
  - the server is conigured from one computer and tested by a different one
  - with non empty files

## Getting Started

### Prerequisites

- C++11 or later
- Unix-based OS (Linux or macOS)
- Make

### Building

```bash
git clone https://github.com/tsimitop/webserv.git
cd webserv
make
```
You can build it in docker if you prefer by using the following.
```bash
make docker_run
```


## Run the server
```bash
./webserv [conf_filename.conf]
```
- If a config file is not provided the default will be used. Please provide only filename, only the config directory will be searched.
- Navigate to http://localhost:your_port (our default ports are 4242, 4243, 4244, 4245, 4246, 4247). If you wish to change them and are working with Docker make sure to update the Makefile.

- Our index provide some correct and faulty cgis to test.
- We have multiple www directories with various uploads folders that can be configured differently.
- Play around with:
  - allowed/forbidden methods
  - large/small text uploads
  - deletions (to delete specify deleting directory i.e.->uploads2/file_to_delete.txt)
  - click on cgis
  - checkout the redirections of the config file
- Multiple problematic configs are provided and can be tested with the testing.sh found in /utils.
- Have fun with our webserver!
