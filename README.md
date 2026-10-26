# Cat Cat Bat Bat

A mobile-first multiplayer chit-passing game that runs in a web browser and can be packaged as an Android APK.

## Features

- Create or join a five-letter room for **3–8 players**.
- Pass chits to the next player and be the first to collect a matching set.
- The winning set is five of a kind for 3–4 players, or four of a kind for 5–8 players.
- Host-authoritative game engine: the room creator's browser runs the game rules and sends each player their view.
- In-game room chat.
- Friends list with friend IDs, requests, direct messages, and room invitations.
- Direct messages use the Web Crypto API with ECDH key agreement and AES-GCM encryption.
- Peer-to-peer connections use WebRTC through Trystero; public Nostr relays are used for peer discovery.

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
| `docs/screenshot1.png` | Opening menu with the create/join room controls |
| `docs/screenshot2.png` | Room lobby showing the room code and players |
| `docs/screenshot3.png` | Active game showing the table and a player's chits |
| `docs/screenshot4.png` | Friends screen or final results screen |

See [docs/SCREENSHOTS.md](docs/SCREENSHOTS.md) for the checklist. The screenshots are placeholders until you add the image files.

## Run locally

### Run the browser game

The game uses JavaScript modules, Web Crypto, and a CDN import. Run it from `localhost` or HTTPS rather than opening the HTML as a `file://` URL.

1. Install Python 3 if it is not already installed.
2. Clone or download this repository.
3. Open a terminal in the repository folder.
4. Start a local static web server:

   ```bash
   python -m http.server 8000
   ```

   On some Windows installations, use `py -m http.server 8000`.
5. Open [http://localhost:8000/src/](http://localhost:8000/src/) in your browser and select `index.html` if the directory listing appears, or go directly to [http://localhost:8000/src/index.html](http://localhost:8000/src/index.html).
6. Allow the page to load its Trystero dependency from the internet.

For multiplayer testing, open the game in separate browser profiles/devices and create or join the same room code. Peer connectivity depends on browser/network conditions; the current source sets `TURN = []`, so no TURN relay is configured.

### Android APK

The repository no longer stores APK binaries in its root. Download a packaged APK from the [GitHub Releases page](https://github.com/binyameen212/cat-cat-bat-bat/releases) once a release has been published.

## How it works

The room creator's browser creates an `Engine` instance and owns the authoritative game state. Other players send actions, such as joining or passing a chit, to the host. The host applies the rules and sends each player a tailored state view. Trystero sets up peer-to-peer connections using WebRTC; public Nostr relays are used for discovery rather than as a dedicated game server.

The Friends feature is a separate browser-side module in the same HTML file. It stores a local identity, contacts, and chats in `localStorage`; friend identities use ECDH P-256 public keys, and direct messages are encrypted with AES-GCM. Friend messaging requires peers to be reachable at the same time for delivery; queued messages remain in local storage until they can be sent.

## Known limitations

- No TURN server is configured by default. Some NATs, firewalls, or restricted networks may prevent peer-to-peer connections.
- The app depends on internet access for the Trystero CDN import, peer discovery, and the external font.
- Browser storage is local to the device/browser. Clearing site data can remove saved friends, identity, and chats.
- The source does not include an automated test suite or a reproducible Android packaging/build setup.
- The repository's APK release workflow and Android-device compatibility have not been independently verified here.
- Screenshots still need to be captured from the running game.

## License

Licensed under the [MIT License](LICENSE), copyright © 2026 Yameen. Confirm that you have permission to distribute any third-party libraries and assets included with the project.
