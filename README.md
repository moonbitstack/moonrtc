# moonrtc

WebRTC is not one protocol but a stack of them. This is that stack, with no
socket in it.

> **Status: planned.** The repository is set up; nothing is
> implemented yet.

Each layer is a package, each package takes bytes and hands back events, and
whoever calls them owns the UDP socket and the event loop. Both sides of every
layer are in scope — this answers a call as readily as it places one.

| Package | Layer | Specification |
|:--|:--|:--|
| `sdp` | Session descriptions, the text both ends exchange to agree on media | [RFC 8866](https://www.rfc-editor.org/rfc/rfc8866) |
| `stun` | Binding requests: what address the far side actually sees | [RFC 8489](https://www.rfc-editor.org/rfc/rfc8489) |
| `turn` | Relaying, for when neither side can be reached directly | [RFC 8656](https://www.rfc-editor.org/rfc/rfc8656) |
| `ice` | Candidate gathering, pairing and the connectivity check state machine | [RFC 8445](https://www.rfc-editor.org/rfc/rfc8445) |
| `rtp` | Media packets and their sequencing | [RFC 3550](https://www.rfc-editor.org/rfc/rfc3550) |
| `rtcp` | The reports that drive retransmission and rate control | [RFC 3550](https://www.rfc-editor.org/rfc/rfc3550) |
| `srtp` | Encrypting those packets, with the ciphers taken from `mooncrypt` | [RFC 3711](https://www.rfc-editor.org/rfc/rfc3711) |
| `sctp` | The data channel: an association carried over DTLS | [RFC 8831](https://www.rfc-editor.org/rfc/rfc8831), [RFC 4960](https://www.rfc-editor.org/rfc/rfc4960) |

## What is deliberately elsewhere

| Thing | Where it lives | Why |
|:--|:--|:--|
| DTLS handshake | [`moontls`](https://github.com/moonbitstack/moontls) | DTLS is TLS over datagrams; a second implementation would be a second thing to get wrong |
| Ciphers and key derivation | [`mooncrypt`](https://github.com/moonbitstack/mooncrypt) | One algorithm to a package, used by everything here |
| Opening the socket, running the loop | the server or the application | Every package here is I/O-free, which is what makes it testable |
| Playlists, containers, ingest | [`moonmedia`](https://github.com/moonbitstack/moonmedia) | Segmented delivery is a different problem from a peer connection |
| Audio and video codecs | nowhere — out of scope | This library carries media; it does not encode it |

## Install

```bash
moon add moonbitstack/moonrtc
```

## Licence

Apache-2.0. See [LICENSE](LICENSE).
