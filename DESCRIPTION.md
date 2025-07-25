This package includes the Snowflake Connector for Python, which conforms to the Python DB API 2.0 specification:
https://www.python.org/dev/peps/pep-0249/

Snowflake Documentation is available at:
https://docs.snowflake.com/

Source code is also available at: https://github.com/snowflakedb/snowflake-connector-python

# Release Notes
- v3.16.1(TBD)
  - Added in-band OCSP exception telemetry.
  - Added `APPLICATION_PATH` within `CLIENT_ENVIRONMENT` to distinguish between multiple scripts using the PythonConnector in the same environment.
  - Disabled token caching for OAuth Client Credentials authentication
  - Added in-band HTTP exception telemetry.
  - Fixed a bug where timezoned timestamps fetched as pandas.DataFrame or pyarrow.Table would overflow for the sake of unnecessary precision. In the case where an overflow cannot be prevented a clear error will be raised now.
  - Fix OAuth authenticator values.

- v3.16.0(July 04,2025)
  - Bumped numpy dependency from <2.1.0 to <=2.2.4.
  - Added Windows support for Python 3.13.
  - Added `bulk_upload_chunks` parameter to `write_pandas` function. Setting this parameter to True changes the behaviour of write_pandas function to first write all the data chunks to the local disk and then perform the wildcard upload of the chunks folder to the stage. In default behaviour the chunks are being saved, uploaded and deleted one by one.
  - Added support for new authentication mechanism PAT with external session ID.
  - Added `client_fetch_use_mp` parameter that enables multiprocessed fetching of result batches.
  - Added basic arrow support for Interval types.
  - Fixed `write_pandas` special characters usage in the location name.
  - Fixed usage of `use_virtual_url` when building the location for gcs storage client.
  - Added support for Snowflake OAuth for local applications.

- v3.15.0(Apr 29,2025)
  - Bumped up min boto and botocore version to 1.24.
  - OCSP: terminate certificates chain traversal if a trusted certificate already reached.
  - Added new authentication methods support for programmatic access tokens (PATs), OAuth 2.0 Authorization Code Flow, OAuth 2.0 Client Credentials Flow, and OAuth Token caching.
    - For OAuth 2.0 Authorization Code Flow:
      - Added the `oauth_client_id`, `oauth_client_secret`, `oauth_authorization_url`, `oauth_token_request_url`, `oauth_redirect_uri`, `oauth_scope`, `oauth_disable_pkce`, `oauth_enable_refresh_tokens` and `oauth_enable_single_use_refresh_tokens` parameters.
      - Added the `OAUTH_AUTHORIZATION_CODE` value for the parameter authenticator.
    - For OAuth 2.0 Client Credentials Flow:
      - Added the `oauth_client_id`, `oauth_client_secret`, `oauth_token_request_url`, and `oauth_scope` parameters.
      - Added the `OAUTH_CLIENT_CREDENTIALS` value for the parameter authenticator.
    - For OAuth Token caching: Passing a username to driver configuration is required, and the `client_store_temporary_credential property` is to be set to `true`.

- v3.14.1(April 21, 2025)
  - Added support for Python 3.13.
    - NOTE: Windows 64 support is still experimental and should not yet be used for production environments.
  - Dropped support for Python 3.8.
  - Added basic decimal floating-point type support.
  - Added experimental authentication methods.
  - Added support of GCS regional endpoints.
  - Added support of GCS virtual urls. See more: https://cloud.google.com/storage/docs/request-endpoints#xml-api
  - Added `client_fetch_threads` experimental parameter to better utilize threads for fetching query results.
  - Added `check_arrow_conversion_error_on_every_column` connection property that can be set to `False` to restore previous behaviour in which driver will ignore errors until it occurs in the last column. This flag's purpose is to unblock workflows that may be impacted by the bugfix and will be removed in later releases.
  - Lowered log levels from info to debug for some of the messages to make the output easier to follow.
  - Allowed the connector to inherit a UUID4 generated upstream, provided in statement parameters (field: `requestId`), rather than automatically generate a UUID4 to use for the HTTP Request ID.
  - Improved logging in urllib3, boto3, botocore - assured data masking even after migration to the external owned library in the future.
  - Improved error message for client-side query cancellations due to timeouts.
  - Improved security and robustness for the temporary credentials cache storage.
  - Fixed a bug that caused driver to fail silently on `TO_DATE` arrow to python conversion when invalid date was followed by the correct one.
  - Fixed expired S3 credentials update and increment retry when expired credentials are found.
  - Deprecated `insecure_mode` connection property and replaced it with `disable_ocsp_checks` with the same behavior as the former property.

- v3.14.0(March 03, 2025)
  - Bumped pyOpenSSL dependency upper boundary from <25.0.0 to <26.0.0.
  - Added a <19.0.0 pin to pyarrow as a workaround to a bug affecting Azure Batch.
  - Optimized distribution package lookup to speed up import.
  - Fixed a bug where privatelink OCSP Cache url could not be determined if privatelink account name was specified in uppercase.
  - Added support for iceberg tables to `write_pandas`.
  - Fixed base64 encoded private key tests.
  - Fixed a bug where file permission check happened on Windows.
  - Added support for File types.
  - Added `unsafe_file_write` connection parameter that restores the previous behaviour of saving files downloaded with GET with 644 permissions.

- v3.13.2(January 29, 2025)
  - Changed not to use scoped temporary objects.

- v3.13.1(January 29, 2025)
  - Remedied SQL injection vulnerability in snowflake.connector.pandas_tools.write_pandas. See more https://github.com/snowflakedb/snowflake-connector-python/security/advisories/GHSA-2vpq-fh52-j3wv
  - Remedied vulnerability in deserialization of the OCSP response cache. See more: https://github.com/snowflakedb/snowflake-connector-python/security/advisories/GHSA-m4f6-vcj4-w5mx
  - Remedied vulnerability connected to cache files permissions. See more: https://github.com/snowflakedb/snowflake-connector-python/security/advisories/GHSA-r2x6-cjg7-8r43

- v3.13.0(January 23,2025)
  - Added a feature to limit the sizes of IO-bound ThreadPoolExecutors during PUT and GET commands.
  - Updated README.md to include instructions on how to verify package signatures using `cosign`.
  - Updated the log level for cursor's chunk rowcount from INFO to DEBUG.
  - Added a feature to verify if the connection is still good enough to send queries over.
  - Added support for base64-encoded DER private key strings in the `private_key` authentication type.

- v3.12.4(December 3,2024)
  - Fixed a bug where multipart uploads to Azure would be missing their MD5 hashes.
  - Fixed a bug where OpenTelemetry header injection would sometimes cause Exceptions to be thrown.
  - Fixed a bug where OCSP checks would throw TypeError and make mainly GCP blob storage unreachable.
  - Bumped pyOpenSSL dependency from >=16.2.0,<25.0.0 to >=22.0.0,<25.0.0.

- v3.12.3(October 25,2024)
  - Improved the error message for SSL-related issues to provide clearer guidance when an SSL error occurs.
  - Improved error message for SQL execution cancellations caused by timeout.

- v3.12.2(September 11,2024)
  - Improved error handling for asynchronous queries, providing more detailed and informative error messages when an async query fails.
  - Improved inference of top-level domains for accounts specifying a region in China, now defaulting to snowflakecomputing.cn.
  - Improved implementation of the `snowflake.connector.util_text.random_string` to reduce the likelihood of collisions.
  - Updated the log level for OCSP fail-open warning messages from ERROR to WARNING.

- v3.12.1(August 20,2024)
  - Fixed a bug that logged the session token when renewing a session.
  - Fixed a bug where disabling client telemetry did not work.
  - Fixed a bug where passing `login_timeout` as a string raised a `TypeError` during the login retry step.
  - Use `pathlib` instead of `os` for default config file location resolution.
  - Removed upper `cryptogaphy` version pin.
  - Removed reference to script `snowflake-export-certs` (its backing module was already removed long ago)
  - Enhanced retry mechanism for handling transient network failures during query result polling when no server response is received.

- v3.12.0(July 24,2024)
  - Set default connection timeout of 10 seconds and socket read timeout of 10 minutes for HTTP calls in file transfer.
  - Optimized `to_pandas()` performance by fully parallel downloading logic.
  - Fixed a bug that specifying client_session_keep_alive_heartbeat_frequency in snowflake-sqlalchemy could crash the connector.
  - Fixed incorrect type hint of connection parameter `private_key`.
  - Added support for connectivity to multiple domains.
  - Bumped keyring dependency from >=23.1.0,<25.0.0 to >=23.1.0,<26.0.0.
  - Disabled OOB Telemetry.

