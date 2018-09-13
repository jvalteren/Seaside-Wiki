To set the path, implement `#updateUrl:` in your components. This will get passed a `WAUrl` instance, which responds to `#addToPath:` and `#addParameter:value:`. Each active component gets a chance to modify it before every redirect and response.

To initialize your app, implement `initialRequest:`. This will be sent at the beginning of the session, and gets passed a `WARequest` instance. Typically you will want to use the information in the request to construct the root component.

See `WABrowser` for an example.

An other approach is to use `WARestfulComponentFilter`.