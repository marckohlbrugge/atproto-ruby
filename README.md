# ATProto Client

Ruby client for the AT Protocol, with support for oauth/dpop authentication. It has been built and tested for bluesky but it should be agnostic of PDS. An omniauth strategy using this layer should appear soon.
The work is in progress but it should allready work and I'd be happy to have feedbacks.

### TODO
- [ ] Reject response without DPoP-Nonce header (to respect the spec)
- [ ] Try to reuse nonce better (currently we double each request to refresh it from the server each time)
- [ ] Give a better API (it changes at a hight rate currently)


## Installation

Add this line to your application's Gemfile:

```ruby
gem 'atproto_client'
```

## Usage

```ruby

# Initialize a client
client = AtProto::Client.new(
  access_token: access_token, 
  refresh_token: refresh_token,
  private_key: private_key_used_for_access_token_creation, 
  refresh_token_url: "https://the-token-server.com" # optional, defaults do https://bsky.social
)

# Should be able to fetch collections
client.request(
  :get,
  "#{PDS_URL}/xrpc/#{lexicon}",
  params:{ repo: "did:therepodid", collection: "app.bsky.feed.post"}
)

# Also gives a handful DPOP handler for any request (here an omniauth example)
dpop_handler = AtProto::DpopHandler.new(options.dpop_private_key)
response = @dpop_handler.make_request(
  token_url,
  :post,
  headers: { "Content-Type" => "application/json", "Accept" => "application/json" },
  body: token_params
)

# you can then use the access_token from the response in conjunction with the same private_key

```

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests.

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/lasercats/atproto-ruby.

## License

The gem is available as open source under the terms of the MIT License.
