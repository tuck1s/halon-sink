# Halon - simple SMTP sink

An SMTP sink that you can harmlessly send messages to.
The `to` and `from` addresses can be anything you wish, as the messages are not relayed to any external destination.

The following examples use `swaks` to show how to get certain responses from the sink. Any other source of email could be used to submit messages to the sink.

|Port|Purpose|
|--|--|
|25|queue_to_sink listener. Passes messages through the MTA to show stats, delivering to the sink. Always accepts messages.|
|587|Email submission that will be delivered to real MXs|
|2525|Direct to the sink - gives variety of responses|


## Default behavior - message accepted (2xx) response
```
swaks --server 127.0.0.1:2525 --from any.from@example.com --to any@example.com
```
You will see a response like
```
250 2.0.0 Sink quietly accepts your message.
```

## To get a deferral (4xx) response

Add an `X-Bounce-Me` header specifying the type of response you want to get back.

```
swaks --server 127.0.0.1:2525 --from any.from@example.com --to any@example.com --add-header "X-Bounce-Me: 499 4.2.1 The user you are trying to contact is receiving mail at a rate that prevents additional messages from being delivered."
```
You can skip the enhanced code `x.y.z`, you will receive a standard `4.3.2` enhanced code in the server response.

```
swaks --server 127.0.0.1:2525 --from any.from@example.com --to any@example.com --add-header "X-Bounce-Me: 432 This is brief text"
```

You can omit the descriptive text. You will receive a standard response text.
```
swaks --server 127.0.0.1:2525 --from any.from@example.com --to any@example.com --add-header "X-Bounce-Me: 499"
```
## To get a rejection (5xx) response

It's the same, just specify 5xx values in your header, e.g.
```
swaks --server 127.0.0.1:2525 --from any.from@example.com --to any@example.com --add-header "X-Bounce-Me: 511 5.1.1 Bad email address"
```

## Troubleshooting

If an `X-Bounce-Me` header is included, but not in a recognized format, you will receive a response:

```
250 2.0.0 Sink accepts your message, X-Bounce-Me header did not ask for a bounce or deferral.
```

___
> The rest of this section is the standard explanation of how to run the config in a Docker container.

## Halon configuration template (container)

### Getting started

1. Install [Docker Desktop](https://www.docker.com/products/docker-desktop)
2. Install [Remote - Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) extension
3. Set the `HALON_REPO_USER` and `HALON_REPO_PASS` environment variables or adjust the build arguments in `.devcontainer/devcontainer.json`
4. [Reopen this folder in the container](https://code.visualstudio.com/docs/remote/containers#_quick-start-open-an-existing-folder-in-a-container)

### Useful information

* This docker container is not configured to be production-ready and should only be used during development
* By default port 25 will be forwarded from the container to the local machine, click on the *PORTS* tab to see which randomized destination port you should use to connect to it
* If you need to restart `smtpd` after building a new startup configuration (`smtpd.yaml`) you can run `supervisorctl restart smtpd`
* If you need to reload `smtpd` after building a new running configuration (`smtpd-app.yaml`) you can run `halonctl config reload`
* If you need to see the text logs you can run `supervisorctl tail -f smtpd stderr`
