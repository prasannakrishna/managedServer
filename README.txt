download axon server separately

This is the Axon Server Enterprise Edition, version 4.6

For information about the Axon Framework and Axon Server,
visit https://docs.axoniq.io.

Running Axon Server Enterprise edition
--------------------------------------

Axon Server Enterprise edition is expected to run in a cluster. If you want to start
using the Enterprise edition you will have to run an initialization command on the first
node in the cluster, and after that add other nodes to the first node.

To start the server on a specific node run the command:

    java -jar axonserver.jar

On the first node initialize the cluster using:

    java -jar axonserver-cli.jar init-cluster

On the other nodes, connect to the first node using:

    java -jar axonserver-cli.jar register-node -h <first-node-hostname>

For more information on setting up clusters and context check the reference guide at:

https://docs.axoniq.io/reference-guide/axon-server/installation/local-installation/axon-server-ee

Once Axon Server is running you can view its configuration using the Axon Dashboard at http://<axonserver>:8024.

Release Notes for version 4.6.10
--------------------------------
* Fix: the processing of items must not be interrupted if an exception occurs during sending
* Fix: improve handling of cancelled queries

Release Notes for version 4.6.9
-------------------------------
* Fix: reduce latency in replication with low traffic
* Fix: filter event processors in overview page based on the selected context
* Updated version of plugin-api library

Release Notes for version 4.6.8
-------------------------------
* Fix: using bloom filter index, invalid sequence number error when submitting events when the latest segment only contains non-domain events

Release Notes for version 4.6.7
-------------------------------
* Fix: startup issue when access control is enabled and some properties are not set

Release Notes for version 4.6.6
-------------------------------
* Fix: memory leak due to a closed event processor keeping memory
* Fix: confusing log message about the license file
* Fix: deletion of an application token does not have effect for contexts that are not on the _admin nodes
* In the UI, the pagination settings for the tables are now kept in the current session
* Updated gRPC and Netty versions to avoid startup error on alpine linux

Release Notes for version 4.6.5
-------------------------------
* OAuth extension (4.6.1) now also supports OIDC authentication
* Fix: deserialization of forwarded authentication fails and prints stacktrace in log file
* Fix: updated version of LDAP extension (4.6.1) to include fixes from version 4.5.2
* Fix: update flow control library version to 1.1
* Fix: change health status for commands to only show warning when there are queued messages and no permits
* Updated audit logging for authentication failures

Release Notes for version 4.6.4
-------------------------------
* Fix: contexts not shown in CLI when the request is using an application token or system token
* Fix: change column name in event processor overview to "Active Segments"
* Fix: null pointer exception popup in dashboard

Release Notes for version 4.6.3
--------------------------------
* Fix for bloom filter index: reading aggregate events searches for older events when the last event sequence number
  is the same as the snapshot sequence number
* New property for bloom filter index, axoniq.axonserver.event.segments-for-sequence-number-check, defines the number of segments
  that Axon Server will check for events on an aggregate when an event with sequence number 0
  is stored. The default value for this property is 10.
  For performance reasons, if you increase this property to a value higher than 100 it is recommended
  to also increase the axoniq.axonserver.event.max-bloom-filters-in-memory property.

Release Notes for version 4.6.2
-------------------------------
* Fix: reading aggregate events hangs on JVM Error
* Fix: canceling an event store query through the gRPC interface does not close the stream
* Fix: event processor operations unavailable in the dashboard for applications using Axon Framework version before 4.5
* Fix: when sending two commands or queries with the same message identifier at the same time, one does not get completed
* Fix: the init-cluster CLI command fails due to unexpected response code
* Fix: AxonServerInformationProvider should return enterprise edition specific information
* Fix: Admin API operations to get context information should check for granted roles for the context
  if the application has no roles for the _admin context

Release Notes for version 4.6.1
-------------------------------
* Security update: updated control database settings

Release Notes for version 4.6.0
--------------------------------
New features:
- Clients can now perform administrative operations in Axon Server through the axonserver-connector-java (requires version 4.6.0 of the connector).
- Streaming queries (requires Axon Framework version 4.6.0). When returning a collection of results from a query, the results can be streamed instead of collecting
  them in the query handler first.
