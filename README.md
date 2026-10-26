# Cat Cat Bat Bat

**Cat Cat Bat Bat** is a mobile-first multiplayer chit-passing game playable in a modern web browser. Android users can use the APK; Windows, iPhone/iPad, and other desktop/mobile users can use the browser edition.

## Play the game

### Windows, macOS, Linux, iPhone/iPad, and other devices

**Use the browser edition:** [Open the game source — `index.html`](index.html).

The game needs to be served over **HTTPS or localhost** because it uses JavaScript modules and the Web Crypto API. Downloading the HTML and opening it directly as a `file://` page may not work. To run it from your own computer:

1. Download or clone this repository.
2. Install Python 3 if needed.
3. Open a terminal in the downloaded repository folder.
4. Start a local web server:
   ```bash
   python -m http.server 8000
   ```
   On Windows, `py -m http.server 8000` also works.
5. Open [http://localhost:8000](http://localhost:8000) in your browser.

For playing with friends on different devices, the game must be served from a publicly reachable HTTPS website. Once GitHub Pages or another HTTPS host is enabled, share that website URL with your friends. **The source file link above is not itself a hosted playable game.**

### Android

Android users can install the APK from the [GitHub Releases page](https://github.com/binyameen212/cat-cat-bat-bat/releases) once a release is published. The APK is intentionally not stored in the repository root.

## Features

- Create or join a five-letter room for **3–8 players**.
- Pass chits to the next player and be the first to collect a matching set.
- The winning set is five of a kind for 3–4 players, or four of a kind for 5–8 players.
- Host-authoritative game engine: the room creator's browser runs the game rules and sends each player their view.
- In-game room chat.
- Friends list with friend IDs, requests, direct messages, and room invitations.
- Direct messages use the Web Crypto API with ECDH key agreement and AES-GCM encryption.
- Peer-to-peer connections use WebRTC through Trystero; public Nostr relays are used for peer discovery.

## Supported platforms

- **Windows:** use a current Chrome, Edge, or Firefox browser.
- **iOS / iPadOS:** use a current Safari browser. For local development, the game must be served over HTTPS or localhost.
- **Android:** use a modern browser or the APK release.
- **macOS / Linux:** use a current browser.

Browser/device compatibility and multiplayer connectivity have not been tested across every platform. Some networks may block direct peer-to-peer connections.

## Tech stack

- HTML5, CSS, and JavaScript modules
- [Trystero 0.25.4](https://github.com/dmotz/trystero) for peer-to-peer room connections
- WebRTC for browser-to-browser data channels
- Web Crypto API for friend-message encryption and identity keys
- `localStorage` for local friend, identity, and chat data
- Google Fonts (Great Vibes) for the handwritten-style title and chits

## Screenshots

Add real screenshots in `docs/` and use these filenames:

| File | Capture |
| --- | --- |
| `docs/screenshot1.png` | Opening menu with create/join controls |
| `docs/screenshot2.png` | Room lobby showing the room code and players |
| `docs/screenshot3.png` | Active gameplay showing the table and a player's chits |
| `docs/screenshot4.png` | Friends screen or final results screen |

See [docs/SCREENSHOTS.md](docs/SCREENSHOTS.md) for the checklist. The screenshot images have not been added yet.

## How it works

The room creator's browser creates an `Engine` instance and owns the authoritative game state. Other players send actions, such as joining or passing a chit, to the host. The host applies the rules and sends each player a tailored state view. Trystero sets up peer-to-peer connections using WebRTC; public Nostr relays are used for discovery rather than as a dedicated game server.

The Friends feature stores a local identity, contacts, and chats in `localStorage`. Friend identities use ECDH P-256 public keys, and direct messages are encrypted with AES-GCM. Friend messaging requires peers to be reachable for delivery; queued messages remain in local storage until they can be sent.

## Known limitations

- No TURN server is configured by default. Some NATs, firewalls, or restricted networks may prevent peer-to-peer connections.
- The game depends on internet access for the Trystero CDN import, peer discovery, and the external font.
- Browser storage is local to each device/browser. Clearing site data can remove saved friends, identity, and chats.
- Directly opening the downloaded HTML as a `file://` URL may fail; use localhost or an HTTPS website.
- A public hosted browser URL is not configured by this repository update. Enable GitHub Pages or deploy the HTML to an HTTPS static host before sharing a playable link.
- The source does not include an automated test suite or a reproducible Android packaging/build setup.
- Screenshots still need to be captured from the running game.

## License

Licensed under the [MIT License](LICENSE), copyright © 2026 Yameen. Confirm that you have permission to distribute any third-party libraries and assets included with the project.
