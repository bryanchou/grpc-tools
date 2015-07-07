# Performance database server

The performance database server, implemented in C++, acts as the central coordinator for the clients submitting their data for the QPS tests, as well for providing data to the frontend which display this data. It is responsible for:

* Collecting performance metrics data such as Queries Per Second (QPS), Percentile Latencies, Server and Client times, etc. from the users and writing them to a database. The database has been implemented using [LevelDb](http://leveldb.org/), an open-source, light-weight, single-purpose library for persistence with bindings to many platforms.

* Receiving the authenticated username from the *Authentication server* for the user, using the hashed id of the user.

* Providing the web front-end (implemented using [Django](https://www.djangoproject.com/)) with data retrieved from the database.

The server timestamps the received data, obtains the username for the user by communicating with the Authentication server, and stores it to the database. Leveldb is a key-value storage database; here the key is chosen as a unique ID number which Google assigns to each user in their Google+ profile information.

The frontend makes two types of calls to the server: to obtain the data either for one user or for all the users. The server sends data to the frontend accordingly.

The server is started using a Python wrapper, _run_perf_db_server.py_ can be passed:
- The location the the performance database bin.
- The address of the performance database server in _hostname:port_ format.
- The address of the authentication server in _hostname:port_ format.
- The location of the performance database.

These parameters, if not passed, will assume default values. The user needs to run `make` before using the server with the Python wrapper, to generate the necessary binaries. The binary of the server is created in 'grpc-tools/benchmarking/performance_db/bins'. The command to run the server will typically look like:

    ./run_perf_db_server --bin=../../bins/perf_db_server --perf_server_addr=address --auth_server_addr=address --database=$HOME/.grpc/perf_db