- Support for updating properties for an existing context

Enhancements:
- Large message support. Before increasing the max-message-size on Axon Server had the effect that the replication log segments were also increased in size. This
  dependency is no longer there.
- Number of events per transaction is no longer limited to 32K.
- Support for using the CLI when the caller is behind a proxy.
- Show complex event metadata values in query result/REST responses if they are printable
- livenessstate and readinessstate probes are now included in the /actuator/health endpoint output by default
- Properties now support more readable values using units
- Changing event processor states through Axon Server now waits for a result from the client
- Clients can now also connect to nodes in a replication group that are not primary or messaging-only nodes.
  To force clients to connect to primary or message-only nodes, set the property force-connection-to-primary-or-messaging-node to true
- Plugins can now use AxonServerInformationProvider to get information on the Axon Server version
- UI updated
- show complex metadata values in query results

Dependency updates:
- updated gRPC and Netty versions
- updated Spring Boot version
- moved to OpenAPI for Swagger support

Bug fixes:
- moved reading indexes from the gRPC thread to prevent blocking these threads
- not able to use an empty internal domain value if the domain was specified for the client connections (requires property
  axoniq.axonserver.experimental.allow-empty-domain set to true and axoniq.axonserver.internal-domain defined with an empty string)
- fix a timing issue in LeadershipStatusNotifier that can cause the leader to forget about its own leader status

Notes:
- For the swagger endpoint use  /swagger-ui.html or /swagger-ui/index.html.
- The generic endpoint for actuator is /actuator (/actuator/ no longer works)
- If you are using the LDAP or OATH extension you need to use the 4.6.0 version
  of these extensions as because of the Spring Boot version update

Release Notes for version 4.5.21
--------------------------------
* Fix for bloom filter index: reading aggregate events searches for older events when the last event sequence number
  is the same as the snapshot sequence number
* New property for bloom filter index, axoniq.axonserver.event.segments-for-sequence-number-check, defines the number of segments
  that Axon Server will check for events on an aggregate when an event with sequence number 0
  is stored. The default value for this property is 10.
  For performance reasons, if you increase this property to a value higher than 100 it is recommended
  to also increase the axoniq.axonserver.event.max-bloom-filters-in-memory property.


Release Notes for version 4.5.20
--------------------------------
* Fix: reading aggregate events hangs on JVM Error

Release Notes for version 4.5.19
--------------------------------
* Security update: updated control database settings

Release Notes for version 4.5.18
--------------------------------
* Performance improvements in replication process
* Index files for replication segments are no longer created
* Reduced memory consumption during transactions
* Improved handling of out-of-memory exceptions

Release Notes for version 4.5.17
--------------------------------
* Deprecated "/v1/backup/filenames" endpoint, use new endpoint /v1/backup/eventstore instead. The new endpoint returns all files
  to back up, given a last closed segment number, and it also returns the currently last closed segment.
* Endpoint "/v1/backup/log/filenames" now also returns the current replication log segment
* Fix: install snapshot for _admin replication group fails when there are plugins installed
* Health status for replication groups now also checks if follower is not too far behind leader
* Removed automatic recovery option in case a node was missing the last committed replication log entry and has newer log entries

Release Notes for version 4.5.16
--------------------------------
* Updated Spring Boot version to 2.5.12 to fix CVE-2022-22965

Release Notes for version 4.5.15
--------------------------------
* Fix: duplicated entries in users table after install snapshot
* Fix: Axon Server does not start when Dynatrace OneAgent is running

Release Notes for version 4.5.14
--------------------------------
* Fix: not able to move to leader state when there are entries that have not been applied yet after recreating a context
* Fix: option to prevent the usage of the SpringPhysicalNamingStrategy in the migration tool
* Performance improvement for reading event processor re-balance configuration per context.

Release Notes for version 4.5.13
--------------------------------
* Fix: node fails to synchronize with leader if leader became leader immediately after it had installed a snapshot
* Updated version of LDAP extension in package
* Updated gRPC version from 1.42.0 to 1.42.2 to avoid CVE-2021-22569

