#!/usr/bin/python
# Copyright 2016 Google Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

import os
import pexpect
import readline
import signal
import subprocess
import sys
import termios
import fcntl
import struct
import glob

BUFSIZE = 1024

def sigwinch(s, sig, data):
  t = struct.pack("HHHH", 0, 0, 0, 0)
  a = struct.unpack('hhhh', fcntl.ioctl(sys.stdout.fileno(),
                                        termios.TIOCGWINSZ, t))
  s.setwinsize(a[0], a[1])

def write_file(s, f):
  while True:
    d = f.read(BUFSIZE)
    if len(d) == 0:
      break
    s.write(d)

def doit(s, opt):
  if opt == 'u':
    l = raw_input('> Local file name: ')
    args = ["sz"]
    args.extend(glob.glob(l))
    p = subprocess.Popen(args, stdin=s, stdout=s)
    p.wait()
    s.write("\r")
  elif opt == 'd':
    p = subprocess.Popen(["rz","-R"], stdin=s, stdout=s)
    p.wait()
    s.write("\r")
  elif opt == 't':
    l = raw_input('> Local file name: ')
    with open(l) as f:
      write_file(s, f)
  else:
    print 'unknown option %s not implemented' % opt


def completer(text, n):
  fs = [x for x in os.listdir(".") if x.startswith(text)]
  ret = None
  if len(fs) > n:
    ret = fs[n]
  return ret

def main():
  s = pexpect.spawn('/usr/bin/ssh', sys.argv[1:])
  sigwinch(s, 0, None)
  signal.signal(signal.SIGWINCH, lambda sig, data: sigwinch(s, sig, data))
  readline.parse_and_bind("tab: complete")
  readline.set_completer(completer)
  while True:
    s.interact()
    if not s.isalive():
      break
    print
    opt = raw_input("> [u]pload/[d]ownload/[t]ype: ")
    if opt in ('u', 'd', 't'):
      doit(s, opt)

if __name__ == '__main__':
  main()