- v3.11.0(June 17,2024)
  - Added support for `token_file_path` connection parameter to read an OAuth token from a file when connecting to Snowflake.
  - Added support for `debug_arrow_chunk` connection parameter to allow debugging raw arrow data in case of arrow data parsing failure.
  - Added support for `disable_saml_url_check` connection parameter to disable SAML URL check in OKTA authentication.
  - Fixed a bug that OCSP certificate signed using SHA384 algorithm cannot be verified.
  - Fixed a bug that status code shown as uploaded when PUT command failed with 400 error.
  - Fixed a bug that a PermissionError was raised when the current user does not have the right permission on parent directory of config file path.
  - Fixed a bug that OCSP GET url is not encoded correctly when it contains a slash.
  - Fixed a bug that an SSO URL didn't accept `:` in a query parameter, for instance, `https://sso.abc.com/idp/startSSO.ping?PartnerSpId=https://xyz.snowflakecomputing.com/`.

- v3.10.1(May 21, 2024)

  - Removed an incorrect error log message that could occur during arrow data conversion.

- v3.10.0(April 29,2024)

  - Added support for structured types to fetch_pandas_all.
  - Fixed an issue relating to incorrectly formed China S3 endpoints.

- v3.9.1(April 22,2024)

  - Fixed an issue that caused a HTTP 400 error when connecting to a China endpoint.

- v3.9.0(April 20,2024)

  - Added easy logging configuration so that users can easily generate log file by setup log config in `$SNOWFLAKE_HOME/config.toml`.
  - Improved s3 acceleration logic when connecting to China endpoint.

- v3.8.1(April 09, 2024)

  - Reverted the change "Updated `write_pandas` to skip TABLE IF NOT EXISTS in truncate mode." introduced in v3.8.0 (yanked) as it's a breaking change. `write_pandas` will be fixed in the future in a non-breaking way.

- v3.8.0(April 04,2024)

  - Improved `externalbrowser` auth in containerized environments
    - Instruct browser to not fetch `/favicon` on success page
    - Simple retry strategy on empty socket.recv
    - Add `SNOWFLAKE_AUTH_SOCKET_REUSE_PORT` flag (usage: `SNOWFLAKE_AUTH_SOCKET_REUSE_PORT=true`) to set the underlying socket's `SO_REUSEPORT` flag (described in the [socket man page](https://man7.org/linux/man-pages/man7/socket.7.html))
      - Useful when the randomized port used in the localhost callback url is being followed before the container engine completes port forwarding to host
      - Statically map a port between your host and container and allow that port to be reused in rapid succession with:
         `SF_AUTH_SOCKET_PORT=3037 SNOWFLAKE_AUTH_SOCKET_REUSE_PORT=true poetry run python somescript.py`
    - Add `SNOWFLAKE_AUTH_SOCKET_MSG_DONTWAIT` flag (usage: `SNOWFLAKE_AUTH_SOCKET_MSG_DONTWAIT=true`) to make a non-blocking socket.recv call and retry on Error
      - Consider using this if running in a containerized environment and externalbrowser auth frequently hangs while waiting for callback
      - NOTE: this has not been tested extensively, but has been shown to improve the experience when using WSL
  - Added support for parsing structured type information in schema queries.
  - Bumped platformdirs from >=2.6.0,<4.0.0 to >=2.6.0,<5.0.0
  - Updated diagnostics to use system$allowlist instead of system$whitelist.
  - Updated `write_pandas` to skip TABLE IF NOT EXISTS in truncate mode.
  - Improved cleanup logic for connection to rely on interpreter shutdown instead of the `__del__` method.
  - Updated the logging level from INFO to DEBUG when logging the executed query using `SnowflakeCursor.execute`.
  - Fixed a bug that the truncated password in log is not masked.

- v3.7.1(February 21, 2024)

  - Bumped pandas dependency from >=1.0.0,<2.2.0 to >=1.0.0,<3.0.0.
  - Bumped cryptography dependency from <42.0.0,>=3.1.0 to >=3.1.0,<43.0.0.
  - Bumped pyOpenSSL dependency from >=16.2.0,<24.0.0 to >=16.2.0,<25.0.0.
  - Fixed a memory leak in decimal data conversion.
  - Fixed a bug where `write_pandas` wasn't truncating the target table.
  - Bumped keyring dependency lower bound to 23.1.0 to address security vulnerability.

- v3.7.0(January 25,2024)

  - Added a new boolean parameter `force_return_table` to `SnowflakeCursor.fetch_arrow_all` to force returning `pyarrow.Table` in case of zero rows.
  - Cleanup some C++ code warnings and performance issues.
  - Added support for Python 3.12
  - Make local testing more robust against implicit assumptions.
  - Fixed PyArrow Table type hinting
  - Added support for connecting using an existing connection via the session and master token.
  - Added support for connecting to Snowflake by authenticating with multiple SAML IDP using external browser.
  - Added support for structured types (OBJECT, MAP, ARRAY) to nanoarrow converters.
  - Fixed compilation issue due to missing cstdint header on gcc13.
  - Improved config permissions warning message.

- v3.6.0(December 09,2023)

  - Added support for Vector types
  - Changed urllib3 version pin to only affect Python versions < 3.10.
  - Support for `private_key_file` and `private_key_file_pwd` connection parameters
  - Added a new flag `expired` to `SnowflakeConnection` class, that keeps track of whether the connection's master token has expired.
  - Fixed a bug where date insertion failed when date format is set and qmark style binding is used.

- v3.5.0(November 13,2023)

  - Version 3.5.0 is the snowflake-connector-python purely built upon apache arrow-nanoarrow project.
    - Reduced the wheel size to ~1MB and installation size to ~5MB.
    - Removed a hard dependency on a specific version of pyarrow.
  - Deprecated the usage of the following class/variable/environment variable for the sake of pure nanoarrow converter:
    - Deprecated class `snowflake.connector.cursor.NanoarrowUsage`.
    - Deprecated environment variable `NANOARROW_USAGE`.
    - Deprecated module variable `snowflake.connector.cursor.NANOARROW_USAGE`.

- v3.4.1(November 08,2023)

  - Bumped vendored `urllib3` to 1.26.18
  - Bumped vendored `requests` to 2.31.0

- v3.4.0(November 03,2023)

  - Added support for `use_logical_type` in `write_pandas`.
  - Removed dependencies on pycryptodomex and oscrypto. All connections now go through OpenSSL via the cryptography library, which was already a dependency.
  - Fixed issue with ingesting files over 80 GB to S3.
  - Added the `backoff_policy` argument to `snowflake.connector.connect` allowing for configurable backoff policy between retries of failed requests. See available implementations in the `backoff_policies` module.
  - Added the `socket_timeout` argument to `snowflake.connector.connect` specifying socket read and connect timeout.
  - Fixed `login_timeout` and `network_timeout` behaviour. Retries of login and network requests are now properly halted after these timeouts expire.
  - Fixed bug for issue https://github.com/urllib3/urllib3/issues/1878 in vendored `urllib`.
  - Add User-Agent header for diagnostic report for tracking.

- v3.3.1(October 16,2023)

  - Added for non-Windows platforms command suggestions (chown/chmod) for insufficient file permissions of config files.
  - Fixed issue with connection diagnostics failing to complete certificate checks.
  - Fixed issue that arrow iterator causes `ImportError` when the c extensions are not compiled.

- v3.3.0(October 10,2023)

  - Updated to Apache arrow-nanoarrow project for result arrow data conversion.
  - Introduced the `NANOARROW_USAGE` environment variable to allows switching between the nanoarrow converter and the arrow converter. Valid values include:
    - `FOLLOW_SESSION_PARAMETER`, which uses the converter configured in the server.
    - `DISABLE_NANOARROW`, which uses arrow converter, overriding the server setting.
    - `ENABLE_NANOARROW`, which uses the nanoarrow converter, overriding the server setting.
  - Introduced the `snowflake.connector.cursor.NanoarrowUsage` enum, whose members include:
    - `NanoarrowUsage.FOLLOW_SESSION_PARAMETER`, which uses the converter configured in the server.
    - `NanoarrowUsage.DISABLE_NANOARROW`, which uses arrow converter, overriding the server setting.
    - `NanoarrowUsage.ENABLE_NANOARROW`, which uses the nanoarrow converter, overriding the server setting.
  - Introduced the `snowflake.connector.cursor.NANOARROW_USAGE` module variable to allow switching between the nanoarrow converter and the arrow converter. It works in conjunction with the `snowflake.connector.cursor.NanoarrowUsage` enum.
  - The newly-introduced environment variable, enum, and module variable are temporary. They will be removed in a future release when switch from arrow to nanoarrow for data conversion is complete.