Release Notes for version 4.5.12
--------------------------------
* Fix: performance issue reading aggregates with first sequence number larger than last sequence number in the event store
* Fix: access controller ignores the context name in the role when using non-standard authentication/authorization
* Fix: during install snapshot for admin RG, the nodes' role was not set
* Improvement: catch exceptions during connection to other AS nodes

Release Notes for version 4.5.11
--------------------------------
* Fix: removed unnecessary index file references when loading an aggregate with up to date snapshot
* Fix: missing/double icons on plugin, users and application pages
* Fix: cluster template fails to create contexts if there are applications or users with roles for all contexts
* Improved logging on client application disconnects
* Updated gRPC and Netty versions
* Updated XStream version

Release Notes for version 4.5.10
-------------------------------
* Fix: improved data validations for contexts, replication groups, users and applications to prevent issues applying log entries
* The install snapshot is interrupted any time there is a communication problem between the two nodes.
* Improvements to avoid race conditions reserving sequence numbers in case of multiple leader elections.
* Raft group configuration changes are handled through Scheduled Task to resume after leadership changes.
* Performance improvements during install snapshot
* Improvements in performance and resources usage during replication. Reduction of unnecessary leader election.
* Fix: prospect nodes are not transformed into followers during the preemptive install snapshot.
* Fix: delete all contexts information when the install snapshot fails
* Improvements in logging
* Fix: the configuration change fails when the "add node request" is received when the node is not the leader.
* Improvements in performances for creation of new applications
* Update Felix to version 7.0.1 to support java 17
* Update JQuery to version 3.6.0
* Fix: incorrect login url when AS is invoked behind a reverse proxy
* Fix: NullPointerException in health check

Release Notes for version 4.5.9
-------------------------------
* Fix: UI issues when running with another context root
* Fix: UI does not refresh the icons for event processor streams
* Fix: Balancing processors for a processing group containing special characters does not work from the UI
* Fix: UI is not updated when the auto-balancing strategy is changed
* Fix: Warning logged when a client closes an  event stream while it is reading from old segments
* Fix: Removing a replication group fails when one or more nodes are not connected
* Fix: Concurrency issue while registering command handlers and query handlers
* Remove timing metrics for commands/queries for clients no longer connected

Release Notes for version 4.5.8
-------------------------------
* Fix: Memory leak in subscription query registrations
* Fix: Different error code and message when context not available or context not exists during client connect

Release Notes for version 4.5.7
-------------------------------
* Fix: Improve loading aggregate events performance for clients connected to a follower
* Fix: Failure in the initialization of a node in leader role now prevents the node from moving to leader role
* Fix: Message from another node with a higher term must force node into follower state if it is in leader state
* Fix: Improved error handling and feedback when uploading invalid plugins
* Fix: Increase default settings for spring.servlet.multipart.max-request-size and spring.servlet.multipart.max-file-size to 25MB

Release Notes for version 4.5.6.1
---------------------------------
* Fix: Close event store segment file when reading is complete
* Fix: In case of timeout during query execution, AS sends a timeout error to the client before canceling the query
* Fix: Queries and commands not cancelled after timeout
* Added type (Event/Snapshot) as a tag for metric indexes on JumpSkipIndex

Release Notes for version 4.5.5
-------------------------------
* Fix Micrometer exception happening in case of multiple contexts with different index type
* Update Spring Boot Dependency to version 2.4.8

Release Notes for version 4.5.4
-------------------------------
* Fix: Clients should not connect to a node that is unable to contact other nodes in the cluster
  If a client requests a connection from an isolated Axon Server node, Axon Server rejects the connection, and the client
  requests a connection from another node. If a client has a connection to an Axon Server node, and the node loses the
  connection to the other Axon Server nodes in the cluster, Axon Server will disconnect the client. The client reconnects to another
  node (also in 4.4.16).
* Fix: Regression in loading aggregate events performance
* Fix: Handle queries with same request type but different response type
* Fix: client sometimes reconnects when other Axon Server node is restarted
* Fix: clients are redirected to Axon Server node that is not ready yet
* Fix: invalid version number in login and error pages

* New metrics added:
  - file.bloom.open: counts the number of bloom filter segments opened since start
  - file.bloom.close: counts the number of bloom filter segments closed since start
  - file.segment.open: counts the number of event store segments opened since start
  - local.aggregate.segments: monitors the number of segments that were accessed for reading aggregate event requests

