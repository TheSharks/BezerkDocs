This page is intended to provide insight into what terminology we use in this documentation. Due to the more complex nature of Bezerk's terminology and functionality, this is dedicated to its own page. **We advise you to read this before you read the rest of the docs to understand better!**

- **Bezerk:** The WildBeast websocket manager. Often abbreviated to WSM.
- **Shard:** Partial WildBeast instance. See [Sharding](http://docs.thesharks.xyz/sharding/) in the WildBeast documentation for more information.
- **Websocket:** The connection "tunnel" between a WildBeast shard and the WSM.
- **OP code:** A code which a shard will emit when an event happens. Client and server side codes work differently. See below.
- **Listener:** A logger listening for events from Bezerk. Has a list of subscriptions.
- **Subscription:** A list of events to listen for from Bezerk and hence the shards.

##OP codes
Certain OP codes will be emitted when WildBeast and Bezerk operate together. Here is a list of them.

Client-side identification emissions:

- **IDENTIFY_SHARD:** Emitted when a socket connection is attempted from a shard. Payload: `[shardcount, shardid]`
- **IDENTIFY_LISTENER:** Emitted when a socket connection is attempted from a listener. Payload: `["event1", "event2" (etc.)]`

Server-side emissions:

- **OK:** Emitted when the `IDENTIFY_SHARD` or `IDENTIFY_LISTENER` process passes successfully.
- **HELLO:** Emitted when the websocket connection is opened, used for triggering `IDENTIFY_SHARD` and `IDENTIFY_LISTENER`.
- **EVAL_REPLY** Emitted when a listener requested an evaluation, this event must be subscribed to in order to receive it.
- **ERROR** Emitted when something unexpected happened, this event must be subscribed to in order to receive it.