- v3.2.1(September 26,2023)

  - Fixed a bug where url port and path were ignored in private link oscp retry.
  - Added thread safety in telemetry when instantiating multiple connections concurrently.
  - Bumped platformdirs dependency from >=2.6.0,<3.9.0 to >=2.6.0,<4.0.0.0 and made necessary changes to allow this.
  - Removed the deprecation warning from the vendored urllib3 about urllib3.contrib.pyopenssl deprecation.
  - Improved robustness in handling authentication response.

- v3.2.0(September 06,2023)

  - Made the ``parser`` -> ``manager`` renaming more consistent in ``snowflake.connector.config_manager`` module.
  - Added support for default values for ConfigOptions
  - Added default_connection_name to config.toml file

- v3.1.1(August 28,2023)

  - Fixed a bug in retry logic for okta authentication to refresh token.
  - Support `RSAPublicKey` when constructing `AuthByKeyPair` in addition to raw bytes.
  - Fixed a bug when connecting through SOCKS5 proxy, the attribute `proxy_header` is missing on `SOCKSProxyManager`.
  - Cherry-picked https://github.com/urllib3/urllib3/commit/fd2759aa16b12b33298900c77d29b3813c6582de onto vendored urllib3 (v1.26.15) to enable enforce_content_length by default.
  - Fixed a bug in tag generation of OOB telemetry event.

