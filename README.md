FuncServer
==========

[![Build Status](https://travis-ci.org/deep-compute/funcserver.svg?branch=master)](https://travis-ci.org/deep-compute/funcserver)
[![PyPI version](https://badge.fury.io/py/funcserver.svg)](https://badge.fury.io/py/funcserver)

Simple and opiniated way to build APIs in Python.

FuncServer An abstraction to implement web accessible servers hosting any sort of functionality. This is built on a Tornado core and supports interacting with the server using a web based python terminal making debugging and maintenance easy. In addition the logs emitted by the process can be viewed from within the web interface.

![Image](./calcserver.png?raw=true)

## Installation
``` bash
pip install funcserver
```

## Usage

### Basic example

The following is the code to implement the most basic Functionality Server.

``` python
from funcserver import Server

if __name__ == '__main__':
    Server().start()
```

Run it by doing

``` bash
python example.py run --debug
```

This server is now started and listening on default port 9345 for commands. You can interact with it using the Web UI by visiting http://localhost:9345/console

If you want to start it on a different port, do

``` bash
python example.py run --port <port no>
```

### Things to do in the Console

> The Web UI console and logs are enabled only when the `--debug` option is provided.

``` python
# see the objects available in the console env
>>> dir()

# write a message to log (open the log tab in a new browser window
# to see the logged message being echoed back). you can use the log
# tab to observe all the logs being written by the application.
>>> server.log.warning('something is happening')

# set a different log level
>>> server.log.setLevel(logging.DEBUG)

# you can import any python module here
>>> import datetime
```

### Calculation server (another example)

You will find an example script in examples/ called `calc__server.py`.

``` bash
python examples/calc_server.py
```

To use the server's functionality, run the provided example client script in examples/ directory.

``` bash
python examples/calc_client.py
```

### Debugging using PDB

When it is required to debug the API code using the Python debugger you may have to trigger the API function from the web based python console. However due to the design of FuncServer PDB does not work well in the scenario (as a result of the output being captured by the python interpretation part of FuncServer). To work around this issue a facility has been provided in the form of the "call" utility function available in the python console namespace. The usage is show below.

Let us assume that you have pdb trace set in code as follows:
``` python
def some_api_fn(self, a, b):
    import pdb; pdb.set_trace()
    c = a + b
    return c
```

If you call this api function as follows then debugging will not work and the api call will block from the console.
``` python
>>> api.some_api_fn(10, 20)
```

Instead do this:
``` python
>>> call(lambda: api.some_api_fn(10, 20))
```

Now the pdb console will appear in the terminal where you started your server.
