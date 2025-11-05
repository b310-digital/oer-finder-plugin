# OER-Finder-Plugin

## Data Model and Concept

To support OER in Nostr, a special Nostr event was created, see https://github.com/edufeed-org/nips/blob/edufeed-amb/edufeed.md . Leveraging this event type, created OER can be broadcasted through the network. The NIP builds upon the AMB Data Model, see https://dini-ag-kim.github.io/amb/latest/
The data model mostly defines OER metadata. Specific file meta data is not included, which is why another event type will need to be used to also include image specific metadata. Therefore, two events will be used in combination:

1) The new EduFeed-Metadata Event (kind 30142)
2) NIP-94 for file metadata (kind 1063)

In practice, this means two event types get broadcasted through the network per OER (e.g. image resource). The system will need to listen at least to event type 30142 AND 1063 (NIP-94), not only filtering out kind 30142 as this would miss the needed file metadata event. Images are never directly broadcasted, but only referenced by URL.

### Nostr Event Examples for an Image

AMB Metadata Event:
```
{
  "kind": 30142,
  "id": "...",
  "pubkey": "...",
  "created_at": ...,
  "tags": [
    ["d", ""],
    ["type", "LearningResource"],
    ["type", "Image"],
    ["name", "Bug"],
    ["description", "A big bug"],
    ["dateCreated", "2025-11-03"],
    ["datePublished", "2025-11-11"],
    ["learningResourceType:id", "???"],
    ["learningResourceType:prefLabel", "Image"],
    ["inLanguage", "en"],
    ["license:id", "https://creativecommons.org/licenses/by-sa/4.0/"],
    ["isAccessibleForFree", "true"],
    ...
  ],
  "content": "",
  "sig": "..."
}
```

File Meta Data Event
```
{
  "kind": 1063,
  "tags": [
    ["url","https://link-to-image"],
    ["m", "image/jpeg"],
    ["size", "23995858"],
    ["dim", "1092"],
    ["summary", "A bug"],
    ["alt", "A bug"],
    ["e", "<event-id-of-amb-event>", "<relay-hint>"]
    ...
  ],
  "content": "...",
  // other fields...
}
```


