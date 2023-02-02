# Rails 7 Error Reproduction 

## Steps

- Install Ruby 3.2.0
- bundle install
- DATABASE_URL="mysql2://user:my_weird^password@mysql:3306/quepid" bin/rails server 
- Load http://localhost:3000 in browser

## Expected

Rails should likely error because `^` is an invalid URI character.

## Actual

The following is output in logs (without filtering of the faulty URI which contains a password, albeit a URI-invalid one): 

```
2023-02-01 18:46:27 -0800 Rack app ("GET /" - (::1)): #<URI::InvalidURIError: bad URI(is not URI?): mysql2://user:my_weird^password@mysql:3306/quepid>
```

<details>
  <summary> Full stacktrace displayed in browser
  </summary>

<pre>
Puma caught this error: bad URI(is not URI?): mysql2://user:my_weird^password@mysql:3306/quepid (URI::InvalidURIError)
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/3.2.0/uri/rfc2396_parser.rb:176:in `split'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/3.2.0/uri/rfc2396_parser.rb:210:in `parse'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activerecord-7.0.4.2/lib/active_record/database_configurations/connection_url_resolver.rb:27:in `initialize'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activerecord-7.0.4.2/lib/active_record/database_configurations/url_config.rb:48:in `new'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activerecord-7.0.4.2/lib/active_record/database_configurations/url_config.rb:48:in `build_url_hash'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activerecord-7.0.4.2/lib/active_record/database_configurations/url_config.rb:38:in `initialize'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activerecord-7.0.4.2/lib/active_record/database_configurations.rb:246:in `new'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activerecord-7.0.4.2/lib/active_record/database_configurations.rb:246:in `environment_url_config'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activerecord-7.0.4.2/lib/active_record/database_configurations.rb:161:in `build_configs'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activerecord-7.0.4.2/lib/active_record/database_configurations.rb:20:in `initialize'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activerecord-7.0.4.2/lib/active_record/core.rb:51:in `new'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activerecord-7.0.4.2/lib/active_record/core.rb:51:in `configurations='
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activerecord-7.0.4.2/lib/active_record/core.rb:53:in `block in <module:Core>'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activesupport-7.0.4.2/lib/active_support/concern.rb:136:in `class_eval'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activesupport-7.0.4.2/lib/active_support/concern.rb:136:in `append_features'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activerecord-7.0.4.2/lib/active_record/base.rb:299:in `include'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activerecord-7.0.4.2/lib/active_record/base.rb:299:in `<class:Base>'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activerecord-7.0.4.2/lib/active_record/base.rb:282:in `<module:ActiveRecord>'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activerecord-7.0.4.2/lib/active_record/base.rb:15:in `<main>'
<internal:/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/site_ruby/3.2.0/rubygems/core_ext/kernel_require.rb>:37:in `require'
<internal:/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/site_ruby/3.2.0/rubygems/core_ext/kernel_require.rb>:37:in `require'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/bootsnap-1.16.0/lib/bootsnap/load_path_cache/core_ext/kernel_require.rb:32:in `require'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/zeitwerk-2.6.6/lib/zeitwerk/kernel.rb:38:in `require'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activerecord-7.0.4.2/lib/active_record/query_cache.rb:36:in `run'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activesupport-7.0.4.2/lib/active_support/execution_wrapper.rb:29:in `before'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activesupport-7.0.4.2/lib/active_support/callbacks.rb:423:in `block in make_lambda'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activesupport-7.0.4.2/lib/active_support/callbacks.rb:199:in `block (2 levels) in halting'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activesupport-7.0.4.2/lib/active_support/callbacks.rb:687:in `block (2 levels) in default_terminator'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activesupport-7.0.4.2/lib/active_support/callbacks.rb:686:in `catch'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activesupport-7.0.4.2/lib/active_support/callbacks.rb:686:in `block in default_terminator'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activesupport-7.0.4.2/lib/active_support/callbacks.rb:200:in `block in halting'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activesupport-7.0.4.2/lib/active_support/callbacks.rb:595:in `block in invoke_before'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activesupport-7.0.4.2/lib/active_support/callbacks.rb:595:in `each'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activesupport-7.0.4.2/lib/active_support/callbacks.rb:595:in `invoke_before'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activesupport-7.0.4.2/lib/active_support/callbacks.rb:106:in `run_callbacks'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activesupport-7.0.4.2/lib/active_support/execution_wrapper.rb:129:in `run'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activesupport-7.0.4.2/lib/active_support/execution_wrapper.rb:125:in `run!'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activesupport-7.0.4.2/lib/active_support/execution_wrapper.rb:78:in `block in run!'
<internal:kernel>:90:in `tap'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/activesupport-7.0.4.2/lib/active_support/execution_wrapper.rb:75:in `run!'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/actionpack-7.0.4.2/lib/action_dispatch/middleware/executor.rb:12:in `call'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/actionpack-7.0.4.2/lib/action_dispatch/middleware/static.rb:23:in `call'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/rack-2.2.6.2/lib/rack/sendfile.rb:110:in `call'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/actionpack-7.0.4.2/lib/action_dispatch/middleware/host_authorization.rb:137:in `call'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/railties-7.0.4.2/lib/rails/engine.rb:530:in `call'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/puma-5.6.5/lib/puma/configuration.rb:252:in `call'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/puma-5.6.5/lib/puma/request.rb:77:in `block in handle_request'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/puma-5.6.5/lib/puma/thread_pool.rb:340:in `with_force_shutdown'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/puma-5.6.5/lib/puma/request.rb:76:in `handle_request'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/puma-5.6.5/lib/puma/server.rb:443:in `process_client'
/Users/olivierlacan/.rbenv/versions/3.2.0/lib/ruby/gems/3.2.0/gems/puma-5.6.5/lib/puma/thread_pool.rb:147:in `block in spawn_thread'
</pre>
</details>

