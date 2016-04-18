```
                                                          +--------------------------+
                                                          |  Router                  |
                                                          |  (haproxy)               |
                                                          |                          |
                                                          | syslog + Kibana + ES API +--------------------------------------+
                                                          |                          +-------+                              |
    +-------------+---------------------------------------+--------------------------+       |                              |
    |             |                                                                          |                              |
    |             |                                                                          |                              |
    |             |                                                                          |                              |
    |             |                                                                          |                              |
    |             |                                                                          ^                              |
    |             |                                                                 +--------+--------+                     |
    |             |                                                                 |                 |                     |
    |             |                                                                 |                 |                     |
    |             |                                                                 |   Datastore     +                     |
    |             |                                                                 | (Elasticsearch)                       |
    |             |                                      +--------------+           |                 +^-------------+      |
    |             |                                +----->  Parser      +----------->                 |              |      |
    |             |                                |     |  (logstash)  |           |                 |              |      |
    |             |                                |     +--------------+           |                 |              |      |
    |       +-----v--------+                       |                                |                 |           +--+------v-----+
    |       |  Ingestor    +----------------+      |     +--------------+           +-----------------+           |      UI       |
    |       |  (logstash)  |                ^      +----^+ Parser       |                                         |   (Kibana)    |
    |       +--------------+            +---+-------+    | (logstash)   ++          +-----------------+           |               |
    |                                   | Queue     |    +---------------+          |                 |           +----+----------+
    |                                   | (redis)   |                    +---------^+                 |                |
    |       +--------------+            +---+------++    +--------------+           |   Datastore     |                |
    +------->  Ingestor    |                ^      |     |  Parser      |           | (Elasticsearch) |                |
            |  (logstash)  +----------------+      +----->  (logstash)  ++          |                 <----------------+
            +--------------+                       |     +-------------------------^+                 |
                                                   |                                |                 |
                                                   |     +--------------+           |                 |
                                                   |     |  Parser      |           +-----------------+
                                                   +----->  (logstash)  ++
                                                         +---------------+          +------------------+
                                                                         |          |                  |
                                                                         |          |   Datastore      |
                                                                         +----------> (Elasticsearch)  |
                                                                                    |                  |
                                                                                    |                  |
                                                                                    |                  |
                                                                                    |                  |
                                                                                    |                  |
                                                                                    +------------------+



+                                                                         + +                               + +                             +
|                      Ingestion pipeline                                 | |         Datastore             | |        UI and API           |
+-------------------------------------------------------------------------+ +-------------------------------+ +-----------------------------+

```