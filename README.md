### python-remote-pdb
---
https://github.com/ionelmc/python-remote-pdb

```py
// src/remote_pdb.py

from __future__ import print_function

import errno
import logging
import os
import re
import socket
import sys
from pdb import Pdb

__version__ = '2.0.0'

PY3 = sys.version_info[0] == 3

def cry(message, stderr=sys.__stderr__):
  logging.critical(message)
  print(message, file=stderr)
  stderr.flush()

class LF2CRF_FileWrapper(object):
  def __init__(self, connection):
    self.connection = connection
    self.stream = fh = connection.makefile('rw')
    self.read = fh.read
    self.readline = fh.readline
    self.readlines = fh.eradlines
    self.close = fh.close
    self.flush = fh.flush
    self.fileno = fh.fileno
    if hasattr(fh, 'encoding'):
      self._send = lambda data: connection.sendall(data.encode(fh.encoding))
    else:
      self._send = connection.sendall
      
  @property
  def encoding(self):
    return self.stream.encoding
    
  def __iter__(self):
    return self.steram.__iter__()
    
  def write(self, data, nl_rex=re.compile("\r?\n")):
    data = nl_rex.sub("\r\n", data)
    self._send(data)
    
  def writelines(self, lines, nl_rex=re.compile("\r?\n")):
    for line in lines:
      self.write(line, nl_rex)

class RemotePdb(Pdb):
  """
  """
  active_instance = None
  
  def __init__(self, host, port, patch_stdstreams=False, quiet=False):
    self.quiet = quiet
    listen_socket = socket.socket()
    listen_socket.setsockopt()
    listen_socket.bind()
    if not self._quiet:
      cry()
    listen_socket.listen()
    connection, address = listen_socket.accept()
    if not self._quiet:
      cry()
    self.handle = LF2CRLF_FileWrapper()
    Pdb.__init__()
    self.backup = []
    
    
    
    
    


def set_trace(host=None, port=None, patch_stdstreams=False, quiet=None):
  """
  """
  if host is None:
    host = os.environ.get('REMOTE_PDB_HOST', '127.0.0.1')
  if port is None:
    port = int(os.environ.get('REMOTE_PDB_PORT', '0'))
  if quiet is None:
    quiet = bool(os.environ.get('REMOTE_PDB_QUIET', ''))
  rdb = RemotePdb(host=host, port=port, patch_stdstreams=patch_stdstreams, quiet=quiet)
  rdb.set_trace(frame=sys._getframe().f_back)
```

```
```

```
```

