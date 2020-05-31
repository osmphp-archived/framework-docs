# Debugging Webpack Scripts

1. Only scripts starting with `node` can be debugged.
2. `%NODE_DEBUG_OPTION%` placeholder should be added just after `node` (I guess, `$NODE_DEBUG_OPTION` on other platforms).

In case of WebPack on Windows, just after

    "testing-watch": "webpack --config vendor/manadev/shop/src/Framework/WebPack/webpack.config.js --env.APP_ENV=testing --watch"

add a script entry to be used for debugging:

    "testing-watch-debug": "node %NODE_DEBUG_OPTION% node_modules/webpack/bin/webpack.js --config vendor/manadev/shop/src/Framework/WebPack/webpack.config.js --env.APP_ENV=testing --watch"

