This page is intended to provide insight into what terminology we use in this documentation. Due to the more complex nature of Bezerk's terminology and functionality, this is dedicated to its own page. **We advise you to read this before you read the rest of the docs to understand better!**

- **Bezerk:** The WildBeast websocket manager. Often abbreviated to WSM.
- **Shard:** Partial WildBeast instance. See [Sharding](http://docs.thesharks.xyz/sharding/) in the WildBeast documentation for more information.
- **Websocket:** The connection "tunnel" between a WildBeast shard and the WSM.
- **OP code:** A code which a shard will emit when opening the websocket connection for identification and when sending messages.
- **Listener:** A logger listening for events from Bezerk. Has a list of subscriptions.
- **Subscription:** A list of events to listen for from Bezerk.