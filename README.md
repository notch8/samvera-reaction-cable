 # Using React in a Rails 5 Samvera app
 
 ### Review starting place
   - Using Hyrax Template from samvera on Github
   - [https://github.com/samvera/hyrax](https://github.com/samvera/hyrax)
 
 ### Goals
   - React based autocomplete (type ahead) for search input
 
 ### Setup
 ```
 $ rails _5.0.6_ new samvera_react -m https://raw.githubusercontent.com/samvera/hyrax/v2.0.0.rc2/template.rb
 $ echo "gem 'webpacker', '~> 3.0'" >> Gemfile
 $ echo "gem 'webpacker-react', '~> 0.3.2'" >> Gemfile
 $ bundle exec rails webpacker:install
 $ bundle exec rails hydra:server
 ```

** Note: ** This configuration is the master branch

### Setup Webpack and Babel
#### .babelrc
```javascript
{
  "presets": [
    ["env", {
      "modules": false,
      "targets": {
        "browsers": "> 1%",
        "uglify": true
      },
      "useBuiltIns": true
    }],
    "stage-0",
    "react"
  ],
  "plugins": [
    "syntax-dynamic-import",
    "transform-object-rest-spread",
    ["transform-class-properties", { "spec": true }]
  ]
}
```
Add "statge-0" and "react" presets.

Make sure we have all the NPM packages installed

```
$ yarn add babel-preset-react babel-preset-stage-0 webpacker-react react react-dom
```

Restart the server to pick up the new config.

## Copy over layout for head
```
$ cp /Users/mclark/.rvm/gems/ruby-2.4.1/gems/hyrax-2.0.0.rc2/app/views/layouts/_head_tag_content.html.erb app/views/layouts
```

## Create a LiveSearch Component
```javascript
import React, { Component } from 'react';

export default class LiveSearch extends Component {
  render() {
    return (
      <h3>Live Search</h3>
    );
  }
}
```

## Create a LiveSearch Pack
#### /app/javascript/packs/LiveSearch.js
```javascript
import LiveSearch from '../components/LiveSearch'
import WebpackerReact from 'webpacker-react'

WebpackerReact.setup({LiveSearch})
```

## add javascript_pack_tag to layout header
#### /app/views/layouts/_head_tag_content.html.erb
```html
<%= javascript_pack_tag 'LiveSearch' %>
```

## Add _search_sidebar.html.erb partial
#### /app/views/catalog/_search_sidebar.html.erb
```html
<h1>Sidebar</h1>
<%= react_component('LiveSearch') %>
```
