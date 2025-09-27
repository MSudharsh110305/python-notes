> [!note]  
> Logging is the standard way to record events, errors, and diagnostics in Python applications and libraries using the standard library’s logging module.

## Why logging (not print)
- Logging adds levels, timestamps, module names, stack traces, and routing to files/streams/remote systems, unlike print which only emits plain text.
- Logging can be configured globally, filtered, disabled, or redirected without changing call sites, supporting production‑grade observability.

## Core concepts

- Logger: the interface used by code to emit log records; retrieved by name with getLogger(name).
- LogRecord: a structured event created when logging; contains message, level, time, origin, exception info, and contextual attributes.
- Handler: routes LogRecord to an output (console, file, rotating file, socket, HTTP, etc.).
- Formatter: turns LogRecord into a formatted string with a chosen style and fields.
- Filter: allows accepting/rejecting or mutating LogRecord before handlers process it, including by context.

## Quick start
```python
import logging

# Minimal configuration: defaults to WARNING and above, to stderr
logging.warning("This is a warning")   # Shown
logging.info("This is info")           # Hidden by default

```
Outcome: WARNING and higher go to stderr; INFO/DEBUG are filtered out by default threshold.

> [!note]  
> Idiomatic usage in libraries/apps creates module‑level loggers: logger = logging.getLogger(**name**).

## Log levels and when to use

- CRITICAL: system unusable, immediate action required.
- ERROR: operation failed, exception or unrecoverable step.
- WARNING: unexpected situation but continued operation.
- INFO: high‑level application flow and milestones.
- DEBUG: detailed diagnostic information for developers.
- NOTSET: special value indicating no explicit level; inherits effective level.
```python
logger = logging.getLogger(__name__)
logger.setLevel(logging.DEBUG)  # Set logger threshold
logger.debug("Debug details")
logger.info("High-level event")
logger.warning("Something odd")
logger.error("Operation failed")
logger.critical("System down")

```
Each call emits a LogRecord at the corresponding severity that must pass both logger and handlers’ thresholds to appear.

## Logger hierarchy and propagation

- Names are dot‑separated: "pkg", "pkg.module", "pkg.module.sub"; child loggers propagate to parents by default.
- Effective level is resolved by walking up the hierarchy until a level is found.
```python
root = logging.getLogger()          # root logger
app  = logging.getLogger("app")     # child of root
web  = logging.getLogger("app.web") # child of "app"

app.setLevel(logging.INFO)          # web inherits unless set explicitly
web.propagate = True                # default True; set False to stop bubbling

```
Propagation lets central handlers (e.g., on root) capture messages from all modules.

## Handlers overview

Common handlers:

- StreamHandler: write to streams like sys.stderr (default).
- FileHandler: write to a file.
- RotatingFileHandler: rotate by size with maxBytes and backupCount.
- TimedRotatingFileHandler: rotate by time intervals (e.g., 'midnight', 'S', 'M', 'H', 'D', 'W0-6').
- NullHandler: ignore records (used by libraries to avoid “No handler” warnings).
- MemoryHandler: buffer records and flush on trigger; can target another handler.
- QueueHandler / QueueListener: decouple logging emission from handling using queues for thread/process safety.
- SocketHandler/DatagramHandler/HTTPHandler/SMTPHandler: send logs over network transports.
```python
import logging, logging.handlers

logger = logging.getLogger("demo")
logger.setLevel(logging.DEBUG)

# Console
ch = logging.StreamHandler()
ch.setLevel(logging.INFO)

# Rotating file
fh = logging.handlers.RotatingFileHandler(
    "app.log", maxBytes=2_000_000, backupCount=5
)
fh.setLevel(logging.DEBUG)

# Formatter
fmt = logging.Formatter(
    "%(asctime)s %(name)s %(levelname)s %(message)s"
)
ch.setFormatter(fmt)
fh.setFormatter(fmt)

logger.addHandler(ch)
logger.addHandler(fh)

```
Here, INFO+ goes to console, DEBUG+ goes to file with size rotation.

---

## Formatters and format styles

- Default format is message‑only: "%(message)s".
    
- Formatter accepts format string and datefmt, plus style: "%" (default), "{" for str.format, or "$" for string.Template.
```python
# %-style
fmt_percent = logging.Formatter(
    "%(asctime)s | %(levelname)s | %(name)s | %(message)s",
    datefmt="%Y-%m-%d %H:%M:%S",
)

# { }-style
fmt_brace = logging.Formatter(
    "{asctime} | {levelname:8s} | {name} | {message}",
    datefmt="%H:%M:%S",
    style="{",
)

# $-style
fmt_template = logging.Formatter(
    "$asctime - $levelname - $message", style="$"
)

```
Choose fields from LogRecord attributes such as asctime, name, levelname, message, filename, lineno, funcName, process, thread, etc.

---

## Filters (accept or enrich)

- Filters can allow/deny records or add contextual data.
- A filter object has a filter(record) -> bool method; can be attached to logger or handler.
```python
class OnlyPayments(logging.Filter):
    def filter(self, record):
        return record.name.startswith("app.payments")

logger = logging.getLogger("app")
h = logging.StreamHandler()
h.addFilter(OnlyPayments())
logger.addHandler(h)

```
This emits only records from "app.payments" subtree through this handler.