Notes:
 - Default value for configuration property axoniq.axonserver.event.events-per-segment-prefetch is decreased from 50 to 10.

Release Notes for version 4.5.3
-------------------------------
* Improved performance for reading aggregates
* Improvements in shutdown process
* Reduced memory usage for in memory indexes
* Reduced number of files kept open as memory mapped files
* Initialize event stores asynchronously on startup
* Fix: Load balancing operations for processors should ignore stopped instances
* Fix: Stop reading events when query deadline expires
* Fix: Disparities in Context Leaders
* Dependency update: updated xstream version used to 1.4.17

Release Notes for version 4.5.2
-----------------------------
* Configurable strategy for aggregate events stream sequence validation
* Fix UI check for updates

Release Notes for version 4.5.1
-----------------------------
* Enabled log entry store validation
* Fix invalid term/index after install snapshot procedure
* Fix concurrent update of creating schedulers for processor auto loadbalancing

Release Notes for version 4.5
-----------------------------

New features:
- Support for customer defined plugins to add custom actions to adding/reading events and snapshots and executing commands and (subscription queries)
  For more information see https://docs.axoniq.io/reference-guide/axon-server/administration/plugins.
- Authentication via Google OAuth (via extension)
- Authentication and authorization with LDAP
- Search snapshots in Axon Dashboard
- New role VIEW_CONFIGURATION allowing user read only access to the configuration of Axon Server in the Dashboard

