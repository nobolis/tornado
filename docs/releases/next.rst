What's new in the next version of Tornado
=========================================

In progress
-----------

Highlights
~~~~~~~~~~

* The new async/await keywords in Python 3.5 are supported. In most cases,
  ``async def`` can be used in place of the ``@gen.coroutine`` decorator.
  Inside a function defined with ``async def``, use ``await`` instead of
  ``yield`` to wait on an asynchronous operation. Coroutines defined with
  async/await will be faster than those defined with ``@gen.coroutine`` and
  ``yield``, but do not support some features including `.Callback`/`.Wait` or
  the ability to yield a Twisted ``Deferred``.
* The async/await keywords are also available when compiling with Cython in
  older versions of Python, as long as the ``backports_abc`` package is
  installed.

`tornado.auth`
~~~~~~~~~~~~~~

* New method `.OAuth2Mixin.oauth2_request` can be used to make authenticated
  requests with an access token.
* Now compatible with callbacks that have been compiled with Cython.

`tornado.autoreload`
~~~~~~~~~~~~~~~~~~~~

* Fixed an issue with the autoreload command-line wrapper in which
  imports would be incorrectly interpreted as relative.

`tornado.curl_httpclient`
~~~~~~~~~~~~~~~~~~~~~~~~~

* Fixed parsing of multi-line headers.

`tornado.gen`
~~~~~~~~~~~~~

* `.WaitIterator` now supports the ``async for`` statement on Python 3.5.
* ``@gen.coroutine`` can be applied to functions compiled with Cython.
  On python versions prior to 3.5, the ``backports_abc`` package must
  be installed for this functionality.
* ``Multi`` and `.multi_future` are deprecated and replaced by
  a unified function `.multi`.

`tornado.httpclient`
~~~~~~~~~~~~~~~~~~~~

* `tornado.httpclient.HTTPError` is now copyable with the `copy` module.

`tornado.httputil`
~~~~~~~~~~~~~~~~~~

* `.HTTPHeaders` can now be pickled and unpickled.

`tornado.ioloop`
~~~~~~~~~~~~~~~~

* ``IOLoop(make_current=True)`` now works as intended instead
  of raising an exception.
* The Twisted and asyncio IOLoop implementations now clear
  ``current()`` when they exit, like the standard IOLoops.
* `.IOLoop.add_callback` is faster in the single-threaded case.
* `.IOLoop.add_callback` no longer raises an error when called on
  a closed IOLoop, but the callback will not be invoked.

`tornado.iostream`
~~~~~~~~~~~~~~~~~~

* Coroutine-style usage of `.IOStream` now converts most errors into
  `.StreamClosedError`, which has the effect of reducing log noise from
  exceptions that are outside the application's control (especially
  SSL errors).
* `.StreamClosedError` now has a ``real_error`` attribute which indicates
  why the stream was closed. It is the same as the ``error`` attribute of
  `.IOStream` but may be more easily accessible than the `.IOStream` itself.
* Improved error handling in `~.BaseIOStream.read_until_close`.
* Logging is less noisy when an SSL server is port scanned.
* ``EINTR`` is now handled on all reads.

`tornado.locale`
~~~~~~~~~~~~~~~~

* `tornado.locale.load_translations` now accepts encodings other than
  UTF-8. UTF-16 and UTF-8 will be detected automatically if a BOM is
  present; for other encodings `.load_translations` has an ``encoding``
  parameter.

`tornado.locks`
~~~~~~~~~~~~~~~

* `.Lock` and `.Semaphore` now support the ``async with`` statement on
  Python 3.5.

`tornado.log`
~~~~~~~~~~~~~

* A new time-based log rotation mode is available with
  ``--log_rotate_mode=time``, ``--log-rotate-when``, and
  ``log-rotate-interval``.

`tornado.netutil`
~~~~~~~~~~~~~~~~~

* `.bind_sockets` now supports ``SO_REUSEPORT`` with the ``reuse_port=True``
  argument.

`tornado.options`
~~~~~~~~~~~~~~~~~

* Dashes and underscores are now fully interchangeable in option names.

`tornado.queues`
~~~~~~~~~~~~~~~~

* `.Queue` now supports the ``async with`` statement on Python 3.5.

`tornado.simple_httpclient`
~~~~~~~~~~~~~~~~~~~~~~~~~~~

* When following redirects, ``streaming_callback`` and
  ``header_callback`` will no longer be run on the redirect responses
  (only the final non-redirect).

`tornado.template`
~~~~~~~~~~~~~~~~~~

* `tornado.template.ParseError` now includes the filename in addition to
  line number.
* Whitespace handling has become more configurable. The `.Loader`
  constructor now has a ``whitespace`` argument, there is a new
  ``template_whitespace`` `.Application` setting, and there is a new
  ``{% whitespace %}`` template directive. All of these options take
  a mode name defined in the `tornado.template.filter_whitespace` function.
  The default mode is ``single``, which is the same behavior as prior
  versions of Tornado.
* Non-ASCII filenames are now supported.

`tornado.testing`
~~~~~~~~~~~~~~~~~

* `.ExpectLog` objects now have a boolean ``logged_stack`` attribute to
  make it easier to test whether an exception stack trace was logged.

`tornado.web`
~~~~~~~~~~~~~

* The hard limit of 4000 bytes per outgoing header has been removed.
* `.StaticFileHandler` returns the correct ``Content-Type`` for files
  with ``.gz``, ``.bz2``, and ``.xz`` extensions.
* Responses smaller than 1000 bytes will no longer be compressed.
* The default gzip compression level is now 6 (was 9).
* Fixed a regression in Tornado 4.2.1 that broke `.StaticFileHandler`
  with a ``path`` of ``/``.
* `tornado.web.HTTPError` is now copyable with the `copy` module.
* The exception `.Finish` now accepts an argument which will be passed to
  the method `.RequestHandler.finish`.

`tornado.websocket`
~~~~~~~~~~~~~~~~~~~

* Fixed handling of continuation frames when compression is enabled.