- v3.1.0(July 31,2023)

  - Added a feature that lets you add connection definitions to the `connections.toml` configuration file. A connection definition refers to a collection of connection parameters, for example, if you wanted to define a connection named `prod``:

    ```toml
    [prod]
    account = "my_account"
    user = "my_user"
    password = "my_password"
    ```
    By default, we look for the `connections.toml` file in the location specified in the `SNOWFLAKE_HOME` environment variable (default: `~/.snowflake`). If this folder does not exist, the Python connector looks for the file in the [platformdirs](https://github.com/platformdirs/platformdirs/blob/main/README.rst) location, as follows:

    - On Linux: `~/.config/snowflake/`,  but follows XDG settings
    - On Mac: `~/Library/Application Support/snowflake/`
    - On Windows: `%USERPROFILE%\AppData\Local\snowflake\`

    You can determine which file is used by running the following command:

    ```
    python -c "from snowflake.connector.constants import CONNECTIONS_FILE; print(str(CONNECTIONS_FILE))"
    ```
  - Bumped cryptography dependency from <41.0.0,>=3.1.0 to >=3.1.0,<42.0.0.
  - Improved OCSP response caching to remove tmp cache files on Windows.
  - Improved OCSP response caching to reduce the times of disk writing.
  - Added a parameter `server_session_keep_alive` in `SnowflakeConnection` that skips session deletion when client connection closes.
  - Tightened our pinning of platformdirs, to prevent their new releases breaking us.
  - Fixed a bug where SFPlatformDirs would incorrectly append application_name/version to its path.
  - Added retry reason for queries that are retried by the client.
  - Fixed a bug where `write_pandas` fails when user does not have the privilege to create stage or file format in the target schema, but has the right privilege for the current schema.
  - Remove Python 3.7 support.
  - Worked around a segfault which sometimes occurred during cache serialization in multi-threaded scenarios.
  - Improved error handling of connection reset error.
  - Fixed a bug about deleting the temporary files happened when running PUT command.
  - Allowed to pass `type_mapper` to `fetch_pandas_batches()` and `fetch_pandas_all()`.
  - Fixed a bug where pickle.dump segfaults during cache serialization in multi-threaded scenarios.
  - Improved retry logic for okta authentication to refresh token if authentication gets throttled.
  - Note that this release does not include the changes introduced in the previous 3.1.0a1 release. Those will be released at a later time.

- v3.0.4(May 23,2023)
  - Fixed a bug in which `cursor.execute()` could modify the argument statement_params dictionary object when executing a multistatement query.
  - Added the json_result_force_utf8_decoding connection parameter to force decoding JSON content in utf-8 when the result format is JSON.
  - Fixed a bug in which we cannot call `SnowflakeCursor.nextset` before fetching the result of the first query if the cursor runs an async multistatement query.
  - Bumped vendored library urllib3 to 1.26.15
  - Bumped vendored library requests to 2.29.0
  - Fixed a bug when `_prefetch_hook()` was not called before yielding results of `execute_async()`.
  - Fixed a bug where some ResultMetadata fields were marked as required when they were optional.
  - Bumped pandas dependency from <1.6.0,>=1.0.0 to >=1.0.0,<2.1.0
  - Fixed a bug where bulk insert converts date incorrectly.
  - Add support for Geometry types.

- v3.0.3(April 20, 2023)
  - Fixed a bug that prints error in logs for GET command on GCS.
  - Added a parameter that allows users to skip file uploads to stage if file exists on stage and contents of the file match.
  - Fixed a bug that occurred when writing a Pandas DataFrame with non-default index in `snowflake.connector.pandas_tool.write_pandas`.
  - Fixed a bug that occurred when writing a Pandas DataFrame with column names containing double quotes in `snowflake.connector.pandas_tool.write_pandas`.
  - Fixed a bug that occurred when writing a Pandas DataFrame with binary data in `snowflake.connector.pandas_tool.write_pandas`.
  - Improved type hint of `SnowflakeCursor.execute` method.
  - Fail instantly upon receiving `403: Forbidden` HTTP response for a login-request.
  - Improved GET logging to warn when downloading multiple files with the same name.

- v3.0.2(March 23, 2023)

  - Fixed a memory leak in the logging module of the Cython extension.
  - Fixed a bug where the `put` command on AWS raised `AttributeError` when uploading file composed of multiple parts.
  - Fixed a bug of incorrect type hints of `SnowflakeCursor.fetch_arrow_all` and `SnowflakeCursor.fetchall`.
  - Fixed a bug where `snowflake.connector.util_text.split_statements` swallows the final line break in the case when there are no space between lines.
  - Improved logging to mask tokens in case of errors.
  - Validate SSO URL before opening it in the browser for External browser authenticator.

- v3.0.1(February 28, 2023)

  - Improved the robustness of OCSP response caching to handle errors in cases of serialization and deserialization.
  - Updated async_executes method's doc-string.
  - Errors raised now have a query field that contains the SQL query that caused them when available.
  - Fixed a bug where MFA token caching would refuse to work until restarted instead of reauthenticating.
  - Replaced the dependency on setuptools in favor of packaging.
  - Fixed a bug where `AuthByKeyPair.handle_timeout` should pass keyword arguments instead of positional arguments when calling `AuthByKeyPair.prepare`.

- v3.0.0(January 26, 2023)

  - Fixed a bug where write_pandas did not use user-specified schema and database to create intermediate objects
  - Fixed a bug where HTTP response code of 429 were not retried
  - Fixed a bug where MFA token caching was not working
  - Bumped pyarrow dependency from >=8.0.0,<8.1.0 to >=10.0.1,<10.1.0
  - Bumped pyOpenSSL dependency from <23.0.0 to <24.0.0
  - During browser-based authentication, the SSO url is now printed before opening it in the browser
  - Increased the level of a log for when ArrowResult cannot be imported
  - Added a minimum MacOS version check when compiling C-extensions
  - Enabled `fetch_arrow_all` and `fetch_arrow_batches` to handle async query results

- v2.9.0(December 9, 2022)

  - Fixed a bug where the permission of the file downloaded via GET command is changed
  - Reworked authentication internals to allow users to plug custom key-pair authenticators
  - Multi-statement query execution is now supported through `cursor.execute` and `cursor.executemany`
    - The Snowflake parameter `MULTI_STATEMENT_COUNT` can be altered at the account, session, or statement level. An additional argument, `num_statements`, can be provided to `execute` to use this parameter at the statement level. It *must* be provided to `executemany` to submit a multi-statement query through the method. Note that bulk insert optimizations available through `executemany` are not available when submitting multi-statement queries.
      - By default the parameter is 1, meaning only a single query can be submitted at a time
      - Set to 0 to submit any number of statements in a multi-statement query
      - Set to >1 to submit the specified exact number of statements in a multi-statement query
    - Bindings are accepted in the same way for multi-statements as they are for single statement queries
    - Asynchronous multi-statement query execution is supported. Users should still use `get_results_from_sfqid` to retrieve results
    - To access the results of each query, users can call `SnowflakeCursor.nextset()` as specified in the DB 2.0 API (PEP-249), to iterate through each statements results
      - The first statement's results are accessible immediately after calling `execute` (or `get_results_from_sfqid` if asynchronous) through the existing `fetch*()` methods

- v2.8.3(November 28,2022)

  - Bumped cryptography dependency from <39.0.0 to <41.0.0
  - Fixed a bug where expired OCSP response cache caused infinite recursion during cache loading

- v2.8.2(November 18,2022)

  - Improved performance of OCSP response caching
  - During the execution of GET commands we no longer resolve target location on the local machine
  - Improved performance of regexes used for PUT/GET SQL statement detection. CVE-2022-42965

- v2.8.1(October 30,2022)

   - Bumped cryptography dependency from <37.0.0 to <39.0.0
   - Bumped pandas dependency from <1.5.0 to <1.6.0
   - Fixed a bug where write_pandas wouldn't write an empty DataFrame to Snowflake
   - When closing connection async query status checking is now parallelized
   - Fixed a bug where test logging would be enabled on Jenkins workers in non-Snowflake Jenkins machines
   - Enhanced the atomicity of write_pandas when overwrite is set to True

- v2.8.0(September 27,2022)

  - Fixed a bug where rowcount was deleted when the cursor was closed
  - Fixed a bug where extTypeName was used even when it was empty
  - Updated how telemetry entries are constructed
  - Added telemetry for imported root packages during run-time
  - Added telemetry for using write_pandas
  - Fixed missing dtypes when calling fetch_pandas_all() on empty result
  - The write_pandas function now supports providing additional arguments to be used by DataFrame.to_parquet
  - All optional parameters of write_pandas can now be provided to pd_writer and make_pd_writer to be used with DataFrame.to_sql

- v2.7.12(August 26,2022)

   - Fixed a bug where timestamps fetched as pandas.DataFrame or pyarrow.Table would overflow for the sake of unnecessary precision. In the case where an overflow cannot be prevented a clear error will be raised now.
   - Added in-file caching for OCSP response caching
   - The write_pandas function now supports transient tables through the new table_type argument which supersedes create_temp_table argument
   - Fixed a bug where calling fetch_pandas_batches incorrectly raised NotSupportedError after an async query was executed
   - Added support for OKTA Identity Engine

- v2.7.11(July 26,2022)

   - Added minimum version pin to typing_extensions

- v2.7.10(July 22,2022)

   - Release wheels are now built on manylinux2014
   - Bumped supported pyarrow version to >=8.0.0,<8.1.0
   - Updated vendored library versions requests to 2.28.1 and urllib3 to 1.26.10
   - Added in-memory cache to OCSP requests
   - Added overwrite option to write_pandas
   - Added attribute `lastrowid` to `SnowflakeCursor` in compliance with PEP249.
   - Fixed a bug where gzip compressed http requests might be garbled by an unflushed buffer
   - Added new connection diagnostics capabilities to snowflake-connector-python
   - Bumped numpy dependency from <1.23.0 to <1.24.0


- v2.7.9(June 26,2022)

   - Fixed a bug where errors raised during get_results_from_sfqid() were missing errno
   - Fixed a bug where empty results containing GEOGRAPHY type raised IndexError


- v2.7.8(May 28,2022)

   - Updated PyPi documentation link to python specific main page
   - Fixed an error message that appears when pandas optional dependency group is required but is not installed
   - Implemented the DB API 2 callproc() method
   - Fixed a bug where decryption took place before decompression when downloading files from stages
   - Fixed a bug where s3 accelerate configuration was handled incorrectly
   - Extra named arguments given executemany() are now forwarded to execute()
   - Automatically sets the application name to streamlit when streamlit is imported and application name was not explicitly set
   - Bumped pyopenssl dependency version to >=16.2.0,<23.0.0


- v2.7.7(April 30,2022)

   - Bumped supported pandas version to < 1.5.0
   - Fixed a bug where partner name (from SF_PARTNER environmental variable) was set after connection was established
   - Added a new _no_retry option to executing queries
   - Fixed a bug where extreme timestamps lost precision


- v2.7.6(March 17,2022)

   - Fixed missing python_requires tag in setup.cfg

- v2.7.5(March 17,2022)

   - Added an option for partners to inject their name through an environmental variable (SF_PARTNER)
   - Fixed a bug where we would not wait for input if a browser window couldn't be opened for SSO login
   - Deprecate support for Python 3.6
   - Exported a type definition for SnowflakeConnection
   - Fixed a bug where final Arrow table would contain duplicate index numbers when using fetch_pandas_all

- v2.7.4(February 05,2022)

   - Add Geography Types
   - Removing automated incident reporting code
   - Fixed a bug where circular reference would prevent garbage collection on some objects
   - Fixed a bug where `DatabaseError` was thrown when executing against a closed cursor instead of `InterfaceError`
   - Fixed a bug where calling `executemany` would crash if an iterator was supplied as args
   - Fixed a bug where violating `NOT NULL` constraint raised `DatabaseError` instead of `IntegrityError`

- v2.7.3(January 22,2022)

   - Fixed a bug where timezone was missing from retrieved Timestamp_TZ columns
   - Fixed a bug where a long running PUT/GET command could hit a Storage Credential Error while renewing credentials
   - Fixed a bug where py.typed was not being included in our release wheels
   - Fixed a bug where negative numbers were mangled when fetched with the connection parameter arrow_number_to_decimal
   - Improved the error message that is encountered when running GET for a non-existing file
   - Fixed rendering of our long description for PyPi
   - Fixed a bug where DUO authentication ran into errors if sms authentication was disabled for the user
   - Add the ability to auto-create a table when writing a pandas DataFrame to a Snowflake table
   - Bumped the maximum dependency version of numpy from <1.22.0 to <1.23.0

- v2.7.2(December 17,2021)

   - Added support for Python version 3.10.
   - Fixed an issue bug where _get_query_status failed if there was a network error.
   - Added the interpolate_empty_sequences connection parameter to control interpolating empty sequences into queries.
   - Fixed an issue where where BLOCKED was considered to be an error by is_an_error.
   - Added source field to Telemetry.
   - Increased the cryptography dependency version.
   - Increased the pyopenssl dependency version.
   - Fixed an issue where dbapi.Binary returned a string instead of bytes.
   - Increased the required version of numpy.
   - Increased the required version of keyring.
   - Fixed issue so that fetch functions now return a typed DataFrames and pyarrow Tables for empty results.
   - Added py.typed
   - Improved error messages for PUT/GET.
   - Added Cursor.query attribute for accessing last query.
   - Increased the required version of pyarrow.


- v2.7.1(November 19,2021)

   - Fixed a bug where uploading a streaming file with multiple parts did not work.
   - JWT tokens are now regenerated when a request is retired.
   - Updated URL escaping when uploading to AWS S3 to match how S3 escapes URLs.
   - Removed the unused s3_connection_pool_size connection parameter.
   - Blocked queries are now be considered to be still running.
   - Snowflake specific exceptions are now set using Exception arguments.
   - Fixed an issue where use_s3_regional_url was not set correctly by the connector.


- v2.7.0(October 25,2021)

   - Removing cloud sdks.snowflake-connector-python will not install them anymore. Recreate your virtualenv to get rid of unnecessary dependencies.
   - Include Standard C++ headers.
   - Update minimum dependency version pin of cryptography.
   - Fixed a bug where error number would not be added to Exception messages.
   - Fixed a bug where client_prefetch_threads parameter was not respected when pre-fetching results.
   - Update signature of SnowflakeCursor.execute's params argument.


- v2.6.2(September 27,2021)

   - Updated vendored urllib3 and requests versions.
   - Fixed a bug where GET commands would fail to download files from sub directories from stages.
   - Added a feature where where the connector will print the url it tried to open when it is unable to open it for external browser authentication.


- v2.6.1(September 16,2021)

   - Bump pandas version from <1.3 to <1.4
   - Fixing Python deprecation warnings.
   - Added more type-hints.
   - Marked HeartBeatTimer threads as daemon threads.
   - Force cast a column into integer in write_pandas to avoid a rare behavior that would lead to crashing.
   - Implement AWS signature V4 to new SDKless PUT and GET.
   - Removed a deprecated setuptools option from setup.py.
   - Fixed a bug where error logs would be printed for query executions that produce no results.
   - Fixed a bug where the temporary stage for bulk array inserts exists.


- v2.6.0(August 29,2021)

   - Internal change to the implementation of result fetching.
   - Upgraded Pyarrow version from 3.0 to 5.0.
   - Internal change to the implementation for PUT and GET. A new connection parameter use_new_put_get was added to toggle between implementations.
   - Fixed a bug where executemany did not detect the type of data it was inserting.
   - Updated the minimum Mac OSX build target from 10.13 to 10.14.


- v2.5.1(July 31,2021)

   - Fixes Python Connector bug that prevents the connector from using AWS S3 Regional URL. The driver currently overrides the regional URL information with the default S3 URL causing failure in PUT.


- v2.5.0(July 22,2021)

   - Fixed a bug in write_pandas when quote_identifiers is set to True the function would not actually quote column names.
   - Bumping idna dependency pin from <3,>=2.5 to >=2.5,<4
   - Fix describe method when running `insert into ...` commands


- v2.4.6(June 25,2021)

   - Fixed a potential memory leak.
   - Removed upper certifi version pin.
   - Updated vendored libraries , urllib(1.26.5) and requests(2.25.1).
   - Replace pointers with UniqueRefs.
   - Changed default value of client_session_keep_alive to None.
   - Added the ability to retrieve metadata/schema without executing the query (describe method).

- v2.4.5(June 15,2021)

   - Fix for incorrect JWT token invalidity when an account alias with a dash in it is used for regionless account URL.

- v2.4.4(May 30,2021)

   - Fixed a segfault issue when using DictCursor and arrow result format with out of range dates.
   - Adds new make_pd_writer helper function


- v2.4.3(April 29,2021)

   - Uses s3 regional URL in private links when a param is set.
   - New Arrow NUMBER to Decimal converter option.
   - Update pyopenssl requirement from <20.0.0,>=16.2.0 to >=16.2.0,<21.0.0.
   - Update pandas requirement from <1.2.0,>=1.0.0 to >=1.0.0,<1.3.0.
   - Update numpy requirement from <1.20.0 to <1.21.0.


- v2.4.2(April 03,2021)

   - PUT statements are now thread-safe.


- v2.4.1(March 04,2021)

   - Make connection object exit() aware of status of parameter `autocommit`


- v2.4.0(March 04,2021)

   - Added support for Python 3.9 and PyArrow 3.0.x.
   - Added support for the upcoming multipart PUT threshold keyword.
   - Added support for using the PUT command with a file-like object.
   - Added some compilation flags to ease building conda community package.
   - Removed the pytz pin because it doesn't follow semantic versioning release format.
   - Added support for optimizing batch inserts through bulk array binding.


- v2.3.10(February 01,2021)

   - Improved query ID logging and added request GUID logging.
   - For dependency checking, increased the version condition for the pyjwt package from <2.0.0 to <3.0.0.


- v2.3.9(January 27,2021)

   - The fix to add proper proxy CONNECT headers for connections made over proxies.


- v2.3.8(January 14,2021)

   - Arrow result conversion speed up.
   - Send all Python Connector exceptions to in-band or out-of-band telemetry.
   - Vendoring requests and urllib3 to contain OCSP monkey patching to our library only.
   - Declare dependency on setuptools.


- v2.3.7(December 10,2020)

   - Added support for upcoming downscoped GCS credentials.
   - Tightened the pyOpenSSL dependency pin.
   - Relaxed the boto3 dependency pin up to the next major release.
   - Relaxed the cffi dependency pin up to the next major release.
   - Added support for executing asynchronous queries.
   - Dropped support for Python 3.5.

- v2.3.6(November 16,2020)

   - Fixed a bug that was preventing the connector from working on Windows with Python 3.8.
   - Improved the string formatting in exception messages.
   - For dependency checking, increased the version condition for the cryptography package from <3.0.0 to <4.0.0.
   - For dependency checking, increased the version condition for the pandas package from <1.1 to <1.2.

- v2.3.5(November 03,2020)

   - Updated the dependency on the cryptography package from version 2.9.2 to 3.2.1.

- v2.3.4(October 26,2020)

   - Added an optional parameter to the write_pandas function to specify that identifiers should not be quoted before being sent to the server.
   - The write_pandas function now honors default and auto-increment values for columns when inserting new rows.
   - Updated the Python Connector OCSP error messages and accompanying telemetry Information.
   - Enabled the runtime pyarrow version verification to fail gracefully. Fixed a bug with AWS glue environment.
   - Upgraded the version of boto3 from 1.14.47 to 1.15.9.
   - Upgraded the version of idna from 2.9 to 2.10.

- v2.3.3(October 05,2020)

   - Simplified the configuration files by consolidating test settings.
   - In the Connection object, the execute_stream and execute_string methods now filter out empty lines from their inputs.

- v2.3.2(September 14,2020)

   - Fixed a bug where a file handler was not closed properly.
   - Fixed various documentation typos.

- v2.3.1(August 25,2020)

   - Fixed a bug where 2 constants were removed by mistake.

- v2.3.0(August 24,2020)

   - When the log level is set to DEBUG, log the OOB telemetry entries that are sent to Snowflake.
   - Fixed a bug in the PUT command where long running PUTs would fail to re-authenticate to GCP for storage.
   - Updated the minimum build target MacOS version to 10.13.

- v2.2.10(August 03,2020)

    - Improved an error message for when "pandas" optional dependency group is not installed and user tries to fetch data into a pandas DataFrame. It'll now point user to our online documentation.

- v2.2.9(July 13,2020)

    - Connection parameter validate_default_parameters now verifies known connection parameter names and types. It emits warnings for anything unexpected types or names.
    - Correct logging messages for compiled C++ code.
    - Fixed an issue in write_pandas with location determination when database, or schema name was included.
    - Bumped boto3 dependency version.
    - Fixed an issue where uploading a file with special UTF-8 characters in their names corrupted file.

- v2.2.8(June 22,2020)

    - Switched docstring style to Google from Epydoc and added automated tests to enforce the standard.
    - Fixed a memory leak in DictCursor's Arrow format code.

- v2.2.7(June 1,2020)

    - Support azure-storage-blob v12 as well as v2 (for Python 3.5.0-3.5.1) by Python Connector
    - Fixed a bug where temporary directory path was not Windows compatible in write_pandas function
    - Added out of band telemetry error reporting of unknown errors

- v2.2.6(May 11,2020)

    - Update Pyarrow version from 0.16.0 to 0.17.0 for Python connector
    - Remove more restrictive application name enforcement.
    - Missing keyring dependency will not raise an exception, only emit a debug log from now on.
    - Bumping boto3 to <1.14
    - Fix flake8 3.8.0 new issues
    - Implement Python log interceptor

- v2.2.5(April 30,2020)

    - Added more efficient way to ingest a pandas.Dataframe into Snowflake, located in snowflake.connector.pandas_tools
    - More restrictive application name enforcement and standardizing it with other Snowflake drivers
    - Added checking and warning for users when they have a wrong version of pyarrow installed

- v2.2.4(April 10,2020)

    - Emit warning only if trying to set different setting of use_openssl_only parameter

- v2.2.3(March 30,2020)

    - Secure SSO ID Token
    - Add use_openssl_only connection parameter, which disables the usage of pure Python cryptographic libraries for FIPS
    - Add manylinux1 as well as manylinux2010
    - Fix a bug where a certificate file was opened and never closed in snowflake-connector-python.
    - Fix python connector skips validating GCP URLs
    - Adds additional client driver config information to in band telemetry.

- v2.2.2(March 9,2020)

    - Fix retry with chunck_downloader.py for stability.
    - Support Python 3.8 for Linux and Mac.

- v2.2.1(February 18,2020)

    - Fix use DictCursor with execute_string #248

- v2.2.0(January 27,2020)

    - Drop Python 2.7 support
    - AWS: When OVERWRITE is false, which is set by default, the file is uploaded if no same file name exists in the stage. This used to check the content signature but it will no longer check. Azure and GCP already work this way.
    - Document Python connector dependencies on our GitHub page in addition to Snowflake docs.
    - Fix sqlalchemy and possibly python-connector warnings.
    - Fix GCP exception using the Python connector to PUT a file in a stage with auto_compress=false.
    - Bump up botocore requirements to 1.14.
    - Fix uppercaseing authenticator breaks Okta URL which may include case-sensitive elements(#257).
    - Fix wrong result bug while using fetch_pandas_all() to get fixed numbers with large scales.
    - Increase multi part upload threshold for S3 to 64MB.

- v2.1.3(January 06,2020)

    - Fix GCP Put failed after hours

- v2.1.2(December 16,2019)

    - Fix the arrow bundling issue for python connector on mac.
    - Fix the arrow dll bundle issue on windows.Add more logging.

- v2.1.1(December 12,2019)

    - Fix GZIP uncompressed content for Azure GET command.
    - Add support for GCS PUT and GET for private preview.
    - Support fetch as numpy value in arrow result format.
    - Fix NameError: name 'EmptyPyArrowIterator' is not defined for Mac.
    - Return empty dataframe for fetch_pandas_all() api if result set is empty.

- v2.1.0(December 2,2019)

    - Fix default `ssl_context` options
    - Pin more dependencies for Python Connector
    - Fix import of SnowflakeOCSPAsn1Crypto crashes Python on MacOS Catalina
    - Update the release note that 1.9.0 was removed
    - Support DictCursor for arrow result format
    - Upgrade Python's arrow lib to 0.15.1
    - Raise Exception when PUT fails to Upload Data
    - Handle year out of range correctly in arrow result format

- v2.0.4(November 13,2019)

    - Increase OCSP Cache expiry time from 24 hours to 120 hours.
    - Fix pyarrow cxx11 abi compatibility issue
    - Use new query result format parameter in python tests

- v2.0.3(November 1,2019)

    - Fix for ,Pandas fetch API did not handle the case that first chunk is empty correctly.
    - Updated with botocore, boto3 and requests packages to the latest version.
    - Pinned stable versions of Azure urllib3 packages.

- v2.0.2(October 21,2019)

    - Fix sessions remaining open even if they are disposed manually. Retry deleting session if the connection is explicitly closed.
    - Fix memory leak in the new fetch pandas API
    - Fix Auditwheel failed with python37
    - Reduce the footprint of Python Connector
    - Support asn1crypto 1.1.x
    - Ensure that the cython components are present for Conda package

- v2.0.1(October 04,2019)

    - Add asn1crypto requirement to mitigate incompatibility change

- v2.0.0(September 30,2019)

    - Release Python Connector 2.0.0 for Arrow format change.
    - Fix SF_OCSP_RESPONSE_CACHE_DIR referring to the OCSP cache response file directory and not the top level of directory.
    - Fix Malformed certificate ID key causes uncaught KeyError.
    - No retry for certificate errors.
    - Fix In-Memory OCSP Response Cache - PythonConnector
    - Move AWS_ID and AWS_SECRET_KEY to their newer versions in the Python client
    - Fix result set downloader for ijson 2.5
    - Make authenticator field case insensitive earlier
    - Update USER-AGENT to be consistent with new format
    - Update Python Driver URL Whitelist to support US Gov domain
    - Fix memory leak in python connector panda df fetch API

- v1.9.1(October 4,2019)

    - Add asn1crypto requirement to mitigate incompatibility change.

- v1.9.0(August 26,2019) **REMOVED from pypi due to dependency compatibility issues**

    - Implement converter for all arrow data types in python connector extension
    - Fix arrow error when returning empty result using python connecter
    - Fix OCSP responder hang, AttributeError: 'ReadTimeout' object has no attribute 'message'
    - Update OCSP Connection timeout.
    - Fix RevokedCertificateError OOB Telemetry events are not sent
    - Uncaught RevocationCheckError for FAIL_OPEN in create_pair_issuer_subject
    - Fix uncaught exception in generate_telemetry_data function
    - Fix connector looses context after connection drop/restore by retrying IncompleteRead error.
    - Make tzinfo class at the module level instead of inlining

- v1.8.7(August 12,2019)

    - Rewrote validateDefaultParameters to validate the database, schema and warehouse at connection time. False by default.
    - Fix OCSP Server URL problem in multithreaded env
    - Fix Azure Gov PUT and GET issue

- v1.8.6(July 29,2019)

    - Reduce retries for OCSP from Python Driver
    - Azure PUT issue: ValueError: I/O operation on closed file
    - Add client information to USER-AGENT HTTP header - PythonConnector
    - Better handling of OCSP cache download failure

- v1.8.5(July 15,2019)

    - Drop Python 3.4 support for Python Connector

- v1.8.4(July 01,2019)

    - Update Python Connector to discard invalid OCSP Responses while merging caches

- v1.8.3(June 17,2019)

    - Update Client Driver OCSP Endpoint URL for Private Link Customers
    - Ignore session gone 390111 when closing
    - Python3.4 using requests 2.21.0 needs older version of urllib3
    - Use Account Name for Global URL

- v1.8.2 (June 03,2019)

    - Pendulum datatype support

- v1.8.1 (May 20,2019)

    - Revoked OCSP Responses persists in Driver Cache + Logging Fix
    - Fixed DeprecationWarning: Using or importing the ABCs from 'collections' instead of from 'collections.abc' is deprecated

- v1.8.0 (May 10, 2019)

    - support ``numpy.bool_`` in binding type
    - Add Option to Skip Request Pooling
    - Add OCSP_MODE metric
    - Fixed PUT URI issue for Windows path
    - OCSP SoftFail

- v1.7.11 (April 22, 2019)

    - numpy timestamp with timezone support
    - qmark not binding None

- v1.7.10 (April 8, 2019)

    - Fix the incorrect custom Server URL in Python Driver for Privatelink

- v1.7.9 (March 25,2019)

    - Python Interim Solution for Custom Cache Server URL
    - Internal change for pending feature

- v1.7.8 (March 12,2019)

    - Add OCSP signing certificate validity check

- v1.7.7 (February 22,2019)

    - Skip HEAD operation when OVERWRITE=true for PUT
    - Update copyright year from 2018 to 2019 for Python

- v1.7.6 (February 08,2019)

    - Adjusted pyasn1 and pyasn1-module requirements for Python Connector
    - Added idna to setup.py. made pyasn1 optional for Python2

- v1.7.5 (January 25, 2019)

    - Incorporate "kwargs" style group of key-value pairs in connection's "execute_string" function.

- v1.7.4 (January 3, 2019)

    - Invalidate outdated OCSP response when checking cache hit
    - Made keyring use optional in Python Connector
    - Added SnowflakeNullConverter for Python Connector to skip all client side conversions
    - Honor ``CLIENT_PREFETCH_THREADS`` to download the result set.
    - Fixed the hang when region=us-west-2 is specified.
    - Added Python 3.7 tests

- v1.7.3 (December 11, 2018)

    - Improved the progress bar control for SnowSQL
    - Fixed PUT/GET progress bar for Azure

- v1.7.2 (December 4, 2018)

    - Refactored OCSP checks
    - Adjusted log level to mitigate confusions

- v1.7.1 (November 27, 2018)

    - Fixed regex pattern warning in cursor.py
    - Fixed 403 error for EU deployment
    - Fixed the epoch time to datetime object converter for Windoww

- v1.7.0 (November 13, 2018)

    - Internal change for pending feature.

- v1.6.12 (October 30, 2018)

    - Updated ``boto3`` and ``botocore`` version dependeny.
    - Catch socket.EAI_NONAME for localhost socket and raise a better error message
    - Added ``client_session_keep_alive_heartbeat_frequency`` to control heartbeat timings for ``client_session_keep_alive``.

- v1.6.11 (October 23, 2018)

    - Fixed exit_on_error=true didn't work if PUT / GET error occurs
    - Fixed a backslash followed by a quote in a literal was not taken into account.
    - Added ``request_guid`` to each HTTP request for tracing.

- v1.6.10 (September 25, 2018)

    - Added ``client_session_keep_alive`` support.
    - Fixed multiline double quote expressions PR #117 (@bensowden)
    - Fixed binding ``datetime`` for TIMESTAMP type in ``qmark`` binding mode. PR #118 (@rhlahuja)
    - Retry HTTP 405 to mitigate Nginx bug.
    - Accept consent response for id token cache. WIP.

- v1.6.9 (September 13, 2018)

    - Changed most INFO logs to DEBUG. Added INFO for key operations.
    - Fixed the URL query parser to get multiple values.

- v1.6.8 (August 30, 2018)

    - Updated ``boto3`` and ``botocore`` version dependeny.

- v1.6.7 (August 22, 2018)

    - Enforce virtual host URL for PUT and GET.
    - Added retryCount, clientStarTime for query-request for better service.

- v1.6.6 (August 9, 2018)

    - Replaced ``pycryptodome`` with ``pycryptodomex`` to avoid namespace conflict with ``PyCrypto``.
    - Fixed hang if the connection is not explicitly closed since 1.6.4.
    - Reauthenticate for externalbrowser while running a query.
    - Fixed remove_comments option for SnowSQL.

- v1.6.5 (July 13, 2018)

    - Fixed the current object cache in the connection for id token use.
    - Added no OCSP cache server use option.

- v1.6.4 (July 5, 2018)

    - Fixed div by zero for Azure PUT command.
    - Cache id token for SSO. This feature is WIP.
    - Added telemetry client and job timings by @dsouzam.

- v1.6.3 (June 14, 2018)

    - Fixed binding long value for Python 2.

- v1.6.2 (June 7, 2018)

    - Removes username restriction for OAuth. PR 86(@tjj5036)
    - Retry OpenSSL.SysError in tests
    - Updated concurrent insert test as the server improved.

- v1.6.1 (May 17, 2018)

    - Enable OCSP Dynamic Cache server for privatelink.
    - Ensure the type of ``login_timeout`` attribute is ``int``.

- v1.6.0 (May 3, 2018)

    - Enable OCSP Cache server by default.

- v1.5.8 (April 26, 2018)

    - Fixed PUT command error 'Server failed to authenticate the request. Make sure the value of Authorization header is formed correctly including the signature.' for Azure deployment.

- v1.5.7 (April 19, 2018)

    - Fixed object has no attribute errors in Python3 for Azure deployment.
    - Removed ContentEncoding=gzip from the header for PUT command. This caused COPY failure if autocompress=false.

- v1.5.6 (April 5, 2018)

    - Updated ``boto3`` and ``botocore`` version dependeny.

- v1.5.5 (March 22, 2018)

    - Fixed TypeError: list indices must be integers or slices, not str. PR/Issue 75 (@daniel-sali).
    - Updated ``cryptography`` dependency.

- v1.5.4 (March 15, 2018)

    - Tightened ``pyasn`` and ``pyasn1-modules`` version requirements
    - Added OS and OS_VERSION session info.
    - Relaxed ``pycryptodome`` version requirements. No 3.5.0 should be used.

- v1.5.3 (March 9, 2018)

    - Pulled back ``pyasn1`` for OCSP check in Python 2. Python 3 continue using ``asn1crypto`` for better performance.
    - Limit the upper bound of ``pycryptodome`` version to less than 3.5.0 for Issue 65.

- v1.5.2 (March 1, 2018)

    - Fixed failue in case HOME/USERPROFILE is not set.
    - Updated ``boto3`` and ``botocore`` version dependeny.

- v1.5.1 (February 15, 2018)

    - Prototyped oauth. Won't work without the server change.
    - Retry OCSP data parse failure
    - Fixed paramstyle=qmark binding for SQLAlchemy

- v1.5.0 (January 26, 2018)

    - Removed ``pyasn1`` and ``pyasn1-modules`` from the dependency.
    - Prototyped key pair authentication.
    - Fixed OCSP response cache expiration check.

- v1.4.17 (January 19, 2018)

    - Adjusted ``pyasn1`` and ``pyasn1-modules`` version dependency. PR 48 (@baxen)
    - Started replacing ``pyasn1`` with ``asn1crypto`` Not activated yet.

- v1.4.16 (January 16, 2018)

    - Added OCSP cache related tools.

- v1.4.15 (January 11, 2018)

    - Added OCSP cache server option.

- v1.4.14 (December 14, 2017)

    - Improved OCSP response dump util.

- v1.4.13 (November 30, 2017)

    - Updated ``boto3`` and ``botocore`` version dependeny.

- v1.4.12 (November 16, 2017)

    - Added ``qmark`` and ``numeric`` paramstyle support for server side binding.
    - Added ``timezone`` session parameter support to connections.
    - Fixed a file handler leak in OCSP checks.

- v1.4.11 (November 9, 2017)

    - Fixed Azure PUT command to use AES CBC key encryption.
    - Added retry for intermittent PyAsn1Error.

- v1.4.10 (October 26, 2017)

    - Added Azure support for PUT and GET commands.
    - Updated ``cryptography``, ``boto3`` and ``botocore`` version dependeny.

- v1.4.9 (October 10, 2017)

    - Fixed a regression caused by ``pyasn1`` upgrade.

- v1.4.8 (October 5, 2017)

    - Updated Fed/SSO parameters. The production version of Fed/SSO from Python Connector requires this version.
    - Refactored for Azure support
    - Set CLIENT_APP_ID and CLIENT_APP_VERSION in all requests
    - Support new behaviors of newer version of ``pyasn1``. Relaxed the dependency.
    - Making socket timeout same as the login time
    - Fixed the case where no error message is attached.

- v1.4.7 (September 20, 2017)

    - Refresh AWS token in PUT command if S3UploadFailedError includes the ExpiredToken error
    - Retry all of 5xx in connection

- v1.4.6 (September 14, 2017)

    - Mitigated sigint handler config failure for SQLAlchemy
    - Improved the message for invalid SSL certificate error
    - Retry forever for query to mitigate 500 errors

- v1.4.5 (August 31, 2017)

    - Fixed regression in #34 by rewriting SAML 2.0 compliant service application support.
    - Cleaned up logger by moving instance to module.

- v1.4.4 (August 24, 2017)

    - Fixed Azure blob certificate issue. OCSP response structure bug fix
    - Added SAML 2.0 compliant service application support. preview feature.
    - Upgraded SSL wrapper with the latest urllib3 pyopenssl glue module. It uses kqueue, epoll or poll in replacement of select to read data from socket if available.

- v1.4.3 (August 17, 2017)

    - Changed the log levels for some messages from ERROR to DEBUG to address confusion as real incidents. In fact, they are not real issues but signals for connection retry.
    - Added ``certifi`` to the dependent component list to mitigate CA root certificate out of date issue.
    - Set the maximum versions of dependent components ``boto3`` and ``botocore``.
    - Updated ``cryptography`` and ``pyOpenSSL`` version dependeny change.
    - Added a connection parameter ``validate_default_parameters`` to validate the default database, schema and warehouse. If the specified object doesn't exist, it raises an error.

- v1.4.2 (August 3, 2017)

    - Fixed retry HTTP 400 in upload file when AWS token expires
    - Relaxed the version of dependent components ``pyasn1`` and ``pyasn1-modules``

- v1.4.1 (July 26, 2017)

    - Pinned ``pyasn1`` and ``pyasn1-modules`` versions to 0.2.3 and 0.0.9, respectively

- v1.4.0 (July 6, 2017)

    - Relaxed the versions of dependent components ``boto3``, ``botocore``, ``cffi`` and ``cryptography`` and ``pyOpenSSL``
    - Minor improvements in OCSP response file cache

- v1.3.18 (June 15, 2017)

    - Fixed OCSP response cache file not found issue on Windows. Drive letter was taken off
    - Use less restrictive cryptography>=1.7,<1.8
    - Added ORC detection in PUT command

- v1.3.17 (June 1, 2017)

    - Timeout OCSP request in 60 seconds and retry
    - Set autocommit and abort_detached_query session parameters in authentication time if specified
    - Fixed cross region stage issue. Could not get files in us-west-2 region S3 bucket from us-east-1

- v1.3.16 (April 20, 2017)

    - Fixed issue in fetching ``DATE`` causing [Error 22] Invalid argument on Windows
    - Retry on ``RuntimeError`` in requests

- v1.3.15 (March 30, 2017)

    - Refactored data converters in fetch to improve performance
    - Fixed timestamp format FF to honor the scale of data type
    - Improved the security of OKTA authentication with hostname verifications
    - Retry PUT on the error ``OpenSSL.SSL.SysCallError`` 10053 with lower concurrency
    - Added ``raw_msg`` attribute to ``Error`` class
    - Refactored session managements

- v1.3.14 (February 24, 2017)

    - Improved PUT and GET error handler.
    - Added proxy support to OCSP checks.
    - Use proxy parameters for PUT and GET commands.
    - Added ``sfqid`` and ``sqlstate`` to the results from query results.
    - Fixed the connection timeout calculation based on ``login_timeout`` and ``network_timeout``.
    - Improved error messages in case of 403, 502 and 504 HTTP reponse code.
    - Upgraded ``cryptography`` to 1.7.2, ``boto3`` to 1.4.4 and ``botocore`` to 1.5.14.
    - Removed explicit DNS lookup for OCSP URL.

- v1.3.13 (February 9, 2017)

    - Fixed AWS SQS connection error with OCSP checks
    - Added ``login_timeout`` and ``network_timeout`` parameters to the ``Connection`` objects.
    - Fixed forbidden access error handing

- v1.3.12 (February 2, 2017)

    - Fixed ``region`` parameter. One character was truncated from the tail of account name
    - Improved performance of fetching data by refactoring fetchone method

- v1.3.11 (January 27, 2017)

    - Fixed the regression in 1.3.8 that caused intermittent 504 errors

- v1.3.10 (January 26, 2017)

    - Compress data in HTTP requests at all times except empty data or OKTA request
    - Refactored FIXED, REAL and TIMESTAMP data fetch to improve performance. This mainly impacts SnowSQL
    - Added ``region`` option to support EU deployments better
    - Increased the retry counter for OCSP servers to mitigate intermittent failure
    - Refactored HTTP access retry logic

- v1.3.9 (January 16, 2017)

    - Upgraded ``botocore`` to 1.4.93 to fix and ``boto3`` to 1.4.3 to fix the HTTPS request failure in Python 3.6
    - Fixed python2 incomaptible import http.client
    - Retry OCSP validation in case of non-200 HTTP code returned

- v1.3.8 (January 12, 2017)

    - Convert non-UTF-8 data in the large result set chunk to Unicode replacement characters to avoid decode error.
    - Updated copyright year to 2017.
    - Use `six` package to support both PY2 and PY3 for some functions
    - Upgraded ``cryptography`` to 1.7.1 to address MacOS Python 3.6 build issue.
    - Fixed OverflowError caused by invalid range of timetamp data for SnowSQL.

- v1.3.7 (December 8, 2016)

    - Increased the validity date acceptance window to prevent OCSP returning invalid responses due to out-of-scope validity dates for certificates.
    - Enabled OCSP response cache file by default.

- v1.3.6 (December 1, 2016)

    - Upgraded ``cryptography`` to 1.5.3, ``pyOpenSSL`` to 16.2.0 and ``cffi`` to 1.9.1.

- v1.3.5 (November 17, 2016)

    - Fixed CA list cache race condition
    - Added retry intermittent 400 HTTP ``Bad Request`` error

- v1.3.4 (November 3, 2016)

    - Added ``quoted_name`` data type support for binding by SQLAlchemy
    - Not to compress ``parquiet`` file in PUT command

- v1.3.3 (October 20, 2016)

    - Downgraded ``botocore`` to 1.4.37 due to potential regression.
    - Increased the stability of PUT and GET commands

- v1.3.2 (October 12, 2016)

    - Upgraded ``botocore`` to 1.4.52.
    - Set the signature version to v4 to AWS client. This impacts ``PUT``, ``GET`` commands and fetching large result set.

- v1.3.1 (September 30, 2016)

    - Added an account name including subdomain.

- v1.3.0 (September 26, 2016)

    - Added support for the ``BINARY`` data type, which enables support for more Python data types:

        - Python 3:

            - ``bytes`` and ``bytearray`` can be used for binding.
            - ``bytes`` is also used for fetching ``BINARY`` data type.

        - Python 2:

            - ``bytearray`` can be used for binding
            - ``str`` is used for fetching ``BINARY`` data type.

    - Added ``proxy_user`` and ``proxy_password`` connection parameters for proxy servers that require authentication.

- v1.2.8 (August 16, 2016)

    - Upgraded ``botocore`` to 1.4.37.
    - Added ``Connection.execute_string`` and ``Connection.execute_stream`` to run multiple statements in a string and stream.
    - Increased the stability of fetching data for Python 2.
    - Refactored memory usage in fetching large result set (Work in Progress).

- v1.2.7 (July 31, 2016)

    - Fixed ``snowflake.cursor.rowcount`` for INSERT ALL.
    - Force OCSP cache invalidation after 24 hours for better security.
    - Use ``use_accelerate_endpoint`` in PUT and GET if Transfer acceleration is enabled for the S3 bucket.
    - Fixed the side effect of ``python-future`` that loads ``test.py`` in the current directory.

- v1.2.6 (July 13, 2016)

    - Fixed the AWS token renewal issue with PUT command when uploading uncompressed large files.

- v1.2.5 (July 8, 2016)

    - Added retry for errors ``S3UploadFailedError`` and ``RetriesExceededError`` in PUT and GET, respectively.

- v1.2.4 (July 6, 2016)

    - Added ``max_connection_pool`` parameter to Connection so that you can specify the maximum number of HTTP/HTTPS connections in the pool.
    - Minor enhancements for SnowSQL.

- v1.2.3 (June 29, 2016)

    - Fixed 404 issue in GET command. An extra slash character changed the S3 path and failed to identify the file to download.

- v1.2.2 (June 21, 2016)

    - Upgraded ``botocore`` to 1.4.26.
    - Added retry for 403 error when accessing S3.

- v1.2.1 (June 13, 2016)

    - Improved fetch performance for data types (part 2): DATE, TIME, TIMESTAMP, TIMESTAMP_LTZ, TIMESTAMP_NTZ and TIMESTAMP_TZ.

- v1.2.0 (June 10, 2016)

    - Improved fetch performance for data types (part 1): FIXED, REAL, STRING.

- v1.1.5 (June 2, 2016)

    - Upgraded ``boto3`` to 1.3.1 and ``botocore`` and 1.4.22.
    - Fixed ``snowflake.cursor.rowcount`` for DML by ``snowflake.cursor.executemany``.
    - Added ``numpy`` data type binding support. ``numpy.intN``, ``numpy.floatN`` and ``numpy.datetime64`` can be bound and fetched.

- v1.1.4 (May 21, 2016)

    - Upgraded ``cffi`` to 1.6.0.
    - Minor enhancements to SnowSQL.

- v1.1.3 (May 5, 2016)

    - Upgraded ``cryptography`` to 1.3.2.

- v1.1.2 (May 4, 2016)

    - Changed the dependency of ``tzlocal`` optional.
    - Fixed charmap error in OCSP checks.

- v1.1.1 (Apr 11, 2016)

    - Fixed OCSP revocation check issue with the new certificate and AWS S3.
    - Upgraded ``cryptography`` to 1.3.1 and ``pyOpenSSL`` to 16.0.0.

- v1.1.0 (Apr 4, 2016)

    - Added ``bzip2`` support in ``PUT`` command. This feature requires a server upgrade.
    - Replaced the self contained packages in ``snowflake._vendor`` with the dependency of ``boto3`` 1.3.0 and ``botocore`` 1.4.2.

- v1.0.7 (Mar 21, 2016)

    - Keep ``pyOpenSSL`` at 0.15.1.

- v1.0.6 (Mar 15, 2016)

    - Upgraded ``cryptography`` to 1.2.3.
    - Added support for ``TIME`` data type, which is now a Snowflake supported data type. This feature requires a server upgrade.
    - Added ``snowflake.connector.DistCursor`` to fetch the results in ``dict`` instead of ``tuple``.
    - Added compression to the SQL text and commands.

- v1.0.5 (Mar 1, 2016)

    - Upgraded ``cryptography`` to 1.2.2 and ``cffi`` to 1.5.2.
    - Fixed the conversion from ``TIMESTAMP_LTZ`` to datetime in queries.

- v1.0.4 (Feb 15, 2016)

    - Fixed the truncated parallel large result set.
    - Added retry OpenSSL low level errors ``ETIMEDOUT`` and ``ECONNRESET``.
    - Time out all HTTPS requests so that the Python Connector can retry the job or recheck the status.
    - Fixed the location of encrypted data for ``PUT`` command. They used to be in the same directory as the source data files.
    - Added support for renewing the AWS token used in ``PUT`` commands if the token expires.

- v1.0.3 (Jan 13, 2016)

    - Added support for the ``BOOLEAN`` data type (i.e. ``TRUE`` or ``FALSE``). This changes the behavior of the binding for the ``bool`` type object:

        - Previously, ``bool`` was bound as a numeric value (i.e. ``1`` for ``True``, ``0`` for ``False``).
        - Now, ``bool`` is bound as native SQL data (i.e. ``TRUE`` or ``FALSE``).

    - Added the ``autocommit`` method to the ``Connection`` object:

        - By default, ``autocommit`` mode is ON (i.e. each DML statement commits the change).
        - If ``autocommit`` mode is OFF, the ``commit`` and ``rollback`` methods are enabled.

    - Avoid segfault issue for ``cryptography`` 1.2 in Mac OSX by using 1.1 until resolved.

- v1.0.2 (Dec 15, 2015)

    - Upgraded ``boto3`` 1.2.2, ``botocore`` 1.3.12.
    - Removed ``SSLv3`` mapping from the initial table.

- v1.0.1 (Dec 8, 2015)

    - Minor bug fixes.

- v1.0.0 (Dec 1, 2015)

    - General Availability release.