Enhancements:
- Flow control for reading aggregates
- Updates in user permissions now have immediate effect (even if the user is logged in)
- Option to use a separate SSL key file for communication between Axon Server nodes
- Logging of illegal access to Axon Server gRPC services
- Improved monitoring of available disk space (see https://docs.axoniq.io/reference-guide/axon-server/administration/monitoring/actuator-endpoints)
- License status added to health page (see https://docs.axoniq.io/reference-guide/axon-server/administration/monitoring/actuator-endpoints)
- List of used 3rd party libraries available from Dashboard
- Migration tool now supports migrating from a MongoDB event store to Axon Server
- Axon Dashboard checks for Axon Server version updates

Dependency updates:
- updated gRPC and Netty versions
- updated Spring Boot version
- updated Swagger version

Bug fixes:
- memory leak in the replication group leader could cause elections
- incorrect version number shown in login page and error page in Dashboard
- invalid number of active subscription queries displayed in Dashboard
- exception initializing a context for the replication group leader on restart after unclean shutdown

Notes:
- Due to the update of the Spring Boot version there are some minor changes to the output of the /actuator/health endpoint.
  This endpoint now uses the element "components" instead of "details" to output the health per subcategory.

- The swagger endpoint has changed from /swagger-ui.html to /swagger-ui/.

Release Notes for version 4.4.17
--------------------------------
* Fix: Failure in initialization of node in leader role now prevents node from moving to leader role
* Fix: Message from another node with a higher term must to force node into follower state if it is in leader state

Release Notes for version 4.4.16
--------------------------------
* Fix: Clients should not connect to a node that is unable to contact other nodes in the cluster
  If a client requests a connection from an isolated Axon Server node, Axon Server rejects the connection, and the client
  requests a connection from another node. If a client has a connection to an Axon Server node, and the node loses the
  connection to the other Axon Server nodes in the cluster, Axon Server will disconnect the client. The client reconnects to another
  node.

Release Notes for version 4.4.15
-------------------------------
* Fix: Load balancing operations for processors should ignore stopped instances
* Fix: Stop reading events when query deadline expires
* Dependency update: updated xstream version used to 1.4.17

Release Notes for version 4.4.14
--------------------------------
* Configurable strategy for aggregate events stream sequence validation
* Enabled log entry store validation
* Fix invalid term/index after install snapshot procedure
* Fix concurrent update of creating schedulers for processor auto loadbalancing

Release Notes for version 4.4.13
--------------------------------
* Fix for race condition during JumpSkipIndex update
* Fix for memory leak during peer node registration in Raft leader

Release Notes for version 4.4.12
--------------------------------
* Fix for subscription queries in case of missing query handler

Release Notes for version 4.4.11
--------------------------------
* Fix for concurrency issue in listing aggregates events during appending events for the same aggregate

Release Notes for version 4.4.10
--------------------------------
* Load balancing strategy for queries is now configurable. Metrics based by default, to configure round-robin set property
  axoniq.axonserver.query-handler-selector=round-robin
* Improvement on metrics based load balancing of queries, now giving higher probabilities based on the response time
  for the handler as recorded on the current axon server node
* Fix for scheduler not rescheduling all events after a leader change

Release Notes for version 4.4.9
-------------------------------
* Improvement for subscription query: initial result are now provided by a single instance per component

Release Notes for version 4.4.8.1
---------------------------------
* Hotfix for version 4.4.8 to fix query handling

Release Notes for version 4.4.8
-------------------------------
* Fix for log compaction never performed if the backup is taken more often than each hour
* Fix in release notes for version 4.4.7
* Fix for processor information showing information on disconnected applications
* Fix for issue with null expressions in ad-hoc queries
* Updated GRPC version to 1.34.0
* Added option to limit the number of commands/queries in progress per context

Release Notes for version 4.4.7
-------------------------------
* Fix for access control issue when updating an application
* Fix for global index files remaining after delete context
* Fix for commit index not updated correctly in specific cases
* Fix for warning messages on opening index that should have been debug messages
* Fix for invalid previous term/index in replication heartbeat messages causing some overhead in communication
* Fix for missing portnumber in cluster template export
* Fix for commands being sent to wrong context if the only handler is in another context
* New property "axoniq.axonserver.enterprise.default-index-type" to specify the index type for new contexts. Default
  value is JUMP_SKIP_INDEX, use BLOOM_FILTER_INDEX for the pre-4.4 index type

Release Notes for version 4.4.6
-------------------------------
* Improved QueryService logging
* Added preserve event store option to delete context CLI command
* Fixed stream completed by the server in case of inactivity
* Hide upload license panel in SE
* Reduced number of open index files
* Fix for GetTokenAt operation
* Improved feedback on license upload errors
* Fix timing issue in leader change potentially causing duplicate events
* New REST endpoint to download a diagnostics zip file

Release Notes for version 4.4.5
-------------------------------
* Fix for connections not correctly registered
* Changed initialization sequence for event store to initialize completed segments first
* Changed order of files in the backup endpoint for contexts with a jump skip index to list the global index files
  before the segment index files
* Fix timing issue when a follower sends new events to a new leader before it is fully initialized
* Improved logging and error handling for log compaction task

Release Notes for version 4.4.4
-------------------------------
* Fix for initializing jump skip index when it has more than 1 segment
* Improved error handling for problems creating a context and storing events/snapshots
* Offload expensive data-writing operations to separate thread pool
* Fix for reading aggregates with older snapshots

Release Notes for version 4.4.3
-------------------------------
* Fix race condition in queries and commands handlers unsubscription during reconnection
* Fix pre-vote election with active backup nodes
* Axon Server SE improvements from 4.4 to 4.4.1
* Fixed issue causing added latency while Tracking live Events from a follower node
* Fix the event processor status refresh process

Release Notes for version 4.4.2
-------------------------------
* Fix for downloading and starting with cluster templates
* Renamed field replicationsGroups to replicationGroup in cluster template
* Fix storing entries when a context is created with pre-existing event store
* Fix query from dashboard when query is executed from admin node and the target context is not defined on this admin node
* Fix for timing issue in sharing metrics between Axon Server nodes causing exception during delete context

Release Notes for version 4.4.1
-------------------------------
* Fix for migration of contexts created before version 4.3

Release Notes for version 4.4
-----------------------------
* Scalability improvements:
  - tracking event processors can now read from any primary node,
  - reading aggregates will read events from followers and only request the leader for latest events
  - introduction of replication groups containing one or more contexts to reduce overhead in replication process
* Performance improvements:
  - new index type to improve speed in reading aggregates
* Usability improvements:
  - Axon Server Enterprise can now start without a license and has the option to upload licenses to the cluster
  - cluster templates to initialize a cluster including replication groups, contexts, users and applications based on a
    template
* Miscellaneous:
  - multi-tier storage to keep only recent data at primary nodes
  - Axon Server can now act as an event scheduler
  - tag-based routing of commands and queries
  - some storage properties can now be set at the context level
  - support fom token store identifiers to identify which tracking event processors share a token store

Migration notes:
Axon Server 4.4 has changes to its internal control db schema. To be able to rollback to a previous version,
it is recommended to create a backup of the control database before upgrading.
It is possible to perform a rolling upgrade to version 4.4, but during this upgrade process you should not create
new replication groups, contexts, users and applications.

Release Notes for version 4.3.7
-------------------------------
* Fixed concurrency issue in subscribing/unsubscribing commands

Release Notes for version 4.3.6
-------------------------------

* Do not override log entries in RAFT log that already have been committed

Release Notes for version 4.3.5
-------------------------------

* Fix for scheduling of tasks in clustering module
* Do not override log entries in RAFT log that already have been committed
* Fixed logging in IndexManager

Release Notes for version 4.3.4
-------------------------------

* Reduced risk for contention when opening an index file
* Offload expensive data-fetching operations to separate thread pool
* Option to configure the way that index files are opened (memory-mapped or file-channel based)
* Limit the number of commands/queries held in Axon Server waiting for the handlers to be ready to handle them, to avoid
  out of memory errors on Axon Server
* Fix for a high number of cluster-request threads being created
* Fix for timing issue in delete context. This could leave the context existing on one of the member nodes
* Fix RAFT bug: configuration changes are not allowed before an entry has been committed in the current term.

New configuration properties added for Axon Server:
axoniq.axonserver.data-fetcher-threads=24 (number of threads that are allocated for doing longer running operations on the event store)
axoniq.axonserver.command-queue-capacity-per-client=10000 (number of command requests for a specific command handling client that Axon Server will cache waiting for permits)
axoniq.axonserver.query-queue-capacity-per-client=10000 (number of query requests for a specific query handling client that Axon Server will cache waiting for permits)
axoniq.axonserver.replication.use-mmap-index=null (by default, AxonServer will determine whether to use memory mapped
                                indexes for replication logs based on operating system and java version, in rare cases it may be useful to override the default)
axoniq.axonserver.replication.force-clean-mmap-index=null (option to forcefully close unused memory mapped files instead of leaving the garbage collector do this,
                                by default, AxonServer will determine this  based on operating system and java version, in rare cases it may be useful to override the default)
axoniq.axonserver.event.use-mmap-index=null (by default, AxonServer will determine whether to use memory mapped
                                indexes for event files in the event store based on operating system and java version, in rare cases it may be useful to override the default)
axoniq.axonserver.event.force-clean-mmap-index=null (option to forcefully close unused memory mapped files instead of leaving the garbage collector do this,
                                by default, AxonServer will determine this  based on operating system and java version, in rare cases it may be useful to override the default)
axoniq.axonserver.snapshot.use-mmap-index=null (by default, AxonServer will determine whether to use memory mapped
                                indexes for snapshot files in the event store based on operating system and java version, in rare cases it may be useful to override the default)
axoniq.axonserver.snapshot.force-clean-mmap-index=null (option to forcefully close unused memory mapped files instead of leaving the garbage collector do this,
                                by default, AxonServer will determine this  based on operating system and java version, in rare cases it may be useful to override the default)

Configuration properties default values changed:

axoniq.axonserver.cluster-executor-thread-count=4 (reduced from 8, configures the number of threads used to handle requests from other axon server nodes,
                                                   reduced as effective processing of the requests if offloaded to other threads)
axoniq.axonserver.executor-thread-count=4 (reduced from 8, configures the number of threads used to handle requests from clients,
                                                   reduced as effective processing of the requests if offloaded to other threads)


Release Notes for version 4.3.3
-------------------------------

* Fix for race condition during rollback of transaction log

Release Notes for version 4.3.2
-------------------------------

* Fix for tracking event processor updates to websocket causing high CPU load in specific situation
* Reduced warnings in log file on clients disconnecting
* Fix for concurrency issue in sending heartbeat while client connects/disconnects

Release Notes for version 4.3.1
-------------------------------

* Updated usage output in CLI
* Updated gRPC/Netty versions
* Prevent errors in log (sending ad-hoc result to client that has gone, sending heartbeat to client that has gone)
* Fix in CLI, application list failed
* Removed retry option from H2 controldb URL
* Fixed Axon Server keeping incorrect application references on restart of Axon Server node where application was connected
* Fixed race condition when two Axon Server nodes are trying to become member of the same context at the same time
* Fix for some REST reads were able with invalid token
* Clean up of server shutdown process
* Fixed error when adding a non-existing node to a context


Release Notes for version 4.3
-------------------------------

* Introduced new roles for nodes in context (ACTIVE_BACKUP, PASSIVE_BACKUP and MESSAGING_ONLY)
* Improved support for running in containers
* New option to configure cluster information in configuration files, to automatically build cluster at startup
* Support load factor for commands
* Support for moving Axon Server cluster to other nodes
* Option to remove a node from a context without deleting the event store on that node
* Separate audit log for configuration changes
* Changed metrics to use common names and tags


Changes in Axon Server 4.2.6
----------------------------

- Rollback in log entry could cause cluster member to transition to fatal state when there were writes pending

Changes in Axon Server 4.2.5
----------------------------

- Fix for start-up issue when management.server.port is set

Changes in Axon Server 4.2.4
----------------------------

- Added instruction acknowledgements
- Client applications heartbeat support
- Cleaned-up logging
- Fix for specific error while reading aggregate
- Optional heartbeat between Axon Server and Axon Framework clients

Changes in Axon Server 4.2.3
----------------------------

- Fix for issue on cancelling queries on lost connection
- Introduced acknowlegments on instructions between Axon Server and clients

Changes in Axon Server 4.2.2
----------------------------

- Fix for continuously checking tracking event processor status
- Fix for issue in access control: combining roles for all contexts and single context

Changes in Axon Server 4.2.1
----------------------------

- Fix for issue in access control: applications with access to multiple contexts are not always correctly validated
- Fix for UI issue: after deleting an application the buttons no longer work
- On auto-load-balance of a event processor group only request the current status from applications with this processor group

Changes in Axon Server 4.2
--------------------------

- Delete leader from group is now possible.
- Removing a context from a node, deletes all data (including event data) on that node.
- Deleting a context removes all data (including event data).
- Blacklisting event types for applications that cannot handle these events (requires AxonFramework v4.2.)
- Expose tracking event processor position and status  (requires AxonFramework v4.2.)
- Clients can provide preferences on which node to connect to  (requires AxonFramework v4.2.)
- New access control roles
- Reduced leader changes by new pre-vote phase
- Improved health page

Changes in Axon Server 4.1.9
----------------------------

- Fix for situation where election takes too long to complete causes 2 leaders
- Wait for leader when event store requests arrive at Axon Server during leader election. New configuration property
  axoniq.axonserver.replication.wait-for-leader-timeout to change this wait time. Default is 2*max election timeout.

Changes in Axon Server 4.1.8
----------------------------

- Fix for unexpected aggregate sequence exception.
- Fix for wrong version number in dashboard.

Changes in Axon Server 4.1.7
----------------------------

- Fix for timing issue when new events arrived at new leader before leader was fully initialized
- Fix for configuration not always applied on all nodes
- Fix for OutOfDirectMemory issue when installing snapshot

Changes in Axon Server 4.1.6
----------------------------

- Fix for unexpected leader change on adding 3rd node to context
- Fix for replication entry log compaction

Changes in Axon Server 4.1.5
----------------------------

- Fix for mismatch in connected applications after rebalance
- Fixes in access control check on REST endpoints for events/snapshot endpoints and renew application token
- Improved error handling in replication
- Fixed occasional timing issue in create context
- Improved recovery options after full data loss on node
- Fix for authorization path mapping
- Fix for subscription query memory leak
- Improvements in error reporting in case of disconnected applications
- Improvements in detection of insufficient disk space

Changes in Axon Server 4.1.4
----------------------------

- Fix on RAFT Leader detection
- Avoided multiple concurrent creation of the same context
- Fix for appendEvent with no events in stream
- Stop log cleaning task when context is deleted or node is removed from context
- Fix for NullPointerException on lost connection


Changes in Axon Server 4.1.3
----------------------------

- Improved recovery of cluster after disk failure
- Fix for updating configuration during install snapshot
- CLI commands now can be performed locally without token.

Changes in Axon Server 4.1.2
-------------------------------

- Improvements in replication to nodes that have been down for a long time
- Tracking event processor auto-loadbalancing fixes
- Status displayed for tracking event processors fixed when segments are running in different applications
- Tracking event processors are updated in separate thread
- Logging does not show application data anymore
- Fixed Axon Dashboard list of cluster nodes missing hostname and ports
- Changed some gRPC error codes returned to avoid clients to disconnect
- CLI commands init-cluster/register-node/add-node-to-context are now idempotent
- Register node returns faster (no longer waits for synchronization to new node)
- Reduced risk of re-election if a node restarts
- Fixed occasional NullPointerException when client connects to a newly registered Axon Server node
- Fixed incorrect leader returned in context API and multiple leaders warning in log


Changes in Axon Server 4.1.1
----------------------------

- Default controldb connection settings changed
- gRPC version update
- Register node no longer needs to be sent to leader of _admin context
- Merge tracking event processor not always available when it should
- Logging changes
- Fix for queries timeout
- Fix for replication with large messages
- Added axonserver-cli.jar to release package (axoniq-cli.jar is deprecated)


Changes in Axon Server 4.1
--------------------------

- Support for Split/Merge of tracking event processors through user interface
- Introduction of _admin context for all cluster management operations
- Updated process for getting started (see above)
- Replication of data and configuration between nodes in the cluster is now based on transaction log replication.
  You will see new files created on AxonServer nodes in the log directory (axoniq.axonserver.replication.log-storage-folder).
  Do not delete those files manually!
- Default setting for health endpoint (/actuator/heath) has changed to show more details.
- Change in TLS configuration for communication between AxonServer nodes (new property axoniq.axonserver.ssl.internal-trust-manager-file)


Configuring Axon Server
=======================

Axon Server uses sensible defaults for all of its settings, so it will actually
run fine without any further configuration. However, if you want to make some
changes, below are the most common options. You can change them using an
"axonserver.properties" file in the directory where you run Axon Server. For the
full list, see the Reference Guide. https://docs.axoniq.io/reference-guide/axon-server

* axoniq.axonserver.name
  This is the name Axon Server uses for itself. The default is to use the
  hostname.
* axoniq.axonserver.hostname
  This is the hostname clients will use to connect to the server. Note that
  an IP address can be used if the name cannot be resolved through DNS.
  The default value is the actual hostname reported by the OS.
* server.port
  This is the port where Axon Server will listen for HTTP requests,
  by default 8024.
* axoniq.axonserver.port
  This is the port where Axon Server will listen for gRPC requests,
  by default 8124.
* axoniq.axonserver.internal-port
  This is the port where Axon Server will listen for gRPC requests from other AxonServer nodes,
  by default 8224.
* axoniq.axonserver.event.storage
  This setting determines where event messages are stored, so make sure there
  is enough diskspace here. Losing this data means losing your Events-sourced
  Aggregates' state! Conversely, if you want a quick way to start from scratch,
  here's where to clean.
* axoniq.axonserver.snapshot.storage
  This setting determines where aggregate snapshots are stored.
* axoniq.axonserver.controldb-path
  This setting determines where Axon Server stores its configuration information.
  Losing this data will affect Axon Server's ability to determine which
  applications are connected, and what types of messages they are interested
  in.
* axoniq.axonserver.replication.log-storage-folder
  This setting determines where the replication logfiles are stored.
* axoniq.axonserver.accesscontrol.enabled
  Setting this to true will require clients to pass a token.

The Axon Server HTTP server
===========================

Axon Server provides two servers; one serving HTTP requests, the other gRPC.
By default these use ports 8024 and 8124 respectively, but you can change
these in the settings as described above.

The HTTP server has in its root context a management Web GUI, a health
indicator is available at "/actuator/health", and the REST API at "/v1'. The
API's Swagger endpoint finally, is available at "/swagger-ui/", and gives
the documentation on the REST API.
