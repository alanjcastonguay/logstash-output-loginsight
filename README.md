# logstash-output-loginsight

This is a plugin for [Logstash](https://github.com/elastic/logstash), sending events to [VMware vRealize Log Insight](https://www.vmware.com/support/pubs/log-insight-pubs.html)

It is fully free and fully open source. The license is Apache 2.0, meaning you are pretty much free to use it however you want in whatever way.

## Documentation

Logstash provides infrastructure to automatically generate documentation for this plugin. We use the asciidoc format to write documentation so any comments in the source code will be first converted into asciidoc and then into html. All plugin documentation are placed under one [central location](http://www.elastic.co/guide/en/logstash/current/).

- For formatting code or config example, you can use the asciidoc `[source,ruby]` directive
- For more asciidoc formatting tips, see the excellent reference here https://github.com/elastic/docs#asciidoc-guide

## Need Help?

Need help? Try #logstash on freenode IRC or the https://discuss.elastic.co/c/logstash discussion forum.

## Developing

### 1. Plugin Developement and Testing

#### Code
- To get started, you'll need JRuby with the Bundler gem installed.

- Create a new plugin or clone and existing from the GitHub [logstash-plugins](https://github.com/logstash-plugins) organization. We also provide [example plugins](https://github.com/logstash-plugins?query=example).

- Install dependencies
```sh
bundle install
```

#### Test

- Update your dependencies

```sh
bundle install
```

- Run tests

```sh
bundle exec rspec
```

### 2. Running your unpublished Plugin in Logstash

#### 2.1 Run in a local Logstash clone

- Edit Logstash `Gemfile` and add the local plugin path, for example:
```ruby
gem "logstash-filter-awesome", :path => "/your/local/logstash-filter-awesome"
```
- Install plugin
```sh
bin/logstash-plugin install --no-verify
```
- Run Logstash with your plugin
```sh
bin/logstash -e 'filter {awesome {}}'
```
At this point any modifications to the plugin code will be applied to this local Logstash setup. After modifying the plugin, simply rerun Logstash.

#### 2.2 Run in an installed Logstash

You can use the same **2.1** method to run your plugin in an installed Logstash by editing its `Gemfile` and pointing the `:path` to your local plugin development directory. Or you can build the gem and install it using:

- Build your plugin gem
```sh
gem build logstash-output-loginsight.gemspec
```
- Install the plugin from the Logstash home
```sh
bin/logstash-plugin install /your/local/plugin/logstash-filter-loginsight.gem
```
- Start Logstash and proceed to test the plugin with http connection to loginsight
```sh
bin/logstash -e 'input { stdin { add_field => { "fieldname" => "10" } } } output { loginsight { host => ["10.11.12.13"] proto => ["http"] port => [9000] } }' --log.level=debug
```
- Start Logstash and proceed to test the plugin with https connection to loginsight using default Certificate Authority
```sh
bin/logstash -e 'input { stdin { add_field => { "fieldname" => "10" } } } output { loginsight { host => ["10.11.12.13"] verify => [true] } }' --log.level=debug
```
- Start Logstash and proceed to test the plugin with https connection to loginsight using path to certificate chain in .pem file
```sh
bin/logstash -e 'input { stdin { add_field => { "fieldname" => "10" } } } output { loginsight { host => ["10.11.12.13"] verify => [true] ca_file => ["/Path to PEM/certificate.pem"] } }' --log.level=debug
```
- How to download the certificate chain .pem file

- In case of a certificate authority signed certificate
```sh
openssl s_client -connect 10.11.12.13:9543 -verify 1
```
- copy the contents of all the sections inside -----BEGIN CERTIFICATE----- and -----END CERTIFICATE----- (both sections inclusive) and save it in certificate.pem file

- In case of a self signed certificate
```sh
openssl s_client -showcerts -connect 10.11.12.13:9543 < /dev/null | openssl x509 -outform PEM > certificate.pem
```

## Contributing

All contributions are welcome: ideas, patches, documentation, bug reports, complaints, and even something you drew up on a napkin.