---

## BasicConfig and root logger

- basicConfig() configures the root logger once; subsequent calls are ignored unless force=True in 3.8+.
- It sets level, adds a handler (default StreamHandler), format, datefmt, etc.
```python
import logging
logging.basicConfig(
    level=logging.INFO,
    format="%(levelname)s:%(name)s:%(message)s"
)
logging.info("Hello via root")

```
Use only in applications’ entry points, not libraries.

--- 

## Library best practices

- In libraries, never configure global logging or add handlers; instead, add NullHandler to avoid “No handlers could be found” warnings.
- Name logger with **name** to follow hierarchy and avoid collisions.
```python
# In library __init__.py
import logging
logging.getLogger(__name__).addHandler(logging.NullHandler())

```
This keeps control with the application integrating the library.

---

## Exceptions and tracebacks

- Use logger.exception("msg") inside an except block to include traceback automatically.
- Alternatively, pass exc_info=True with any logging call to include current exception context.
```python
try:
    1/0
except ZeroDivisionError:
    logger.exception("Division failed")

```
Produces stack trace and message at ERROR level.

---

## Structured context (extra and adapters)

- Provide extra contextual fields via extra={"key": value}; accessed in Formatter as %(key)s if present.
- Use LoggerAdapter to consistently add contextual information.
```python
adapter = logging.LoggerAdapter(logger, extra={"tenant": "acme"})
adapter.info("Tenant event")  # Can format %(tenant)s in formatter
```
This enables per‑tenant or request identifiers across logs.

---

## dictConfig and fileConfig (central config)

- dictConfig: configure loggers, handlers, formatters, and filters from a dictionary; supports incremental updates.
- fileConfig: configure via INI‑style file; less flexible than dictConfig.
```python
import logging.config

LOGGING = {
  "version": 1,
  "disable_existing_loggers": False,
  "formatters": {
    "standard": {
      "format": "%(asctime)s %(levelname)s %(name)s %(message)s"
    }
  },
  "filters": {},
  "handlers": {
    "console": {
      "class": "logging.StreamHandler",
      "level": "INFO",
      "formatter": "standard",
      "stream": "ext://sys.stderr"
    },
    "file": {
      "class": "logging.handlers.TimedRotatingFileHandler",
      "level": "DEBUG",
      "formatter": "standard",
      "filename": "app.log",
      "when": "midnight",
      "backupCount": 7,
      "encoding": "utf-8"
    }
  },
  "loggers": {
    "app": {
      "level": "DEBUG",
      "handlers": ["console", "file"],
      "propagate": False
    }
  },
  "root": {
    "level": "WARNING",
    "handlers": ["console"]
  }
}

logging.config.dictConfig(LOGGING)

```
This is the preferred way to ship production configurations.

---

## Advanced handlers and patterns

- QueueHandler/QueueListener: move I/O off main threads; essential for high‑throughput or multiprocessing.
- MemoryHandler: buffer and flush on level or capacity; wrap a target handler to batch writes.
- SMTPHandler: email errors; set secure/auth arguments when needed.
- HTTPHandler: send to HTTP server; consider JSON encoding and async alternatives.
- SysLogHandler/NTEventLogHandler: integrate with OS logging facilities.
- WatchedFileHandler: reopen file if it changes (useful with logrotate on Unix).
```python
from logging.handlers import QueueHandler, QueueListener
from queue import Queue
q = Queue(-1)
qh = QueueHandler(q)
logger.addHandler(qh)

console = logging.StreamHandler()
file = logging.FileHandler("app.log")
listener = QueueListener(q, console, file)
listener.start()

```
Queue decoupling ensures logging doesn’t block critical paths.

---

## Logging Cookbook highlights

- Custom handler/formatter/filter classes for domain needs.
- Using dictConfig with objects via import strings for factory functions.
- Avoid multiple‑process file contention: use SocketHandler/Queue + a single listener process or a 3rd‑party aggregator.
- Using filters or adapters to inject contextual data like request IDs.
```python
# Multiprocess pattern: workers -> SocketHandler -> listener process with handlers
# See cookbook recipes for end-to-end example.
```
This avoids corruption of single log files from concurrent processes.

---

## Capturing warnings and third‑party logs

- Capture Python warnings into logging: logging.captureWarnings(True).
- Third‑party modules logging flows into root unless filtered; control via named loggers or disable_existing_loggers in dictConfig.
```python
import logging, warnings
logging.captureWarnings(True)
warnings.warn("Deprecated API")
```
Warnings become LogRecords under "py.warnings" logger.

---

## Formatting fields (reference)

Common fields:

- %(message)s: message after %-formatting with args.
- %(asctime)s: time formatted with datefmt.
- %(name)s: logger name.
- %(levelname)s/%(levelno)s: level info.
- %(pathname)s, %(filename)s, %(module)s, %(funcName)s, %(lineno)d: origin.
- %(process)d/%(processName)s; %(thread)d/%(threadName)s.
- Custom fields provided via extra dict or adapters.
```python
fmt = "%(asctime)s [%(process)d] %(threadName)s %(levelname)s %(name)s:%(lineno)d %(message)s"
```