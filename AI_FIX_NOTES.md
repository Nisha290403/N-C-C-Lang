# AI Fix Notes

Session: seq-1777375634873-uwr3o9mnm
Repository: Nisha290403/N-C-C-Lang

- [1] (critical) C-C--master/client_server/tcp_full_duplex_server.c: Network server implementation uses a fork-based infinite loop for full-duplex communication. This pattern can easily become vulnerable to resource exhaustion, zombie processes, and uncontrolled child-process growth if children are not reaped correctly. Add explicit signal handling, process cleanup, and connection/command limits.
- [2] (critical) C-C--master/data_structures/binary_trees/red_black_tree.c: newNode allocates memory and initializes fields but does not return the created Node pointer. This is a major bug that will lead to undefined behavior at call sites and likely crashes.
- [3] (critical) C-C--master/developer_tools/malloc_dbg.c: Custom memory-debug wrappers around malloc/calloc/free are high-risk if they override standard allocation macros globally. Incorrect bookkeeping can cause double-free, use-after-free, or pointer metadata corruption. Audit all macro redefinitions and ensure wrappers preserve exact semantics under failure.
- [4] (high) C-C--master/client_server/tcp_full_duplex_client.c: The design description mentions fork-based full-duplex communication and infinite loops. In C socket programs this commonly introduces risk of zombie processes, descriptor leaks, uncontrolled blocking, and denial-of-service if peer disconnects are not handled. Verify all send/recv paths check errors, close unused descriptors in both parent and child, and handle SIGCHLD or process termination correctly.
- [5] (high) C-C--master/client_server/tcp_full_duplex_server.c: Socket-based code likely performs unbounded reads/writes over the network without visible input validation or authentication. In a TCP server, this can lead to denial of service, message injection, and unsafe parsing. Validate all received data, enforce message length limits, and avoid assuming null-terminated strings from socket buffers.

