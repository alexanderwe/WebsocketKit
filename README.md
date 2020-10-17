# WebsocketKit

<p align="center">
   <a href="https://developer.apple.com/swift/">
      <img src="https://img.shields.io/badge/Swift-5.0-orange.svg?style=flat" alt="Swift 5.0">
   </a>
   <a href="https://github.com/apple/swift-package-manager">
      <img src="https://img.shields.io/badge/Swift%20Package%20Manager-compatible-brightgreen.svg" alt="SPM">
   </a>

   <a href="https://github.com/alexanderwe/LoggingKit">
      <img src="https://github.com/alexanderwe/WebsocketKit/workflows/CI/badge.svg" alt="CI">
   </a>   
</p>

<p align="center">
LoggingKit is a small wrapper around the `Network` framework to work with websocket connections
</p>

## Installation

### Swift Package Manager

To integrate using Apple's [Swift Package Manager](https://swift.org/package-manager/), add the following as a dependency to your `Package.swift`:

```swift
dependencies: [
    .package(url: "https://github.com/alexanderwe/WebsocketKit.git", from: "1.0.0")
]
```

Alternatively navigate to your Xcode project, select `Swift Packages` and click the `+` icon to search for `LoggingKit`.

### Manually

If you prefer not to use any of the aforementioned dependency managers, you can integrate LoggingKit into your project manually. Simply drag the `Sources` Folder into your Xcode project.

## Usage

At first import `WebsocketKit`

```swift
import WebsocketKit
```

Define a `Websocket` instance

```swift
let websocket = Websocket(url: URL(string: "wss://echo.websocket.org")!)
```

It also makes sense to create a instance of a class that conforms to the `WebsocketConnectionDelegate` in order to receive websocket events

```swift
class WebsocketDelegate: WebSocketConnectionDelegate {

    func webSocketDidConnect(connection: WebSocketConnection) {
        print("Websocket did connect")
    }

    func websocketDidPrepare(connection: WebSocketConnection) {
        print("Websocket did prepare")
    }

    func webSocketDidDisconnect(connection: WebSocketConnection, closeCode: NWProtocolWebSocket.CloseCode, reason: Data?) {
        print("Websocket did disconnect")
    }

    func websocketDidCancel(connection: WebSocketConnection) {
        print("Websocket did cancel")
    }

    func webSocketDidReceiveError(connection: WebSocketConnection, error: Error) {
        print("Websocket did receive error")
    }

    func webSocketDidReceivePong(connection: WebSocketConnection) {
        print("Websocket did receive pong")
    }

    func webSocketDidReceiveMessage(connection: WebSocketConnection, string: String) {
        print("Websocket did receive string message")
    }

    func webSocketDidReceiveMessage(connection: WebSocketConnection, data: Data) {
        print("Websocket did receive data message")
    }
}
```

Set an instance of the delegate instance to the `Websocket` instance and start listenting for events

```swift
let delegate = WebsocketDelegate()
websocket.delegate = delegate

websocket.connect() // Connects to the url specified in the initializer and listens for messages
```

### Custom headers

Often it is necessary to attach custom headers to the connection. You can do so by specifying them in the initializer of the `Websocket` class.

```swift
let websocket = Websocket(url: URL(string: "wss://echo.websocket.org")!,
                          additionalHeaders: [
                            "Authorization:": "Bearer <someToken>",
                            "My-Custom-Header-Key:": "My-Custom-Header-Value"
                          ]
)
```

## Contributing

Contributions are very welcome 🙌

## License

```
LoggingKit
Copyright (c) 2020 Alexander Weiß

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
```
