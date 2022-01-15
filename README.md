Krakend HTTP Cache
====

A cached http client for the [KrakenD](github.com/devopsfaith/krakend) framework

## Using it

This package exposes two simple factories capable to create a instances of the `proxy.HTTPClientFactory` and the `proxy.BackendFactory` interfaces, respectively, embedding an in-memory-cached http client using the package [github.com/albatarnik/httpcache](https://github.com/albatarnik/httpcache). The client will cache the responses honoring the defined Cache HTTP header.

	import 	(
		"context"
		"net/http"
		"github.com/luraproject/lura/config"
		"github.com/luraproject/lura/proxy"
		"github.com/devopsfaith/krakend-httpcache"
	)

	requestExecutorFactory := func(cfg *config.Backend) proxy.HTTPRequestExecutor {
		clientFactory := httpcache.NewHTTPClient(cfg)
		return func(ctx context.Context, req *http.Request) (*http.Response, error) {
			return clientFactory(ctx).Do(req.WithContext(ctx))
		}
	}

You can create your own proxy.HTTPRequestExecutor and inject it into your BackendFactory