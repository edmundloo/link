# plaid-link with Signals

This is a simple web app that uses [Negroni][1] and [plaid-go][2] to demonstrate a client and server-side integration of [Plaid Link][3].

## Getting Started

`go get` the repository:

```console
$ go get -u github.com/plaid/link
```

`go get` will complain about `no buildable Go source files`, but you can safely ignore this.

Then:

```console
$ cd $GOPATH/src/github.com/plaid/link/examples/go && go get
```

Build the binary:

```console
$ go build -o go-example
```

## Set your `public_key`

The Plaid Link integration lives in `/public/index.html`.  You **must** edit this file to replace `{YOUR_PUBLIC_KEY}` with your actual `public_key` from the [Plaid dashboard][4] if you wish to test with real accounts.

If you only wish to test with Plaid [sandbox accounts][5], you may skip this step.

## Running the server

The entry point for the server is `./server.go`.  Required configuration options are passed in via environment variables:

- `APP_PORT`: the port on which the server should listen
- `PLAID_CLIENT_ID` and `PLAID_SECRET`: the client id and secret associated with your Plaid account, used to make authenticated Plaid API requests on the server

For example, the command below starts the server with the sandbox client_id and secret on port 8000:

```console
$ APP_PORT=8000 PLAID_CLIENT_ID=test_id PLAID_SECRET=test_secret ./go-example
```

**Note:** To test with non-sandbox accounts, simply replace `test_id` and `test_secret` with your `client_id` and `secret`, which can be found on the [Plaid dashboard][4].

Then load up <http://localhost:8000>!

And that's it!

[1]: http://github.com/codegangsta/negroni
[2]: https://github.com/plaid/plaid-go
[3]: https://github.com/plaid/link
[4]: https://plaid.com/account/
[5]: https://plaid.com/docs#sandbox
___

# Signals: Song/Transaction Comparisons

## Rundown

The purpose of this code is to determine a song that most closely matches your spending habits. By opening the app and logging in to the bank, the app will pull the amounts for your past financial transactions. These transactions will be used to form a signal represented as a series of coordinates on the Cartesian plane. Samples will be taken from a number of songs stored locally and the amplitudes of the songs will be sampled and also represented as a series of coordinates. The Fréchet distance algorithm is then used to compare your transactions to a set of song samples to determine which song most closely matches your transactions! A web page then gives you the option to play your song and shows you a graph of your transaction and song signals.

## Getting Started

Just run the server as indicated above and log into your bank account (or use the sample one)! :)

## Using your own music

You need to use your own music to play a song from the web interface. Else, the web interface will just give you a song and the graph for the signal and will be unable to play the actual song.

Make sure youre music is in `.wav` format and move your songs to the `./public/songs` (create it if it doesn't exist) directory. After doing this, update the `songsDirectory` variable in `./server.go` to reflect your full path to the songs folder, i.e. `"/Users/edmundloo/Work/src/github.com/plaid/link/examples/go/public/songs"`. After doing this, uncomment the `storeAllSignals` function in `./server.go` and hit the `/transactions` endpoint by trying to view your song comparison results. Make sure to comment out `storeAllSignals` afterwards as you only need to run this to update your song database.

## Details and Difficulties
- Songs must be converted to `.wav` if not already in `.wav`
- Due to algorithm runtime, only 50 transactions and samples are used for the comparison by default
- Your latest 50 transaction and a sample per second in the first 50 seconds of the song is the default setting

## Screenshot
<img width="1059" alt="screen shot 2017-01-26 at 14 31 11" src="https://cloud.githubusercontent.com/assets/5935208/22353769/f0422776-e3d6-11e6-8263-5444d98bf6c6.png">
