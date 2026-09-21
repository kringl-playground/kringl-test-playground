# How a Monday item opens work here

This repo is watched through the Monday connector. When an item is created (or updated) on
one of this repo's boards, the connector opens a Kringl session for it automatically - nobody
has to start it by hand.

## The boards

- **Bugs Queue** (#5104608238), group **Incoming Bugs**: bug reports land here.
- **Tasks** (#5104608234): regular work items land here.

## What happens when an item is opened

1. Someone (or an automation) creates an item on one of the boards above, in a watched group.
2. The connector picks up the item and opens a new Kringl session for it.
3. The item's fields become the ticket: the item name is the request, the board/group tell the
   session which queue it came from, and the item id (`item_id`) ties the session back to that
   exact Monday item.
4. The session carries the item under `external` in its metadata, so it always knows which
   Monday item it is working for.
5. The session works the ticket end to end: investigate, change, deploy (when this project has
   a deploy target), open a PR, merge it.
6. When the session finishes (or needs a person), it does not post to Monday itself: the
   platform posts the session's summary as an update on the original item. Same for a question
   back to a person, it lands as an update on the item, not as a message the session sends.

## What a session should not do

A session should not create or change Monday items on its own initiative. The `monday` MCP is
available for reading boards, items and updates freely; creating or changing an item is only
for when a person explicitly asked for it in the conversation.

Writing an update on an item that already has a session does not open a new one: it resumes the existing conversation with that update as the next message.
