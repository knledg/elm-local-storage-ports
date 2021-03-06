# Elm LocalStorage Ports [![Build Status](https://travis-ci.org/paulstatezny/elm-local-storage-ports.svg?branch=master)](https://travis-ci.org/paulstatezny/elm-local-storage-ports)

Interface with LocalStorage in Elm.

## Quick Start

### 1. Install via NPM

```
$ npm install --save elm-local-storage-ports
```

### 2. In `elm-package.json`, import [`Ports/LocalStorage.elm`](lib/elm/Ports/LocalStorage.elm)

Add `node_modules/elm-local-storage-ports/lib/elm` to your `source-directories`:

```js
// elm-package.json

{
    // ...

    "source-directories": [
        "../../node_modules/elm-local-storage-ports/lib/elm", // Exact path to node_modules may be different for you
        "./"
    ],

    // ...
}
```

### 3. Use it in your Elm code

```elm
type Msg
  = SaveSearch String
  | RequestLastSearch
  | ReceiveFromLocalStorage (String, Json.Decode.Value)


subscriptions : Model -> Sub Msg
subscriptions model =
  Ports.LocalStorage.storageGetItemResponse ReceiveFromLocalStorage


update : Msg -> Model -> (Model, Cmd Msg)
update msg model =
  case msg of
    SaveSearch searchQuery ->
      ( model
      , Ports.LocalStorage.storageSetItem ("lastSearch", Json.Encode.string searchQuery)
      )

    RequestLastSearch ->
      (model, Ports.LocalStorage.storageGetItem "lastSearch")

    ReceiveFromLocalStorage ("lastSearch", value) ->
      case JD.decodeValue JD.string value of
        Ok searchQuery ->
          -- Do something with searchQuery
```

### 4. Register your Elm app in JavaScript

#### Using [Elm Router](https://github.com/paulstatezny/elm-router)

```javascript
var localStoragePorts = require('elm-local-storage-ports');
elmRouter.start(Elm, [localStoragePorts]);
```

#### Without Elm Router

```javascript
var localStoragePorts = require("elm-local-storage-ports");
var myElmApp = Elm.MyElmApp.embed(document.getElementById("my-elm-app-container"));

localStoragePorts.register(myElmApp.ports);
```

## API Reference

[View the full API reference here.](./API.md)

## Questions or Problems?

Feel free to create an issue in the [GitHub issue tracker](https://github.com/paulstatezny/elm-local-storage-ports/issues).
