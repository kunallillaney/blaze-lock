# blaze-lock

[![](https://img.shields.io/pypi/v/blazelock.svg)](https://pypi.python.org/pypi/blazelock)
[![Hex.pm](https://img.shields.io/hexpm/l/plug.svg)](http://www.apache.org/licenses/LICENSE-2.0.html)

**blaze-lock** is a python module which implements areaders-writer lock using Redis. 

### Installation

```console
pip install blazelock
```

### Implementation
```pseudo
acquire_read_lock:
  mutex_lock_acquire and not writer_waiting
  reader_count ++
  mutex_lock_release

release_read_lock:
  mutex_lock_acquire
  reader_count --
  if reader_count == 0:
    notify_all
  mutex_lock_release

acquire_writer_lock:
  mutex_lock_acquire
  while reader_count > 0:
    writer_waiting = true
    wait

release_writer_lock:
  writer_waiting = false
  mutex_lock_release

wait:
  mutex_lock_release
  wake_signal
  mutex_lock_acquire
```
