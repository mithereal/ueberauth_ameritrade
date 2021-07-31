# Überauth Ameritrade

> Ameritrade OAuth2 strategy for Überauth.

## Installation

1. Setup your application at [Ameritrade Developers](https://developer.tdameritrade.com/user/me/apps).

1. Add `:ueberauth_ameritrade` to your list of dependencies in `mix.exs`:

    ```elixir
    def deps do
      [{:ueberauth_ameritrade, "~> 0.1"}]
    end
    ```

1. Add the strategy to your applications:

    ```elixir
    def application do
      [applications: [:ueberauth_ameritrade]]
    end
    ```

1. Add Ameritrade to your Überauth configuration:

    ```elixir
    config :ueberauth, Ueberauth,
      providers: [
        ameritrade: {Ueberauth.Strategy.Ameritrade, []}
      ]
    ```

1.  Update your provider configuration:

    ```elixir
    config :ueberauth, Ueberauth.Strategy.Ameritrade.OAuth,
      client_id: System.get_env("AMERITRADE_KEY")
    ```

1.  Include the Überauth plug in your controller:

    ```elixir
    defmodule MyApp.AuthController do
      use MyApp.Web, :controller
      plug Ueberauth
      ...
    end
    ```

1.  Create the request and callback routes if you haven't already:

    ```elixir
    scope "/auth", MyApp do
      pipe_through :browser

      get "/:provider", AuthController, :request
      get "/:provider/callback", AuthController, :callback
    end
    ```

    And make sure to set the correct redirect URI(s) in your Ameritrade application to wire up the callback.

1. Your controller needs to implement callbacks to deal with `Ueberauth.Auth` and `Ueberauth.Failure` responses.

For an example implementation see the [Überauth Example](https://github.com/ueberauth/ueberauth_example) application.

## Calling

Depending on the configured url you can initialize the request through:

    /auth/td


You must use something other than Ameritrade in the callback routes, I use /auth/td see below:

```elixir
config :ueberauth, Ueberauth,
  providers: [
    ameritrade: {Ueberauth.Strategy.Ameritrade,  [request_path: "/auth/td", callback_path: "/auth/td/callback"]}
  ]
```


## License

Please see [LICENSE](https://github.com/schwarz/ueberauth_ameritrade/blob/master/LICENSE) for licensing details.
